---
title: 运行状况服务故障
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: 11af69d1c6f32205b87ad4605edebacb59b0b710
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369709"
---
# <a name="health-service-faults"></a>运行状况服务故障
> 适用于：Windows Server 2019、Windows Server 2016

## <a name="what-are-faults"></a>什么是故障

运行状况服务不断监视存储空间直通群集，以检测问题并生成 "故障"。 一个新 cmdlet 显示所有当前错误，使你可以轻松地验证部署的运行状况，而无需依次查看每个实体或功能。 故障的描述非常精确、易于理解，且可操作。  

每个故障都包含五个重要字段：  

-   severity
-   问题的描述
-   用于解决问题的建议后续步骤
-   故障实体的标识信息
-   其物理位置（如果适用）

例如，以下是一个典型的故障：  

```
Severity: MINOR                                         
Reason: Connectivity has been lost to the physical disk.                           
Recommendation: Check that the physical disk is working and properly connected.    
Part: Manufacturer Contoso, Model XYZ9000, Serial 123456789                        
Location: Seattle DC, Rack B07, Node 4, Slot 11
```

 >[!NOTE]
 > 物理位置源自故障域配置。 有关容错域的详细信息，请参阅[Windows Server 2016 中的容错域](fault-domains.md)。 如果未提供此信息，位置字段将不那么有用 - 例如，它可能仅显示插槽编号。  

## <a name="root-cause-analysis"></a>根本原因分析

运行状况服务可以评估发生错误的实体之间的潜在因果关系，以确定并合并导致相同基本问题的错误。 通过识别作用链，可降低报告的繁琐性。 例如，如果服务器停机，则该服务器中的任何驱动器都应该不会连接。 因此，根本原因（在本例中为服务器）将只引发一次错误。  

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

若要查看 PowerShell 中的任何当前错误，请运行以下 cmdlet：

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

这会返回影响整体存储空间直通群集的任何故障。 大多数情况下，这些故障与硬件或配置相关。 如果没有故障，此 cmdlet 将不返回任何内容。  

>[!NOTE]
> 在非生产环境中，你可以通过自行触发故障来试验这项功能，例如，删除一个物理磁盘或关闭一个节点。 出现错误后，重新插入物理磁盘或重新启动节点，故障将再次消失。

你还可以查看只影响特定卷或文件共享的错误，这些错误只影响以下 cmdlet：  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

这会返回仅影响特定卷或文件共享的任何错误。 大多数情况下，这些故障与容量规划、数据复原或功能（例如存储服务质量或存储副本）相关。 

## <a name="usage-in-net-and-c"></a>.NET 和中的用法C#

### <a name="connect"></a>连接

若要查询运行状况服务，需要建立与群集的**CimSession** 。 为此，你将需要一些仅适用于完整 .NET 的功能，这意味着你无法直接从 web 或移动应用程序中执行此操作。 这些代码示例将使用 C @ no__t，这是此数据访问层最简单的选择。

``` 
...
using System.Security;
using Microsoft.Management.Infrastructure;

public CimSession Connect(string Domain = "...", string Computer = "...", string Username = "...", string Password = "...")
{
    SecureString PasswordSecureString = new SecureString();
    foreach (char c in Password)
    {
        PasswordSecureString.AppendChar(c);
    }

    CimCredential Credentials = new CimCredential(
        PasswordAuthenticationMechanism.Default, Domain, Username, PasswordSecureString);
    WSManSessionOptions SessionOptions = new WSManSessionOptions();
    SessionOptions.AddDestinationCredentials(Credentials);
    Session = CimSession.Create(Computer, SessionOptions);
    return Session;
}
```

提供的用户名应该是目标计算机的本地管理员。

建议你实时直接从用户输入构造密码**SecureString** ，因此其密码绝不会以明文形式存储在内存中。 这有助于缓解各种安全问题。 但实际上，对其进行构造是出于原型的目的。

### <a name="discover-objects"></a>发现对象

建立**CimSession**后，可以在群集上查询 WINDOWS MANAGEMENT INSTRUMENTATION （WMI）。

你需要获取多个相关对象的实例，然后才能获取错误或度量值。 首先， **MSFT @ no__t-1StorageSubSystem**表示群集上存储空间直通。 使用它，你可以获取群集中的每个**msft @ no__t-1StorageNode** ，以及每个**msft @ no__t-3Volume**，这些数据卷。 最后，还需要**MSFT @ no__t-1StorageHealth**，运行状况服务本身。

```
CimInstance Cluster;
List<CimInstance> Nodes;
List<CimInstance> Volumes;
CimInstance HealthService;

public void DiscoverObjects(CimSession Session)
{
    // Get MSFT_StorageSubSystem for Storage Spaces Direct
    Cluster = Session.QueryInstances(@"root\microsoft\windows\storage", "WQL", "SELECT * FROM MSFT_StorageSubSystem")
        .First(Instance => (Instance.CimInstanceProperties["FriendlyName"].Value.ToString()).Contains("Cluster"));

    // Get MSFT_StorageNode for each cluster node
    Nodes = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToStorageNode", null, "StorageSubSystem", "StorageNode").ToList();

    // Get MSFT_Volumes for each data volume
    Volumes = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToVolume", null, "StorageSubSystem", "Volume").ToList();

    // Get MSFT_StorageHealth itself
    HealthService = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToStorageHealth", null, "StorageSubSystem", "StorageHealth").First();
}
```

这些对象是在 PowerShell 中使用**StorageSubSystem**、 **StorageNode**和**Volume**等 cmdlet 获取的相同对象。

可以访问[存储管理 API 类](https://msdn.microsoft.com/library/windows/desktop/hh830612(v=vs.85).aspx)中所述的所有相同属性。

```
...
using System.Diagnostics;

foreach (CimInstance Node in Nodes)
{
    // For illustration, write each node's Name to the console. You could also write State (up/down), or anything else!
    Debug.WriteLine("Discovered Node " + Node.CimInstanceProperties["Name"].Value.ToString());
}
```

### <a name="query-faults"></a>查询错误

调用 "**诊断**" 以获取范围为目标**CimInstance**的任何当前错误，即群集或任何卷。

下面介绍了 Windows Server 2016 中每个范围内可用的错误的完整列表。

```       
public void GetFaults(CimSession Session, CimInstance Target)
{
    // Set Parameters (None)
    CimMethodParametersCollection FaultsParams = new CimMethodParametersCollection();
    // Invoke API
    CimMethodResult Result = Session.InvokeMethod(Target, "Diagnose", FaultsParams);
    IEnumerable<CimInstance> DiagnoseResults = (IEnumerable<CimInstance>)Result.OutParameters["DiagnoseResults"].Value;
    // Unpack
    if (DiagnoseResults != null)
    {
        foreach (CimInstance DiagnoseResult in DiagnoseResults)
        {
            // TODO: Whatever you want!
        }
    }
}
```

### <a name="optional-myfault-class"></a>可选：MyFault 类

构造和保留自己的错误表示可能非常有意义。 例如，此**MyFault**类存储错误的几个关键属性，其中包括**FaultId**，稍后可将其用于关联 update 或 remove 通知，或者在多次检测到相同错误的情况下使用删除重复。出于任何原因。

```       
public class MyFault {
    public String FaultId { get; set; }
    public String Reason { get; set; }
    public String Severity { get; set; }
    public String Description { get; set; }
    public String Location { get; set; }

    // Constructor
    public MyFault(CimInstance DiagnoseResult)
    {
        CimKeyedCollection<CimProperty> Properties = DiagnoseResult.CimInstanceProperties;
        FaultId     = Properties["FaultId"                  ].Value.ToString();
        Reason      = Properties["Reason"                   ].Value.ToString();
        Severity    = Properties["PerceivedSeverity"        ].Value.ToString();
        Description = Properties["FaultingObjectDescription"].Value.ToString();
        Location    = Properties["FaultingObjectLocation"   ].Value.ToString();
    }
}
```

```
List<MyFault> Faults = new List<MyFault>;

foreach (CimInstance DiagnoseResult in DiagnoseResults)
{
    Faults.Add(new Fault(DiagnoseResult));
}
```

下面介绍了每个错误（**DiagnoseResult**）中属性的完整列表。

### <a name="fault-events"></a>错误事件

创建、删除或更新错误时，运行状况服务会生成 WMI 事件。 这对于在不频繁轮询的情况下保持应用程序状态保持同步非常重要，例如，有助于确定何时发送电子邮件警报。 为了订阅这些事件，此示例代码再次使用观察程序设计模式。

首先，订阅**MSFT @ no__t-1StorageFaultEvent**事件。

```      
public void ListenForFaultEvents()
{
    IObservable<CimSubscriptionResult> Events = Session.SubscribeAsync(
        @"root\microsoft\windows\storage", "WQL", "SELECT * FROM MSFT_StorageFaultEvent");
    // Subscribe the Observer
    FaultsObserver<CimSubscriptionResult> Observer = new FaultsObserver<CimSubscriptionResult>(this);
    IDisposable Disposeable = Events.Subscribe(Observer);
}   
```

接下来，实现将在每次生成新事件时调用**OnNext （）** 方法的观察程序。

每个事件都包含**ChangeType** ，用于指示是否正在创建、删除或更新错误以及相关**FaultId**。

此外，它们还包含错误本身的所有属性。

```
class FaultsObserver : IObserver
{
    public void OnNext(T Event)
    {
        // Cast
        CimSubscriptionResult SubscriptionResult = Event as CimSubscriptionResult;

        if (SubscriptionResult != null)
        {
            // Unpack            
            CimKeyedCollection<CimProperty> Properties = SubscriptionResult.Instance.CimInstanceProperties;
            String ChangeType = Properties["ChangeType"].Value.ToString();
            String FaultId = Properties["FaultId"].Value.ToString();

            // Create
            if (ChangeType == "0")
            {
                Fault MyNewFault = new MyFault(SubscriptionResult.Instance);
                // TODO: Whatever you want!
            }
            // Remove
            if (ChangeType == "1")
            {
                // TODO: Use FaultId to find and delete whatever representation you have...
            }
            // Update
            if (ChangeType == "2")
            {
                // TODO: Use FaultId to find and modify whatever representation you have...
            }
        }
    }
    public void OnError(Exception e)
    {
        // Handle Exceptions
    }
    public void OnCompleted()
    {
        // Nothing
    }
}
```

### <a name="understand-fault-lifecycle"></a>了解故障生命周期

故障不会被标记为 "已查看" 或由用户解决。 它们是在运行状况服务观察到问题时创建的，并且仅当运行状况服务无法再发现问题时，才会被自动删除。 通常，这反映了问题已修复。

但是，在某些情况下，运行状况服务重新发现可能会导致故障（例如，故障转移后或由于间歇性连接等）。 出于此原因，保留自己的错误表示可能有意义，因此可以轻松删除重复。 如果发送电子邮件警报或等效项，则这一点特别重要。

### <a name="properties-of-faults"></a>错误属性

此表显示了错误对象的几个关键属性。 对于完整的架构，请在*storagewmi*中检查**MSFT @ no__t 1StorageDiagnoseResult**类。

| **Property**              | **示例**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | FaultType （正常）                      |
| Reason                    | "卷的可用空间不足。"                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N。 123456789                                  |
| FaultingObjectLocation    | 机架 A06，RU 25，槽11                                        |
| RecommendedActions        | {"展开卷。"，"将工作负荷迁移到其他卷"。   |

**FaultId**在一个群集范围内是唯一的。

**PerceivedSeverity**PerceivedSeverity = {4，5，6} = {"信息"、"警告" 和 "错误"} 或等效的颜色，如蓝色、黄色和红色。

**FaultingObjectDescription**硬件的部分信息，通常为空白。

**FaultingObjectLocation**硬件的位置信息，通常为空白（对于软件对象）。

**RecommendedActions**建议的操作的列表，这些操作是独立的，不是特定的顺序。 现在，此列表的长度通常为1。

## <a name="properties-of-fault-events"></a>错误事件的属性

此表显示了错误事件的几个关键属性。 对于完整的架构，请在*storagewmi*中检查**MSFT @ no__t 1StorageFaultEvent**类。

请注意**ChangeType**，它指示是否正在创建、删除或更新错误，以及**FaultId**。 事件还包含受影响的错误的所有属性。

| **Property**              | **示例**                                                     |
|---------------------------|-----------------------------------------------------------------|
| ChangeType                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | FaultType （正常）                      |
| Reason                    | "卷的可用空间不足。"                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N。 123456789                                  |
| FaultingObjectLocation    | 机架 A06，RU 25，槽11                                        |
| RecommendedActions        | {"展开卷。"，"将工作负荷迁移到其他卷"。   |

**ChangeType**ChangeType = {0，1，2} = {"创建"，"删除"，"更新"}。

## <a name="coverage"></a>覆盖范围

在 Windows Server 2016 中，运行状况服务提供以下故障范围：  

### <a name="physicaldisk-8"></a>**PhysicalDisk （8）**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>FaultTypeFaultType. PhysicalDisk. FailedMedia
* 严重性：警告
* 原因： *"物理磁盘出现故障。"*
* RecommendedAction: *"替换物理磁盘。"*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>FaultTypeFaultType. PhysicalDisk. LostCommunication
* 严重性：警告
* 原因： *"物理磁盘的连接已丢失。"*
* RecommendedAction: *"检查物理磁盘是否正常运行且已正确连接。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>FaultTypeFaultType. PhysicalDisk。
* 严重性：警告
* 原因： *"物理磁盘会定期无响应。"*
* RecommendedAction: *"替换物理磁盘。"*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>FaultTypeFaultType. PhysicalDisk. PredictiveFailure
* 严重性：警告
* 原因： *"物理磁盘出现故障，很快就会发生。"*
* RecommendedAction: *"替换物理磁盘。"*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>FaultTypeFaultType. PhysicalDisk. UnsupportedHardware
* 严重性：警告
* 原因： *"物理磁盘被隔离，因为它不受解决方案供应商支持。"*
* RecommendedAction: *"将物理磁盘替换为支持的硬件"。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>FaultTypeFaultType. PhysicalDisk. UnsupportedFirmware
* 严重性：警告
* 原因： *"物理磁盘处于隔离区，因为其固件版本不受解决方案供应商支持。"*
* RecommendedAction: *"将物理磁盘上的固件更新为目标版本"。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>FaultTypeFaultType. PhysicalDisk. UnrecognizedMetadata
* 严重性：警告
* 原因： *"物理磁盘具有无法识别的元数据。"*
* RecommendedAction: *"此磁盘可能包含未知存储池中的数据。首先，请确保此磁盘上没有有用的数据，然后重置磁盘。 "*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>FaultTypeFaultType. PhysicalDisk. FailedFirmwareUpdate
* 严重性：警告
* 原因： *"尝试更新物理磁盘上的固件失败"。*
* RecommendedAction: *"尝试使用其他固件二进制文件。"*

### <a name="virtual-disk-2"></a>**虚拟磁盘（2）**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>FaultTypeFaultType. VirtualDisks. NeedsRepair
* 严重性：信息性
* 原因： *"此卷上的某些数据不能完全复原。它仍可访问。 "*
* RecommendedAction: *"还原数据的复原能力"。*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>FaultTypeFaultType. VirtualDisks。
* 严重性：关键
* 原因： *"卷不可访问。某些数据可能丢失。 "*
* RecommendedAction: *"检查所有存储设备的物理和/或网络连接。可能需要从备份还原。*

### <a name="pool-capacity-1"></a>**池容量（1）**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>FaultTypeFaultType. StoragePool. InsufficientReserveCapacityFault
* 严重性：警告
* 原因： *"存储池没有建议的最低预留容量。这可能会限制在出现驱动器故障时还原数据复原的能力。 "*
* RecommendedAction: *"将额外容量添加到存储池，或释放容量。建议的最小保留保留因部署而异，但大约为2个驱动器的容量。 "*

### <a name="volume-capacity-2sup1sup"></a>**卷容量（2）** <sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultTypeFaultType （正常）
* 严重性：警告
* 原因： *"卷的可用空间不足。"*
* RecommendedAction: *"扩展卷或将工作负荷迁移到其他卷"。*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultTypeFaultType （正常）
* 严重性：关键
* 原因： *"卷的可用空间不足。"*
* RecommendedAction: *"扩展卷或将工作负荷迁移到其他卷"。*

### <a name="server-3"></a>**服务器（3）**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>FaultTypeFaultType 已关闭。
* 严重性：关键
* 原因： *"无法连接到服务器。"*
* RecommendedAction: *"启动或替换服务器"。*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>FaultTypeFaultType （独立）
* 严重性：关键
* 原因：*由于连接问题，服务器与群集隔离。 "*
* RecommendedAction: *"如果隔离仍然存在，请检查网络或将工作负荷迁移到其他节点。"*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>FaultTypeFaultType 已隔离的
* 严重性：关键
* 原因： *"由于重复失败，该服务器已由群集隔离。"*
* RecommendedAction: *"替换服务器或修复网络"。*

### <a name="cluster-1"></a>**群集（1）**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>FaultTypeFaultType. ClusterQuorumWitness. 错误
* 严重性：关键
* 原因： *"群集是一台服务器故障。"*
* RecommendedAction: *"检查见证服务器资源，并根据需要重新启动。启动或替换失败的服务器。 "*

### <a name="network-adapterinterface-4"></a>**网络适配器/接口（4）**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>FaultTypeFaultType. 网络适配器。
* 严重性：警告
* 原因： *"网络接口已断开连接"。*
* RecommendedAction: *"重新连接网络电缆"。*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>FaultTypeFaultType. NetworkInterface。缺少
* 严重性：警告
* 原因： *"服务器 {server} 缺少连接到群集网络的网络适配器 {群集网络}"。*
* RecommendedAction: *"将服务器连接到缺少的群集网络"。*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>FaultTypeFaultType. 网络适配器. 硬件
* 严重性：警告
* 原因： *"网络接口发生硬件故障。"*
* RecommendedAction: *"替换网络接口适配器。"*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>FaultTypeFaultType. 网络适配器。
* 严重性：警告
* 原因： *"网络接口 {network interface} 未启用且未在使用中。"*
* RecommendedAction: *"启用网络接口"。*

### <a name="enclosure-6"></a>**机箱（6）**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>FaultTypeFaultType. StorageEnclosure. LostCommunication
* 严重性：警告
* 原因： *"对存储机箱的通信已丢失。"*
* RecommendedAction: *"启动或替换存储机箱。"*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>FaultTypeFaultType. StorageEnclosure. FanError
* 严重性：警告
* 原因： *"存储机箱的位置 {position} 处的风扇出现故障。"*
* RecommendedAction: *"更换存储机箱中的风扇。"*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>FaultTypeFaultType. StorageEnclosure. CurrentSensorError
* 严重性：警告
* 原因： *"存储机箱的当前传感器位置 {position} 已失败。"*
* RecommendedAction: *"替换存储机箱中的当前传感器。"*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>FaultTypeFaultType. StorageEnclosure. VoltageSensorError
* 严重性：警告
* 原因： *"存储机箱的位置 {position} 处的电压传感器出现故障。"*
* RecommendedAction: *"更换存储机箱中的电压传感器。"*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>FaultTypeFaultType. StorageEnclosure. IoControllerError
* 严重性：警告
* 原因： *"存储机箱的位置 {position} 处的 IO 控制器出现故障。"*
* RecommendedAction: *"替换存储机箱中的 IO 控制器。"*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>FaultTypeFaultType. StorageEnclosure. TemperatureSensorError
* 严重性：警告
* 原因： *"存储机箱的位置 {position} 处的温度传感器出现故障。"*
* RecommendedAction: *"更换存储机箱中的温度传感器。*

### <a name="firmware-rollout-3"></a>**固件推出（3）**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>FaultTypeFaultType. FaultDomain. FailedMaintenanceMode
* 严重性：警告
* 原因： *"目前无法在执行固件推出时进行进度"。*
* RecommendedAction: *"验证所有存储空间是否正常，以及没有容错域当前是否处于维护模式。"*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>FaultTypeFaultType. FaultDomain. FirmwareVerifyVersionFaile
* 严重性：警告
* 原因：*由于在应用固件更新后，固件回滚已被取消或意外的固件版本信息取消。 "*
* RecommendedAction: *"固件问题解决后重新启动固件推出"。*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>FaultTypeFaultType. FaultDomain. TooManyFailedUpdates
* 严重性：警告
* 原因：*由于物理磁盘太多而导致固件更新尝试失败，因此已取消固件回滚。 "*
* RecommendedAction: *"固件问题解决后重新启动固件推出"。*

### <a name="storage-qos-3sup2sup"></a>**存储 QoS （3）** <sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>FaultTypeFaultType. StorQos. InsufficientThroughput
* 严重性：警告
* 原因： *"存储吞吐量不足以满足预留。"*
* RecommendedAction: *"重新配置存储 QoS 策略"。*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>FaultTypeFaultType. StorQos. LostCommunication
* 严重性：警告
* 原因： *"存储 QoS 策略管理器已失去与该卷的通信。"*
* RecommendedAction: *"请重新启动节点 {节点}"*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>FaultTypeFaultType. StorQos. MisconfiguredFlow
* 严重性：警告
* 原因： *"一个或多个存储使用者（通常是虚拟机）正在使用 id 为 {id} 的不存在的策略。"*
* RecommendedAction: *"重新创建任何缺少的存储 QoS 策略。"*

<sup>1</sup>表示卷已达到 80% （次严重性）或 90% （主要严重性）。  
<sup>2</sup>指示卷上的某些 .vhd 未达到其最小 IOPS，超过 10% （次）、30% （主要）或 50% （严重）滚动时间为24小时。  

>[!NOTE]
> 存储机箱组件（如风扇、电源和传感器）的运行状况派生自 SCSI 机箱服务 (SES)。 如果你的供应商不提供此信息，运行状况服务不能对其进行显示。  

## <a name="see-also"></a>请参阅

- [Windows Server 2016 中的运行状况服务](health-service-overview.md)

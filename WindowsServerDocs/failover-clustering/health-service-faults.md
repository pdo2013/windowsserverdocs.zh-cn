---
title: 运行状况服务故障
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: 31a38eacea3af3c0a288d61a77a24b4fa45a1932
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843368"
---
# <a name="health-service-faults"></a>运行状况服务故障
> 适用于 Windows Server 2016

## <a name="what-are-faults"></a>故障有哪些？

运行状况服务会持续监视存储空间直通群集以检测问题并生成"故障"。 一个新的 cmdlet 显示任何当前故障，从而可以轻松地验证你的部署的运行状况，无需查看的每个实体或反过来功能。 故障的描述非常精确、易于理解，且可操作。  

每个故障包含五个重要的字段：  

-   严重性
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

运行状况服务可以评估故障以标识和合并由相同根本问题导致的错误的实体之间的潜在因果关系。 通过识别作用链，可降低报告的繁琐性。 例如，如果服务器已关闭，应不是在服务器内的任何驱动器也将断开连接。 因此的根本原因-在这种情况下，服务器都会引发一个错误。  

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

若要查看在 PowerShell 中的任何当前故障，运行此 cmdlet:

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

这会返回任何错误，这会影响总体存储空间直通群集。 大多数情况下，这些故障与硬件或配置。 如果不有任何错误，此 cmdlet 将返回执行任何操作。  

>[!NOTE]
> 在非生产环境中，并由您自己承担，您可以通过自行触发故障-例如，通过移除一个物理磁盘或关闭一个节点来体验此功能。 一旦出现故障，重新插入物理磁盘或重新启动节点，该故障会再次消失。

此外可以查看故障影响仅特定卷或文件共享，使用以下 cmdlet:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

这将返回影响仅特定卷或文件共享的任何错误。 大多数情况下，这些故障与容量规划、 数据复原或存储服务质量或存储副本等功能。 

## <a name="usage-in-net-and-c"></a>在.NET 中的使用情况和C#

### <a name="connect"></a>连接

若要查询运行状况服务，你将需要建立**CimSession**与群集。 若要执行此操作，你将需要完整的.NET 中才有一些事项，这意味着，你无法轻松地执行此操作直接从 web 或移动应用。 这些代码示例将使用 C\#、 的最直接选择将为此数据访问层。

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

提供的用户名应为目标计算机的本地管理员。

建议您构造密码**SecureString**直接从中的用户输入实时的因此其密码永远不会存储在内存中以明文形式。 这有助于缓解各种安全问题。 但在实践中，按上述方式构造是常见的原型设计目的。

### <a name="discover-objects"></a>发现对象

与**CimSession**建立，您可以在群集上查询 Windows Management Instrumentation (WMI)。

可以获取错误或度量值之前，需要获取多个相关对象的实例。 首先， **MSFT\_StorageSubSystem**它表示存储空间直通群集上。 使用的可以获取每个**MSFT\_StorageNode**在群集中，并且每个**MSFT\_卷**，数据卷。 最后，你将需要**MSFT\_StorageHealth**，运行状况服务本身，过。

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

这些是你在 PowerShell 中获取使用等 cmdlet 的相同对象**Get-storagesubsystem**， **Get-storagenode**，并**Get-volume**。

您可以访问所有相同的属性中所述[存储管理 API 类](https://msdn.microsoft.com/en-us/library/windows/desktop/hh830612(v=vs.85).aspx)。

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

调用**诊断**若要获取的目标范围内任何当前故障**CimInstance**，它是在群集或任何卷。

在 Windows Server 2016 中的每个作用域可用的错误的完整列表如下所述。

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

它可能会对你进行构造和保留自己的表示形式中的故障而言非常重要。 例如，以下**MyFault**类存储的错误，包括多个关键属性**FaultId**，可以用它更高版本将更新或删除通知，或进行重复数据删除的事件中多次，无论什么原因检测到相同的故障。

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

在每个错误中的属性的完整列表 (**DiagnoseResult**) 如下所述。

### <a name="fault-events"></a>错误事件

当错误创建、 删除，或更新时，运行状况服务会生成 WMI 事件。 这些对保持应用程序状态的同步，而无需频繁地轮询至关重要，可帮助确定何时发送电子邮件警报，例如等。 若要订阅这些事件，此代码示例使用观察程序设计模式试。

首先，订阅**MSFT\_StorageFaultEvent**事件。

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

接下来，实现观察程序其**OnNext()** 生成新事件时，将调用方法。

每个事件都包含**ChangeType** ，该值指示是否错误正在创建，将其删除，或已更新，以及相关**FaultId**。

此外，它们包含本身的错误的所有属性。

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

### <a name="understand-fault-lifecycle"></a>了解容错的生命周期

不应标记为"看到"或由用户解析错误。 当运行状况服务观察到的问题，并将自动删除且仅当运行状况服务无法再发现该问题，则创建它们。 一般情况下，这反映出的问题得到解决。

但是，在某些情况下，故障可能会被重新发现运行状况服务 （例如故障转移后，或者由于间歇性连接，等等。）。 出于此原因，则可能最好保存自己的表示形式中的故障，以便您可以轻松地重复数据删除。 这一点尤其重要，如果发送电子邮件警报或等效身份。

### <a name="properties-of-faults"></a>错误的属性

此表列出了多个关键属性的错误对象。 对于完整的架构中，检查**MSFT\_StorageDiagnoseResult**类*storagewmi.mof*。

| **属性**              | **示例**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| 原因                    | "卷可用空间不足。"                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Rack A06, RU 25, Slot 11                                        |
| RecommendedActions        | {"扩展卷。"，"工作负荷迁移到其他卷。"}   |

**FaultId**一个群集的作用域内唯一。

**PerceivedSeverity** PerceivedSeverity = {4，5，6} = {"Informational"、"警告"和"错误"} 或等效的颜色，例如蓝色、 黄色和红色。

**FaultingObjectDescription**第部分有关的硬件，通常空白表示软件对象的信息。

**FaultingObjectLocation**位置信息有关的硬件，通常空白的软件对象。

**RecommendedActions**列表的建议是独立的操作和顺序不分先后。 目前，此列表通常是长度为 1。

## <a name="properties-of-fault-events"></a>错误事件的属性

此表列出了多个关键属性的错误事件。 对于完整的架构中，检查**MSFT\_StorageFaultEvent**类*storagewmi.mof*。

请注意**ChangeType**，指示是否错误创建时，将其删除，或已更新，并**FaultId**。 事件还包含受影响的错误的所有属性。

| **属性**              | **示例**                                                     |
|---------------------------|-----------------------------------------------------------------|
| ChangeType                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| 原因                    | "卷可用空间不足。"                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Rack A06, RU 25, Slot 11                                        |
| RecommendedActions        | {"扩展卷。"，"工作负荷迁移到其他卷。"}   |

**ChangeType** ChangeType = {0，1，2} = {"创建"，"删除"，"更新"}。

## <a name="coverage"></a>覆盖范围

在 Windows Server 2016 中，运行状况服务提供了以下故障覆盖范围：  

### <a name="physicaldisk-8"></a>**PhysicalDisk (8)**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>FaultType:Microsoft.Health.FaultType.PhysicalDisk.FailedMedia
* 严重性：警告
* 原因：*"物理磁盘已失败。"*
* RecommendedAction:*"替换物理磁盘"。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>FaultType:Microsoft.Health.FaultType.PhysicalDisk.LostCommunication
* 严重性：警告
* 原因：*"连接已丢失和物理磁盘。"*
* RecommendedAction:*"检查物理磁盘是工作并已正确连接。"*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>FaultType:Microsoft.Health.FaultType.PhysicalDisk.Unresponsive
* 严重性：警告
* 原因：*"物理磁盘暴露定期无响应。"*
* RecommendedAction:*"替换物理磁盘"。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>FaultType:Microsoft.Health.FaultType.PhysicalDisk.PredictiveFailure
* 严重性：警告
* 原因：*"被预测很快就会发生故障的物理磁盘"。*
* RecommendedAction:*"替换物理磁盘"。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>FaultType:Microsoft.Health.FaultType.PhysicalDisk.UnsupportedHardware
* 严重性：警告
* 原因：*"物理磁盘被隔离，因为它不支持你解决方案的供应商。"*
* RecommendedAction:*"替换为物理磁盘支持的硬件。"*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>FaultType:Microsoft.Health.FaultType.PhysicalDisk.UnsupportedFirmware
* 严重性：警告
* 原因：*"物理磁盘是在隔离区因为其固件版本不支持你解决方案的供应商。"*
* RecommendedAction:*"更新到的目标版本的物理磁盘上的固件。"*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>FaultType:Microsoft.Health.FaultType.PhysicalDisk.UnrecognizedMetadata
* 严重性：警告
* 原因：*"物理磁盘有无法识别的元数据。"*
* RecommendedAction:*"此磁盘可能包含从未知的存储池的数据。首先请确保此磁盘上没有任何有用数据然后重设盘。"*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>FaultType:Microsoft.Health.FaultType.PhysicalDisk.FailedFirmwareUpdate
* 严重性：警告
* 原因：*"尝试失败的物理磁盘上的固件更新。"*
* RecommendedAction:*"请尝试使用不同的固件二进制"。*

### <a name="virtual-disk-2"></a>**虚拟磁盘 (2)**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>FaultType:Microsoft.Health.FaultType.VirtualDisks.NeedsRepair
* 严重性：信息性
* 原因：*"在此卷上的某些数据不是完全可复原的。它仍可访问。"*
* RecommendedAction:*"还原数据的复原能力。"*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>FaultType:Microsoft.Health.FaultType.VirtualDisks.Detached
* 严重性：严重
* 原因：*"卷不可访问。某些数据可能会丢失。"*
* RecommendedAction:*"检查物理和/或网络连接的所有存储设备。您可能需要从备份中还原。"*

### <a name="pool-capacity-1"></a>**池容量 (1)**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>FaultType:Microsoft.Health.FaultType.StoragePool.InsufficientReserveCapacityFault
* 严重性：警告
* 原因：*"存储池没有最小建议的保留容量。这可能会限制你能够还原数据时的驱动器故障还原能力。"*
* RecommendedAction:*"添加到存储池的更多的容量或释放的容量。最小建议保留因部署，但容量的大约 2 个驱动器的价值。"*

### <a name="volume-capacity-2sup1sup"></a>**卷容量 (2)**<sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType:Microsoft.Health.FaultType.Volume.Capacity
* 严重性：警告
* 原因：*"卷可用空间不足。"*
* RecommendedAction:*"扩展卷或将工作负荷迁移到其他卷。"*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType:Microsoft.Health.FaultType.Volume.Capacity
* 严重性：严重
* 原因：*"卷可用空间不足。"*
* RecommendedAction:*"扩展卷或将工作负荷迁移到其他卷。"*

### <a name="server-3"></a>**Server (3)**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>FaultType:Microsoft.Health.FaultType.Server.Down
* 严重性：严重
* 原因：*"无法连接到服务器"。*
* RecommendedAction:*"启动或替换服务器"。*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>FaultType:Microsoft.Health.FaultType.Server.Isolated
* 严重性：严重
* 原因：*"服务器是独立于连接问题所导致的群集"。*
* RecommendedAction:*"如果隔离仍然存在，检查多个网络或工作负荷迁移到其他节点。"*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>FaultType:Microsoft.Health.FaultType.Server.Quarantined
* 严重性：严重
* 原因：*"服务器隔离的定期故障导致群集中"。*
* RecommendedAction:*"替换服务器或修复网络"。*

### <a name="cluster-1"></a>**群集 (1)**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>FaultType:Microsoft.Health.FaultType.ClusterQuorumWitness.Error
* 严重性：严重
* 原因：*"群集是离开停运的一次服务器失败。"*
* RecommendedAction:*"检查见证资源，并根据需要重新启动。启动或替换发生故障的服务器"。*

### <a name="network-adapterinterface-4"></a>**网络适配器/接口 (4)**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>FaultType:Microsoft.Health.FaultType.NetworkAdapter.Disconnected
* 严重性：警告
* 原因：*"网络接口已断开连接。"*
* RecommendedAction:*"重新连接网络电缆。"*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>FaultType:Microsoft.Health.FaultType.NetworkInterface.Missing
* 严重性：警告
* 原因：*"{Server} 的服务器为缺少的网络适配器连接到群集网络 {群集网络}。"*
* RecommendedAction:*"将服务器连接到缺失的群集网络。"*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>FaultType:Microsoft.Health.FaultType.NetworkAdapter.Hardware
* 严重性：警告
* 原因：*"网络接口已有了硬件故障。"*
* RecommendedAction:*"替换网络接口适配器"。*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>FaultType:Microsoft.Health.FaultType.NetworkAdapter.Disabled
* 严重性：警告
* 原因：*"网络接口 {网络接口} 未启用，未被使用。"*
* RecommendedAction:*"启用网络接口"。*

### <a name="enclosure-6"></a>**主机箱 (6)**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>FaultType:Microsoft.Health.FaultType.StorageEnclosure.LostCommunication
* 严重性：警告
* 原因：*"通信已丢失到存储机箱。"*
* RecommendedAction:*"启动或替换存储机箱。"*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>FaultType:Microsoft.Health.FaultType.StorageEnclosure.FanError
* 严重性：警告
* 原因：*"在位置 {位置} 的存储机箱风扇已失败。"*
* RecommendedAction:*"替换在存储机箱风扇。"*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>FaultType:Microsoft.Health.FaultType.StorageEnclosure.CurrentSensorError
* 严重性：警告
* 原因：*"在位置 {位置} 的存储机箱电流传感器已失败。"*
* RecommendedAction:*"替换在存储机箱电流传感器"。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>FaultType:Microsoft.Health.FaultType.StorageEnclosure.VoltageSensorError
* 严重性：警告
* 原因：*"在位置 {位置} 的存储机箱电压传感器已失败。"*
* RecommendedAction:*"替换电压传感器的存储机箱中。"*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>FaultType:Microsoft.Health.FaultType.StorageEnclosure.IoControllerError
* 严重性：警告
* 原因：*"在位置 {位置} 的存储机箱的 IO 控制器已失败。"*
* RecommendedAction:*"替换存储机箱中的 IO 控制器"。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>FaultType:Microsoft.Health.FaultType.StorageEnclosure.TemperatureSensorError
* 严重性：警告
* 原因：*"在位置 {位置} 的存储机箱温度传感器已失败。"*
* RecommendedAction:*"替换存储机箱中的温度传感器"。*

### <a name="firmware-rollout-3"></a>**固件推出 (3)**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>FaultType:Microsoft.Health.FaultType.FaultDomain.FailedMaintenanceMode
* 严重性：警告
* 原因：*"当前无法执行固件推出时取得进展。"*
* RecommendedAction:*"验证所有存储空间运行正常，并且任何容错域当前处于维护模式"。*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>FaultType:Microsoft.Health.FaultType.FaultDomain.FirmwareVerifyVersionFaile
* 严重性：警告
* 原因：*"固件推出已取消由于应用固件更新后不可读或意外的固件版本信息。"*
* RecommendedAction:*"重新启动固件推出后固件问题已解决。"*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>FaultType:Microsoft.Health.FaultType.FaultDomain.TooManyFailedUpdates
* 严重性：警告
* 原因：*"固件推出已取消由于太多物理磁盘固件更新尝试失败。"*
* RecommendedAction:*"重新启动固件推出后固件问题已解决。"*

### <a name="storage-qos-3sup2sup"></a>**存储 QoS (3)**<sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>FaultType:Microsoft.Health.FaultType.StorQos.InsufficientThroughput
* 严重性：警告
* 原因：*"存储吞吐量不足，无法满足预留。"*
* RecommendedAction:*"重新配置存储 QoS 策略。"*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>FaultType:Microsoft.Health.FaultType.StorQos.LostCommunication
* 严重性：警告
* 原因：*"存储 QoS 策略管理器与该卷的通信已丢失"。*
* RecommendedAction:*"请重新启动节点 {节点}"*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>FaultType:Microsoft.Health.FaultType.StorQos.MisconfiguredFlow
* 严重性：警告
* 原因：*"一个或多个存储使用者 （通常为虚拟机） 使用不存在的策略 id 为 {id}"。*
* RecommendedAction:*"重新创建缺少的任何存储 QoS 策略。"*

<sup>1</sup>指示该卷已达到 80%已满 （次要严重性） 或 90%已满 （主要严重性）。  
<sup>2</sup>指示卷上的某些.vhd(s) 尚未达到其最小 IOPS，超过 10%（次要）、 30%（主要） 或 50%（严重） 的滚动 24 小时时间窗口。  

>[!NOTE]
> 存储机箱组件（如风扇、电源和传感器）的运行状况派生自 SCSI 机箱服务 (SES)。 如果你的供应商不提供此信息，运行状况服务不能对其进行显示。  

## <a name="see-also"></a>请参阅

- [Windows Server 2016 中的运行状况服务](health-service-overview.md)

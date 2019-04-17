---
title: "运行状况服务的错误"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: c9e1fb4568ee93739c49ccc1a13106b09161c5f3
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="health-service-faults"></a>运行状况服务的错误
> 适用于 Windows Server 2016

## <a name="what-are-faults"></a>什么是错误？

运行状况服务会持续监视存储空间直通 群集检测问题和生成"错误"。 一个新 cmdlet 显示当前的任何错误，这允许你轻松地查看的每个实体无验证部署的运行状况或反过来又功能。 故障都设计为可精确、 容易理解和执行。  

每种错误包含 5 个重要的字段：  

-   严重性
-   问题描述
-   推荐的下一个步骤来解决问题
-   在故障实体识别信息
-   它的物理位置 （如果适用）

例如，以下是典型错误：  

```
Severity: MINOR                                         
Reason: Connectivity has been lost to the physical disk.                           
Recommendation: Check that the physical disk is working and properly connected.    
Part: Manufacturer Contoso, Model XYZ9000, Serial 123456789                        
Location: Seattle DC, Rack B07, Node 4, Slot 11
```

 >[!NOTE]
 > 物理位置派生自您的故障域配置。 有关故障域的详细信息，请参阅[故障域，Windows Server 2016](fault-domains.md)。 如果不提供此信息，将不有用位置字段-例如，它可能仅显示 slot 编号。  

## <a name="root-cause-analysis"></a>根本原因分析

运行状况服务可以对之间故障实体结合故障的影响的相同基础问题，找出潜在的因果评估。 通过识别链的效果，这将较少花哨的报告。 例如，如果该服务器出现故障，应比任何服务器内的驱动器也将无法连接。 因此，将为根本原因-在此情况下，服务器引发只有一个错误。  

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

若要查看在 PowerShell 中的任何当前错误，请运行此 cmdlet:

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

这将返回任何故障的影响整体存储空间直通 群集。 大多数情况下，这些故障与硬件或配置。 如果没有故障，此 cmdlet 会返回任何内容。  

>[!NOTE]
> 在非 production 环境中，并在自己的风险，可以尝试使用此功能通过自行触发故障-例如，通过拔除物理磁盘的一个或关机一个节点。 已出现故障之后, 重新插入物理磁盘或重启该节点，并故障再次将消失。

你还可以查看故障的影响只有特定卷或使用以下 cmdlet 文件共享：  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

这将返回任何影响仅的特定文件或音量共享的故障。 大多数情况下，这些故障与容量规划、数据复原或功能，如存储服务质量或存储的副本。 

## <a name="usage-in-net-and-c"></a>使用.NET 和 C#

### <a name="connect"></a>连接

为了查询运行状况服务时，你将需要建立**CimSession**与群集。 若要这样做，你将需要完全.NET 中仅有的一些操作，意味着你无法随时执行此操作直接来自 web 或移动的应用。 这些代码示例将使用 C\ #，此数据的访问权限层的最简单的选择。

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

提供的用户名应该目标计算机的本地管理员。

我们建议你构建密码**SecureString**直接从中的用户输入实时，因此他们的密码永远不会存储在内存中纯文本。 这有助于减少各种的安全问题。 但实际上，它构建与上述很常见原型供。

### <a name="discover-objects"></a>发现对象

与**CimSession**建立，你可以在群集查询 Windows Management Instrumentation (WMI)。

你可以获取故障或指标之前，你将需要获取相关的几个对象的实例。 首先，**MSFT\_StorageSubSystem**这表示存储空间直通 群集上。 使用，你可以获取每个**MSFT\_StorageNode**群集、中和每个**MSFT\_Volume**，数据量。 最后，你将需要**MSFT\_StorageHealth**，运行状况服务自行，过。

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

以下是你在 PowerShell 中获取使用等 cmdlet 相同的对象**获取 StorageSubSystem**，**获取 StorageNode**，并**获取音量**。

你可以完全相同访问属性，在记录[存储管理 API 类](https://msdn.microsoft.com/en-us/library/windows/desktop/hh830612(v=vs.85).aspx)。

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

调用**诊断**获取目标仅限于任何当前错误**CimInstance**，这会群集或任何音量。

下面介绍了 Windows Server 2016 的每个范围故障可用的完整列表。

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

它可能会让你构建并保留你自己的表示形式故障感知。 例如，这**MyFault**类存储的错误，包括几个主要属性**FaultId**，可使用稍后将更新或删除通知，或者消除的同一故障是检测到多次出于任何原因。

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

完整列表的每个错误属性 (**DiagnoseResult**) 如下所述。

### <a name="fault-events"></a>故障事件。

当故障是创建、删除或更新时，运行状况服务将生成 WMI 事件。 这些至关重要保留你的应用程序状态无频繁轮询，同步，有助于确定何时发送电子邮件通知，例如诸如与。 这些事件订阅，此的示例代码观察程序设计模式再次使用。

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

接下来，实现观察其**OnNext()**每当生成新的活动，将调用方法。

每个事件包含**误差**指示是否故障正在创建，将其删除，或已更新内容，以及相关**FaultId**。

此外，他们包含错误本身的所有属性。

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

不应将标记为"查看"或解决方法是用户故障。 当运行状况服务遵循问题，并且会自动删除它们，并仅在运行状况服务可能不会再观察问题时，它们将创建。 一般情况下，这表明问题已解决。

但是，在某些情况下，故障可能重新发现的运行状况服务（例如后故障转移，或者由于连接中断、等。）。 出于此原因，可能有意义保持你自己的表示形式错误，以便你可以轻松地消除。 这是你发送电子邮件通知或等效尤其重要。

### <a name="properties-of-faults"></a>故障的属性

此表列出了几个主要属性故障对象。 有关完整方案中，检查**MSFT\_StorageDiagnoseResult**中类*storagewmi.mof*。

| **属性**              | **示例**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| 原因                    | "音量空间已满可用。"                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | 机 A06、入 25、Slot 11                                        |
| RecommendedActions        | {"展开"音量。、"到其他卷迁移工作负载。"}   |

**FaultId**唯一一个群集的范围内。

**PerceivedSeverity** PerceivedSeverity = {4、5、6} = {"信息"、"警告，"和"错误"} 或如蓝色、黄色和红色等效颜色。

**FaultingObjectDescription**部件硬件、软件对象通常为空的信息。

**FaultingObjectLocation**位置信息的硬件，通常空白的软件物体。

**RecommendedActions**列表推荐独立的操作以及任何特定的顺序。 今天，该列表通常是长度为 1。

## <a name="properties-of-fault-events"></a>故障事件属性

此表列出了几个主要属性故障事件。 有关完整方案中，检查**MSFT\_StorageFaultEvent**中类*storagewmi.mof*。

注意**误差**，这表示是否故障创建，将其删除，或已更新内容，并**FaultId**。 一场活动还包含所有受影响的故障的属性。

| **属性**              | **示例**                                                     |
|---------------------------|-----------------------------------------------------------------|
| 误差                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| 原因                    | "音量空间已满可用。"                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | 机 A06、入 25、Slot 11                                        |
| RecommendedActions        | {"展开"音量。、"到其他卷迁移工作负载。"}   |

**误差**误差 = {0、1、2} = {"创建"、"Remove"、"更新"}。

## <a name="coverage"></a>覆盖率

在 Windows Server 2016 中，运行状况服务提供以下故障覆盖率：  

### **<a name="physicaldisk-8"></a>物理磁盘 (8)**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedMedia
* 严重性：警告
* 原因：*"失败物理磁盘。"*
* RecommendedAction: *"替换物理磁盘。"*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.LostCommunication
* 严重性：警告
* 原因：*"连接已丢失物理磁盘。"*
* RecommendedAction: *"检查物理磁盘是工作且已正确连接。"*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.Unresponsive
* 严重性：警告
* 原因：*"物理磁盘展示定期无响应。"*
* RecommendedAction: *"替换物理磁盘。"*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.PredictiveFailure
* 严重性：警告
* 原因：*"物理磁盘的失败预测即将发生。"*
* RecommendedAction: *"替换物理磁盘。"*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedHardware
* 严重性：警告
* 原因：*"物理磁盘隔离因为不受你解决方案供应商。"*
* RecommendedAction: *"替换物理磁盘受支持的硬件。"*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedFirmware
* 严重性：警告
* 原因：*"物理磁盘处于隔离因为解决方案供应商不支持它的固件版本。"*
* RecommendedAction: *"更新到最目标版本物理磁盘上的固件。"*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnrecognizedMetadata
* 严重性：警告
* 原因：*"物理磁盘具有不可识别的元数据。"*
* RecommendedAction: *"该磁盘可能包含来自未知的存储池中数据。首先，请确保没有有用数据对该磁盘，然后重置磁盘。"*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedFirmwareUpdate
* 严重性：警告
* 原因：*"失败尝试物理磁盘上的固件更新。"*
* RecommendedAction: *"尝试使用不同的固件二进制。"*

### **<a name="virtual-disk-2"></a>虚拟磁盘 (2)**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>FaultType: Microsoft.Health.FaultType.VirtualDisks.NeedsRepair
* 严重级别：信息
* 原因：*"不完全复原这个卷上的某些数据。它仍然可以访问。"*
* RecommendedAction: *"的数据还原复原"。*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>FaultType: Microsoft.Health.FaultType.VirtualDisks.Detached
* 严重性：关键
* 原因：*"音量不可用。某些数据可能会丢失。"*
* RecommendedAction: *"检查物理和/或网络的存储的所有设备的连接。你可能需要从备份进行还原。"*

### **<a name="pool-capacity-1"></a>池的容量 (1)**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>FaultType: Microsoft.Health.FaultType.StoragePool.InsufficientReserveCapacityFault
* 严重性：警告
* 原因：*"存储池中没有的最低建议的预订容量。这可能会限制你能够恢复在驱动器发生故障时的数据复原。"*
* RecommendedAction: *"向存储池中添加更多容量或释放容量。所需的最低建议预订因部署，但大约 2 驱动器相当于的容量。"*

### <a name="volume-capacity-2sup1sup"></a>**音量容量 (2)**<sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft.Health.FaultType.Volume.Capacity
* 严重性：警告
* 原因：*"音量空间已满可用。"*
* RecommendedAction: *"展开该卷，或将工作负载迁移到其他卷。"*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft.Health.FaultType.Volume.Capacity
* 严重性：关键
* 原因：*"音量空间已满可用。"*
* RecommendedAction: *"展开该卷，或将工作负载迁移到其他卷。"*

### **<a name="server-3"></a>服务器 (3)**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>FaultType: Microsoft.Health.FaultType.Server.Down
* 严重性：关键
* 原因：*"无法连接到服务器"。*
* RecommendedAction: *"开始或替换 server。"*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>FaultType: Microsoft.Health.FaultType.Server.Isolated
* 严重性：关键
* 原因：*"服务器正独立从群集由于连接问题。*
* RecommendedAction: *"如果隔离仍然存在，请检查网络或迁移到其他节点工作负载。"*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>FaultType: Microsoft.Health.FaultType.Server.Quarantined
* 严重性：关键
* 原因：*"服务器由隔离由于定期失败群集。"*
* RecommendedAction: *"替换服务器或解决网络"。*

### **<a name="cluster-1"></a>群集 (1)**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>FaultType: Microsoft.Health.FaultType.ClusterQuorumWitness.Error
* 严重性：关键
* 原因：*"群集是离开转一次服务器失败。"*
* RecommendedAction: *"检查见证资源，并根据需要重启。开始或更换失败的服务器。"*

### **<a name="network-adapterinterface-4"></a>网络适配器/接口 (4)**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disconnected
* 严重性：警告
* 原因：*"网络接口已断开连接。"*
* RecommendedAction: *"重新连接网络电缆。"*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>FaultType: Microsoft.Health.FaultType.NetworkInterface.Missing
* 严重性：警告
* 原因：*"{服务器} 不会产生缺少网络适配器连接到的网络群集 {群集网络}。*
* RecommendedAction: *"连接到缺少群集网络服务器。"*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Hardware
* 严重性：警告
* 原因：*"网络接口已硬件故障。"*
* RecommendedAction: *"替换网络接口适配器。"*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disabled
* 严重性：警告
* 原因：*"网络接口 {网络接口} 未启用和不使用"*
* RecommendedAction: *"启用网络接口。"*

### **<a name="enclosure-6"></a>外壳 (6)**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.LostCommunication
* 严重性：警告
* 原因：*"通信已丢失到存储盒。"*
* RecommendedAction: *"开始或替换存储盒。"*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.FanError
* 严重性：警告
* 原因：*"失败位置 {位置} 存储箱的粉丝。"*
* RecommendedAction: *"替换中存储箱爱好者。"*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.CurrentSensorError
* 严重性：警告
* 原因：*"失败当前的存储盒位置 {位置} 传感器。"*
* RecommendedAction: *"替换存储箱中当前的传感器。"*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.VoltageSensorError
* 严重性：警告
* 原因：*"失败电压传感器的存储盒位置 {位置} 在。"*
* RecommendedAction: *"替换电压传感器中存储盒。"*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.IoControllerError
* 严重性：警告
* 原因：*"失败的存储盒位置 {位置} 在 IO 控制器。"*
* RecommendedAction: *"替换 IO 控制器中存储盒。"*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.TemperatureSensorError
* 严重性：警告
* 原因：*"失败位置 {位置} 存储箱的传感器。"*
* RecommendedAction: *"替换存储箱中的传感器。"*

### **<a name="firmware-rollout-3"></a>固件 Rollout (3)**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.FailedMaintenanceMode
* 严重性：警告
* 原因：*当前无法执行固件推广时取得进展。"*
* RecommendedAction: *"验证所有的存储空间在正常运行，并且无故障域当前维护型。"*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.FirmwareVerifyVersionFaile
* 严重性：警告
* 原因：*"固件推广而被取消由于后固件更新的应用无法读取或意外固件版本信息。"*
* RecommendedAction: *"重启固件推出后固件问题已得到解决。"*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.TooManyFailedUpdates
* 严重性：警告
* 原因：*"固件推广而被取消无法固件更新尝试物理磁盘过多由于。"*
* RecommendedAction: *"重启固件推出后固件问题已得到解决。"*

### <a name="storage-qos-3sup2sup"></a>**存储服务 (3) 质量**<sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>FaultType: Microsoft.Health.FaultType.StorQos.InsufficientThroughput
* 严重性：警告
* 原因：*"存储吞吐量不足以满足保留。"*
* RecommendedAction: *"重新存储服务质量策略配置。"*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>FaultType: Microsoft.Health.FaultType.StorQos.LostCommunication
* 严重性：警告
* 原因：*"存储服务质量策略管理器失去通信，使用音量。"*
* RecommendedAction: *"请重新启动节点 {节点}"*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>FaultType: Microsoft.Health.FaultType.StorQos.MisconfiguredFlow
* 严重性：警告
* 原因：*"一个或多个存储消费者（通常虚拟机）使用不存在策略 id {id}。"*
* RecommendedAction: *"重新创建任何缺少存储服务质量策略。"*

<sup>1</sup>指示音量已达到 80%完整 （次要严重性） 或 90%完整 （主要程度）。  
<sup>2</sup>指示卷上的某些.vhd(s) 尚未达到其至少 IOPS 超过 10%（次要）、 30%（主要） 或 50%滚动 24 小时的窗口 （关键）。  

>[!NOTE]
> 存储外壳组件，例如爱好者、 电源和传感器的运行状况派生从 SCSI 存储模块服务 (SES)。 如果你供应商不提供此信息，运行状况服务无法显示它。  

## <a name="see-also"></a>请参阅

- [Windows Server 2016 中的运行状况服务](health-service-overview.md)

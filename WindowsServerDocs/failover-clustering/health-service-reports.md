---
title: "运行状况服务报告"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: bc21b9fdec5700fec23dc6af7ca15873ded34bea
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="health-service-reports"></a>运行状况服务报告
> 适用于 Windows Server 2016

## <a name="what-are-reports"></a>报告有哪些？  

运行状况服务减少工作，需要从你的存储空间直通群集获取实时性能和容量信息。 一个新 cmdlet 提供基本指标高效收集并跨节点，请使用内置的逻辑检测群集会员动态聚合的精选的列表。 所有值仅都均实时和的时间点。  

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

使用此 cmdlet 以获取适用于整个存储空间直通 群集指标：

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport
```

可选**计数**参数指示多少设置的值若要返回，在一个秒的间隔。  

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport -Count <Count>  
```

你还可以为某一特定的音量或服务器获取指标：  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Get-StorageHealthReport -Count <Count>  

Get-StorageNode -Name <Name> | Get-StorageHealthReport -Count <Count>
```

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

调用**GetReport**开始流式传输示例是高效收集，并跨节点，请使用内置的逻辑检测群集会员动态聚合的基本指标专家规划列表。 示例将到达此后每秒。 所有值仅都均实时和的时间点。

对于三范围可以流式传输指标：群集、任何节点或任何音量。

下面介绍了 Windows Server 2016 的每个范围指标可用的完整列表。

### <a name="iobserveronnext"></a>IObserver.OnNext()

此示例代码使用[观察程序设计模式](https://msdn.microsoft.com/en-us/library/ee850490(v=vs.110).aspx)实现观察其**OnNext()**指标每个新样本到达时，将调用方法。 其**OnCompleted()**将继续调用如果/流式传输结束时。 例如，你可能会使用它重新启动流式传输，以便无限期持续。

```
class MetricsObserver<T> : IObserver<T>
{
    public void OnNext(T Result)
    {
        // Cast
        CimMethodStreamedResult StreamedResult = Result as CimMethodStreamedResult;

        if (StreamedResult != null)
        {
            // For illustration, you could store the metrics in this dictionary
            Dictionary<string, string> Metrics = new Dictionary<string, string>();

            // Unpack
            CimInstance Report = (CimInstance)StreamedResult.ItemValue;
            IEnumerable<CimInstance> Records = (IEnumerable<CimInstance>)Report.CimInstanceProperties["Records"].Value;
            foreach (CimInstance Record in Records)
            {
                /// Each Record has "Name", "Value", and "Units"
                Metrics.Add(
                    Record.CimInstanceProperties["Name"].Value.ToString(),
                    Record.CimInstanceProperties["Value"].Value.ToString()
                    );
            }

            // TODO: Whatever you want!
        }
    }
    public void OnError(Exception e)
    {
        // Handle Exceptions
    }
    public void OnCompleted()
    {
        // Reinvoke BeginStreamingMetrics(), defined in the next section
    }
}
```

### <a name="begin-streaming"></a>开始流式传输

与定义观察，你可以开始流式传输。

指定目标**CimInstance**到所需的范围内的指标。 它可以群集、任何节点或任何音量。

流式传输结束之前，计数参数是样本数。

```
CimInstance Target = Cluster; // From among the objects discovered in DiscoverObjects()

public void BeginStreamingMetrics(CimSession Session, CimInstance HealthService, CimInstance Target)
{
    // Set Parameters
    CimMethodParametersCollection MetricsParams = new CimMethodParametersCollection();
    MetricsParams.Add(CimMethodParameter.Create("TargetObject", Target, CimType.Instance, CimFlags.In));
    MetricsParams.Add(CimMethodParameter.Create("Count", 999, CimType.UInt32, CimFlags.In));
    // Enable WMI Streaming
    CimOperationOptions Options = new CimOperationOptions();
    Options.EnableMethodResultStreaming = true;
    // Invoke API
    CimAsyncMultipleResults<CimMethodResultBase> InvokeHandler;
    InvokeHandler = Session.InvokeMethodAsync(
        HealthService.CimSystemProperties.Namespace, HealthService, "GetReport", MetricsParams, Options
        );
    // Subscribe the Observer
    MetricsObserver<CimMethodResultBase> Observer = new MetricsObserver<CimMethodResultBase>(this);
    IDisposable Disposeable = InvokeHandler.Subscribe(Observer);
}
```

毫无疑问，这些指标可以要可视化，存储在数据库中，或使用以任何方式，直到合适为止。

### <a name="properties-of-reports"></a>报告的属性

指标的每一个示例是一个"报告"其中包含许多"记录"对应于个别指标。

有关完整方案中，检查**MSFT\_StorageHealthReport**和**MSFT\_HealthRecord**中类*storagewmi.mof*。

每个指标有只需三个属性，每个此表。

| **属性** | **示例**       |
| -------------|-------------------|
| 名称         | IOLatencyAverage  |
| 值        | 0.00021           |
| 设备        | 3                 |

单位 = {0、1、2、3 和 4}，其中 0 ="字节为单位"1 ="BytesPerSecond"，2 ="CountPerSecond，"3 ="秒钟"或 4 ="百分比"。

## <a name="coverage"></a>覆盖率

以下是适用于 Windows Server 2016 的每个范围指标。

### <a name="msftstoragesubsystem"></a>MSFT_StorageSubSystem

| **名称**                        | **设备** |
|---------------------------------|-----------|
| CPUUsage                        | 4         |
| CapacityPhysicalPooledAvailable | 0         |
| CapacityPhysicalPooledTotal     | 0         |
| CapacityPhysicalTotal           | 0         |
| CapacityPhysicalUnpooled        | 0         |
| CapacityVolumesAvailable        | 0         |
| CapacityVolumesTotal            | 0         |
| IOLatencyAverage                | 3         |
| IOLatencyRead                   | 3         |
| IOLatencyWrite                  | 3         |
| IOPSRead                        | 2         |
| IOPSTotal                       | 2         |
| IOPSWrite                       | 2         |
| IOThroughputRead                | 1         |
| IOThroughputTotal               | 1         |
| IOThroughputWrite               | 1         |
| MemoryAvailable                 | 0         |
| MemoryTotal                     | 0         |


### <a name="msftstoragenode"></a>MSFT_StorageNode

| **名称**            | **设备** |
|---------------------|-----------|
| CPUUsage            | 4         |
| IOLatencyAverage    | 3         |
| IOLatencyRead       | 3         |
| IOLatencyWrite      | 3         |
| IOPSRead            | 2         |
| IOPSTotal           | 2         |
| IOPSWrite           | 2         |
| IOThroughputRead    | 1         |
| IOThroughputTotal   | 1         |
| IOThroughputWrite   | 1         |
| MemoryAvailable     | 0         |
| MemoryTotal         | 0         |

### <a name="msftvolume"></a>MSFT_Volume

| **名称**            | **设备** |
|---------------------|-----------|
| CapacityAvailable   | 0         |
| CapacityTotal       | 0         |
| IOLatencyAverage    | 3         |
| IOLatencyRead       | 3         |
| IOLatencyWrite      | 3         |
| IOPSRead            | 2         |
| IOPSTotal           | 2         |
| IOPSWrite           | 2         |
| IOThroughputRead    | 1         |
| IOThroughputTotal   | 1         |
| IOThroughputWrite   | 1         |

## <a name="see-also"></a>请参阅

- [Windows Server 2016 中的运行状况服务](health-service-overview.md)

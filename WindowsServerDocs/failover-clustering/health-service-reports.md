---
title: 运行状况服务报告
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: e018c0270a0bf410dada9c05d2c25e51fdfac1d8
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280156"
---
# <a name="health-service-reports"></a>运行状况服务报告
> 适用于：Windows Server 2019、Windows Server 2016

## <a name="what-are-reports"></a>报表有哪些？  

运行状况服务会减少从存储空间直通群集获取实时性能和容量信息所需的工作。 一个新的 cmdlet 提供了有效地收集和动态聚合跨节点，具有内置逻辑来检测群集成员身份的基本指标的特选的列表。 所有值均仅是实时和时间点形式的。  

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

此 cmdlet 用于获取整个存储空间直通群集的指标：

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport
```

可选**计数**参数指示多少设置的值返回一个第二个时间间隔。  

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport -Count <Count>  
```

您还可以获取一个特定卷或服务器的度量值：  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Get-StorageHealthReport -Count <Count>  

Get-StorageNode -Name <Name> | Get-StorageHealthReport -Count <Count>
```

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

您可以访问所有相同的属性中所述[存储管理 API 类](https://msdn.microsoft.com/library/windows/desktop/hh830612(v=vs.85).aspx)。

```
...
using System.Diagnostics;

foreach (CimInstance Node in Nodes)
{
    // For illustration, write each node's Name to the console. You could also write State (up/down), or anything else!
    Debug.WriteLine("Discovered Node " + Node.CimInstanceProperties["Name"].Value.ToString());
}
```

调用**GetReport**要开始流式处理示例的重要指标，这有效地收集和动态聚合跨节点，具有内置逻辑来检测群集成员身份的专家特选列表。 示例将到达此后每隔一秒。 所有值均仅是实时和时间点形式的。

可将指标流式传输的三个作用域： 群集、 任意节点或任何卷。

在 Windows Server 2016 中的每个作用域的可用度量值的完整列表如下所述。

### <a name="iobserveronnext"></a>IObserver.OnNext()

此代码示例使用[观察者设计模式](https://msdn.microsoft.com/library/ee850490(v=vs.110).aspx)若要实现观察程序其**OnNext()** 度量值的每个新示例到达时，将调用方法。 其**OnCompleted()** 方法时将调用如果/流式处理结束。 例如，您可能会使用它来重新启动流式处理，因此它将无限期地继续。

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

### <a name="begin-streaming"></a>开始流式处理

通过定义观察程序，你可以开始流式处理。

指定目标**CimInstance**到所需作用域内的度量值。 它可以是群集、 任意节点或任何卷。

Count 参数是在流结束前的样本数。

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

不用说，这些指标可以进行可视化，存储在数据库中，或用于任何认为适当的方式。

### <a name="properties-of-reports"></a>报表的属性

度量值的每个示例是一个"报表"包含多个"记录"对应于单个指标。

对于完整的架构中，检查**MSFT\_StorageHealthReport**并**MSFT\_HealthRecord**中的类*storagewmi.mof*。

每个度量值具有只需三个属性，每个此表。

| **属性** | **示例**       |
| -------------|-------------------|
| 名称         | IOLatencyAverage  |
| ReplTest1        | 0.00021           |
| 单位        | 3                 |

单位 = {0、 1、 2、 3、 4}，其中 0 ="Bytes"，1 ="BytesPerSecond"，2 ="CountPerSecond"，3 ="Seconds"或 4 ="百分比"。

## <a name="coverage"></a>覆盖范围

以下是指标适用于 Windows Server 2016 中的每个作用域。

### <a name="msftstoragesubsystem"></a>MSFT_StorageSubSystem

| **名称**                        | **单位** |
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

| **名称**            | **单位** |
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

| **名称**            | **单位** |
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

---
title: 运行状况服务报表
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: e65db8834bd0b059dc7bbebbcaf9288fb46da225
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369676"
---
# <a name="health-service-reports"></a>运行状况服务报表
> 适用于：Windows Server 2019、Windows Server 2016

## <a name="what-are-reports"></a>什么是报表  

运行状况服务减少了从存储空间直通群集获取实时性能和容量信息所需的工作量。 一个新的 cmdlet 提供了一个特选的基本指标列表，它们使用内置逻辑来检测群集成员身份，以动态方式跨节点进行收集和聚合。 所有值均仅是实时和时间点形式的。  

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

使用此 cmdlet 可获取整个存储空间直通群集的指标：

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport
```

可选**计数**参数指示要返回的值的数目，以一秒为间隔。  

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport -Count <Count>  
```

你还可以获取一个特定卷或服务器的指标：  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Get-StorageHealthReport -Count <Count>  

Get-StorageNode -Name <Name> | Get-StorageHealthReport -Count <Count>
```

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

调用**GetReport** ，以开始特选基本指标的流式处理示例，这些示例是使用内置逻辑跨节点动态收集并动态聚合的，以检测群集成员身份。 稍后将每秒到达一次示例。 所有值均仅是实时和时间点形式的。

可为三个作用域流式传输度量值：群集、任意节点或任何卷。

Windows Server 2016 中的每个作用域提供的指标完整列表如下所述。

### <a name="iobserveronnext"></a>IObserver. OnNext （）

此示例代码使用[观察程序设计模式](https://msdn.microsoft.com/library/ee850490(v=vs.110).aspx)来实现一个观察器，当度量值的每个新样本到达时，将调用其**OnNext （）** 方法。 如果流式处理结束时，将调用其**OnCompleted （）** 方法。 例如，你可以使用它来重新启动流式处理，因此它会无限期地继续。

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

定义观察者后，可以开始流式传输。

指定要为其指定指标范围的目标**CimInstance** 。 它可以是群集、任何节点或任何卷。

Count 参数是流式处理结束之前的样本数。

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

不用说，可以可视化这些指标，将其存储在数据库中，或者以任何适合的方式使用。

### <a name="properties-of-reports"></a>报表的属性

度量值的每个示例都是一个 "报表"，其中包含与各个度量值相对应的多个 "记录"。

对于完整的架构，请在*storagewmi*中检查**msft @ no__t-1StorageHealthReport**和**msft @ no__t-3HealthRecord**类。

每个指标仅有三个属性，每个表。

| **Property** | **示例**       |
| -------------|-------------------|
| 名称         | IOLatencyAverage  |
| ReplTest1        | 0.00021           |
| 计算        | 3                 |

单位 = {0，1，2，3，4}，其中 0 = "字节"，1 = "BytesPerSecond"，2 = "CountPerSecond"，3 = "秒"，或 4 = "百分比"。

## <a name="coverage"></a>覆盖范围

下面是适用于 Windows Server 2016 中每个作用域的指标。

### <a name="msft_storagesubsystem"></a>MSFT_StorageSubSystem

| **名称**                        | **计算** |
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


### <a name="msft_storagenode"></a>MSFT_StorageNode

| **名称**            | **计算** |
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

### <a name="msft_volume"></a>MSFT_Volume

| **名称**            | **计算** |
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

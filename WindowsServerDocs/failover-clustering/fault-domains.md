---
ms.assetid: 56fc7f80-9558-467e-a6e9-a04c9abbee33
title: 故障域感知
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-failover-clustering
ms.topic: article
author: cosmosdarwin
ms.date: 09/16/2016
ms.openlocfilehash: 18b7a932cc8a22c356fde89baa316c0532ebc374
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475994"
---
# <a name="fault-domain-awareness"></a>故障域感知

> 适用于：Windows Server 2019 和 Windows Server 2016

故障转移群集允许多台服务器协同工作以提供高可用性，换句话说即提供节点容错能力。 但如今的企业需要从其基础结构越来越高的可用性。 要实现类似于云的正常运行时间，即使是极不可能发生的情况（例如底盘故障、机架中断或自然灾害等）也必须进行防护。 这就是为什么故障转移群集 Windows Server 2016 中引入了底盘、 机架和站点容错功能。

## <a name="fault-domain-awareness"></a>故障域感知

容错域和容错能力是密切相关的概念。 容错域是一组共享单一故障点的硬件组件。 要使容错能力达到某个级别，需要相应级别的多个容错域。 例如，要使机架具备容错能力，服务器和数据必须分布在多个机架。

此短视频概述了 Windows Server 2016 中的容错域：  
[![单击此图像可观看概述 Windows Server 2016 中的容错域](media/Fault-Domains-in-Windows-Server-2016/Part-1-Fault-Domains-Overview.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-1-Overview)

### <a name="fault-domain-awareness-in-windows-server-2019"></a>在 Windows Server 2019 的容错域感知

容错域感知现已推出 Windows Server 2019，但它默认处于禁用状态，必须通过 Windows 注册表启用。

若要启用 Windows Server 2019 中的容错域感知，转到 Windows 注册表，并设置 （Get-群集）。AutoAssignNodeSite 注册表项设置为 1。

```Registry
    (Get-Cluster).AutoAssignNodeSite=1
```

若要禁用 Windows 2019 中的容错域感知，转到 Windows 注册表，并设置 （Get-群集）。AutoAssignNodeSite 注册表项设置为 0。

```Registry
    (Get-Cluster).AutoAssignNodeSite=0
```

## <a name="benefits"></a>优势
- **存储空间，包括存储空间直通，使用容错域来最大化数据安全性。**  
    从概念上讲，存储空间复原类似软件定义的分布式 RAID。 所有数据的多个副本都保持同步，如果硬件出现故障，一个副本丢失，其他副本会被重新复制，以修复复原功能。 要获得可能的最佳复原，副本应保存在单独的容错域中。

- **[运行状况服务](health-service-overview.md)使用容错域来提供更多有用的警报。**  
    每个容错域可与位置元数据关联，这些数据将自动包含在任何后续警报中。 这些描述符可以协助操作或维护人员，并通过消除硬件歧义减少错误。  

- **拉伸群集使用容错域，用于存储关联。** 拉伸群集允许远程服务器加入通用群集。 为了获得最佳性能，应用程序或虚拟机应运行在靠近为其提供存储空间的服务器上。 容错域感知使此存储关联。   

## <a name="levels-of-fault-domains"></a>容错域级别  
有四个规范级别的容错域：站点、机架、底盘和节点。 系统会自动发现节点；其他每个级别是可选的。 例如，如果部署未使用刀片服务器，则底盘级别可能就没有意义。  

![关系图的不同级别的容错域](media/Fault-Domains-in-Windows-Server-2016/levels-of-fault-domains.png)

## <a name="usage"></a>用法  
可以使用 PowerShell 或 XML 标记来指定容错域。 这两种方法是等效的并可提供完整功能。

>[!IMPORTANT]
> 如果可以，在启用存储空间直通前指定容错域。 这样可为底盘或机架容错启用自动配置，以准备池、层和类似复原及列计数的设置。 一旦创建了池和卷，数据不会追溯地移动以响应容错域拓扑的变化。 启用存储空间直通后，要在底盘或机架之间移动节点，首先应使用 `Remove-ClusterNode -CleanUpDisks` 从池中退出节点及其驱动器。

### <a name="defining-fault-domains-with-powershell"></a>使用 PowerShell 定义容错域
Windows Server 2016 引入了以下 cmdlet，可以使用容错域：
* `Get-ClusterFaultDomain`
* `Set-ClusterFaultDomain`
* `New-ClusterFaultDomain` 
* `Remove-ClusterFaultDomain`

此短视频演示了这些 cmdlet 的用法。
[![单击此图像可观看简短视频上的群集的容错域 cmdlet 的使用情况](media/Fault-Domains-in-Windows-Server-2016/Part-2-Using-PowerShell.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-2-Using-PowerShell)

使用`Get-ClusterFaultDomain`若要查看当前容错域拓扑。 该 cmdlet 将列出群集中的所有节点，以及已创建的任何底盘、机架或站点。 可以使用类似于 **-Type** 或 **-Name** 的参数进行筛选，但这不是必需操作。

```PowerShell
Get-ClusterFaultDomain
Get-ClusterFaultDomain -Type Rack
Get-ClusterFaultDomain -Name "server01.contoso.com"
```

使用`New-ClusterFaultDomain`若要创建新底盘、 机架或站点。 `-Type`和`-Name`参数是必需的。 可能的值`-Type`都`Chassis`， `Rack`，和`Site`。 `-Name`可以是任意字符串。 (有关`Node`类型的容错域，名称必须是作为集的实际节点名称中，自动)。

```PowerShell
New-ClusterFaultDomain -Type Chassis -Name "Chassis 007"
New-ClusterFaultDomain -Type Rack -Name "Rack A"
New-ClusterFaultDomain -Type Site -Name "Shanghai"
```

> [!IMPORTANT]  
> Windows Server 不能和不会验证你创建任何容错域对应于实际的物理世界中的任何内容。 （这可能听起来平淡无奇，但务必了解）。在物理世界中，如果节点都位于一个机架中，那么在软件中创建两个 `-Type Rack` 容错域并不能就神奇地提供机架容错能力。 你有责任确保使用这些 cmdlet 创建的拓扑匹配硬件的实际排列方式。

使用`Set-ClusterFaultDomain`将一个容错域移动到另一个。 术语“父项”和“子项”通常用于描述此嵌套关系。 `-Name`和`-Parent`参数是必需的。 在中`-Name`，提供名称移动的容错域; 在`-Parent`，提供目标的名称。 要一次移动多个容错域，请列出它们的名称。

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent "Rack A"
Set-ClusterFaultDomain -Name "Rack A", "Rack B", "Rack C", "Rack D" -Parent "Shanghai"
```

> [!IMPORTANT]  
> 移动容错域时，其子项随其移动。 在上面的示例中，如果机架 A 是 server01.contoso.com 的父项，后者不需要单独移动到上海站点 - 由于其父项已存在于此站点，因此它也已在此站点存在，就像在物理世界中一样。

可以看到的输出中的父-子关系`Get-ClusterFaultDomain`，请在`ParentName`和`ChildrenNames`列。

此外可以使用`Set-ClusterFaultDomain`若要修改的容错域的某些其他属性。 例如，你可以提供可选`-Location`或`-Description`为任何容错域的元数据。 如果提供，此信息将包含在运行状况服务发出的硬件警报中。 此外可以重命名容错域使用`-NewName`参数。 不要重命名`Node`类型的容错域。

```PowerShell
Set-ClusterFaultDomain -Name "Rack A" -Location "Building 34, Room 4010"
Set-ClusterFaultDomain -Type Node -Description "Contoso XYZ Server"
Set-ClusterFaultDomain -Name "Shanghai" -NewName "China Region"
```

使用`Remove-ClusterFaultDomain`删除底盘、 机架或站点已创建。 `-Name` 参数是必需的。 您不能删除包含子项的容错域 – 首先，删除子项，或将它们移之外使用`Set-ClusterFaultDomain`。 若要移动的容错域之外的所有其他容错域，设置其`-Parent`为空字符串 ("")。 不能删除`Node`类型的容错域。 要一次删除多个容错域，请列出它们的名称。

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent ""
Remove-ClusterFaultDomain -Name "Rack A"
```

### <a name="defining-fault-domains-with-xml-markup"></a>使用 XML 标记定义容错域
使用 XML 极具创意的语法来指定容错域。 我们建议使用你最喜欢的文本编辑器，如 Visual Studio Code（可从 *[此处](https://code.visualstudio.com/)* 免费获取）或记事本，创建一个可以保存并重复使用的 XML 文档。  

此短视频演示了如何使用 XML 标记来指定容错域。

[![单击此图像可观看简短视频对如何使用 XML 来指定容错域](media/Fault-Domains-in-Windows-Server-2016/Part-3-Using-XML-Markup.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-3-Using-XML)

在 PowerShell 中运行以下 cmdlet: `Get-ClusterFaultDomainXML`。 这将返回群集的当前容错域规范，例如 XML。 这反映了每个发现`<Node>`，在打开和关闭包装`<Topology>`标记。  

运行以下操作，将此输出保存到文件中。  

```PowerShell
Get-ClusterFaultDomainXML | Out-File <Path>  
```

打开该文件，并添加`<Site>`， `<Rack>`，和`<Chassis>`标记，以指定如何将这些节点分布在站点、 机架和底盘。 每个标记必须通过唯一的 **Name** 进行识别。 对于节点，必须保持默认填充的节点名称。  

> [!IMPORTANT]  
> 虽然所有其他标记均为可选，但它们必须遵循可传递的 Site &gt; Rack &gt; Chassis &gt; Node 层次结构，并且必须正确关闭。  
除了名称以外，自由格式`Location="..."`和`Description="..."`描述符可以添加到任何标记。  

#### <a name="example-two-sites-one-rack-each"></a>例如：两个站点一个机架  

```XML
<Topology>  
  <Site Name="SEA" Location="Contoso HQ, 123 Example St, Room 4010, Seattle">  
    <Rack Name="A01" Location="Aisle A, Rack 01">  
      <Node Name="Server01" Location="Rack Unit 33" />  
      <Node Name="Server02" Location="Rack Unit 35" />  
      <Node Name="Server03" Location="Rack Unit 37" />  
    </Rack>  
  </Site>  
  <Site Name="NYC" Location="Regional Datacenter, 456 Example Ave, New York City">  
    <Rack Name="B07" Location="Aisle B, Rack 07">  
      <Node Name="Server04" Location="Rack Unit 20" />  
      <Node Name="Server05" Location="Rack Unit 22" />  
      <Node Name="Server06" Location="Rack Unit 24" />  
    </Rack>  
  </Site>  
</Topology> 
``` 

#### <a name="example-two-chassis-blade-servers"></a>示例：两个底盘，刀片服务器  
```XML
<Topology>  
  <Rack Name="A01" Location="Contoso HQ, Room 4010, Aisle A, Rack 01">  
    <Chassis Name="Chassis01" Location="Rack Unit 2 (Upper)" >  
      <Node Name="Server01" Location="Left" />  
      <Node Name="Server02" Location="Right" />  
    </Chassis>  
    <Chassis Name="Chassis02" Location="Rack Unit 6 (Lower)" >  
      <Node Name="Server03" Location="Left" />  
      <Node Name="Server04" Location="Right" />  
    </Chassis>  
  </Rack>  
</Topology>  
```

若要设置新容错域规范，保存您的 XML 并运行以下命令在 PowerShell 中。  

```PowerShell
$xml = Get-Content <Path> | Out-String  
Set-ClusterFaultDomainXML -XML $xml
```

本指南介绍了仅使用两个示例，但`<Site>`， `<Rack>`， `<Chassis>`，和`<Node>`标记可以进行混合和匹配多种其他方式，以反映你的部署的物理拓扑，可能存在的内容。 我们希望这些示例可阐明这些标记的灵活性，以及自由格式位置描述符消除它们歧义的价值。  

### <a name="optional-location-and-description-metadata"></a>可选：位置和说明的元数据

你可以提供可选**位置**或**说明**为任何容错域的元数据。 如果提供，此信息将包含在运行状况服务发出的硬件警报中。 此短片演示如何添加此类描述符的值。

[![单击可查看演示将位置描述符添加到容错域的值的简短视频](media/Fault-Domains-in-Windows-Server-2016/part-4-location-description.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-4-Location-Description)

## <a name="see-also"></a>请参阅  
- [Windows Server 2019 入门](https://docs.microsoft.com/windows-server/get-started-19/get-started-19)  
- [开始使用 Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/server-basics)  
-   [存储空间直通概述](../storage/storage-spaces/storage-spaces-direct-overview.md) 

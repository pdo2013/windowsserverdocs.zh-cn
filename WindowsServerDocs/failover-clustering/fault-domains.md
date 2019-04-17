---
ms.assetid: 56fc7f80-9558-467e-a6e9-a04c9abbee33
title: "故障域感知"
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-failover-clustering
ms.topic: article
author: cosmosdarwin
ms.date: 09/16/2016
ms.openlocfilehash: f5c64bb8f8b7d4b8d13c76c4e94cfcf52ee32c30
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="fault-domain-awareness-in-windows-server-2016"></a>在 Windows Server 2016 的故障域感知

> 适用于： Windows Server 2016

故障转移群集支持多个一起协作，以提供高可用性 – 或将另一种方法以提供节点错放的服务器。 但今天发布的企业需要更好的可用性从他们的基础结构。 若要实现类似于云中的运行时间，必须抵御例如机箱故障、机架中断，或者自然灾难太甚至可能发生。 这就是为什么故障转移群集 Windows Server 2016 中也引入机箱、架和网站故障的能力。

故障域和出错的功能是密切相关的概念。 故障域是失败的一组共享单点的硬件组件。 若要允许的某个级别的故障，你将需要该级别的多个错误域。 例如，若要进行机架能力故障、你服务器和您的数据必须分发跨多个机架。

此简短视频概述了故障域，Windows Server 2016 中：  
[![单击此观看故障域，Windows Server 2016 中的概览的图像](media/Fault-Domains-in-Windows-Server-2016/Part-1-Fault-Domains-Overview.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-1-Overview)

## <a name="benefits"></a>权益
- **存储空间，包括存储空间直通，使用故障域来最大化数据安全。**  
    存储空间中的复原概念就好像在 RAID 分布式、软件定义。 多个副本的所有数据将保持同步，并且无法正常工作的硬件和一个副本会丢失，其他人重新复制还原复原。 以实现最佳的可能复原，应在单独的故障域保持副本。

- **[运行状况服务](health-service-overview.md)使用故障域以提供更有用的警报。**  
    每个错误域可以关联的位置的元数据，自动将包括在任何后续警报。 这些描述符可以助手操作或维护人员，并减少错误消除歧义硬件。  

- **拉伸群集故障域用于存储关联。** 拉伸群集允许加入常见群集远服务器。 为了获得最佳性能，应是与提供它们的存储附近的服务器上运行应用程序或虚拟机。 故障域感知启用此存储关联。   

## <a name="levels-of-fault-domains"></a>故障域的级别  
有四种故障域的站点、机箱，及其节点的 canonical 级别。 自动; 发现节点是可选的每个额外的电量。 例如，如果你的部署不使用刀口服务器，机箱级别可能不意义为你。  

![故障域不同级别的图示](media/Fault-Domains-in-Windows-Server-2016/levels-of-fault-domains.png)

## <a name="usage"></a>使用情况  
你可以使用 PowerShell 或 XML 标记指定故障域。 这两种方法等效并提供完整的功能。

>[!IMPORTANT]
> 启用存储空间直通，尽可能之前，请指定故障域。 这使自动配置准备层，池，如复原和列计数设置来机箱或一架故障的能力。 创建池和卷之后, 数据将不会进入响应更改故障域拓扑。 个节点或之间进行移动机箱机架启用存储空间直通 后，你应该先退出节点和从池中使用其驱动器`Remove-ClusterNode -CleanUpDisks`。

### <a name="defining-fault-domains-with-powershell"></a>定义与 PowerShell 故障域
Windows Server 2016 引入才能故障域以下 cmdlet:
* `Get-ClusterFaultDomain`
* `Set-ClusterFaultDomain`
* `New-ClusterFaultDomain` 
* `Remove-ClusterFaultDomain`

此简短视频演示这些 cmdlet 的使用量。
[![单击该图像观看的群集故障域 cmdlet 使用量的简短视频](media/Fault-Domains-in-Windows-Server-2016/Part-2-Using-PowerShell.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-2-Using-PowerShell)

使用`Get-ClusterFaultDomain`以查看当前故障域拓扑。 这将列出群集，以及任何机箱、机架或你创建的站点中的所有节点。 你可以使用类似于参数筛选**的键入**或**-名称**，但它们不需要。

```PowerShell
Get-ClusterFaultDomain
Get-ClusterFaultDomain -Type Rack
Get-ClusterFaultDomain -Name "server01.contoso.com"
```

使用`New-ClusterFaultDomain`创建新机箱、机架或网站。 `-Type`和`-Name`所需的参数。 可能值`-Type`是`Chassis`，`Rack`，并`Site`。 `-Name`可以是任何字符串。 (对于`Node`类型故障域，名称必须实际节点名称，设置为自动)。

```PowerShell
New-ClusterFaultDomain -Type Chassis -Name "Chassis 007"
New-ClusterFaultDomain -Type Rack -Name "Rack A"
New-ClusterFaultDomain -Type Site -Name "Shanghai"
```

> [!IMPORTANT]  
> Windows Server 无法，并且无法验证你创建的任何故障域对应于实际、物理世界中的所有内容。 （这可能看起来很明显，但请务必了解。）如果在物理世界中，全部在一个架将是你节点，然后创建两个`-Type Rack`故障域中软件神奇不提供机架故障的能力。 你负责确保的拓扑您将使用这些 cmdlet 创建符合你的硬件的实际排列。

使用`Set-ClusterFaultDomain`移动到另一个故障域。 术语"家长"和"的孩子"通常用于描述此嵌套关系。 `-Name`和`-Parent`所需的参数。 在`-Name`，提供移动; 故障域名称在`-Parent`，提供目标位置的名称。 若要一次性移动多个错误域，列出了他们的姓名。

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent "Rack A"
Set-ClusterFaultDomain -Name "Rack A", "Rack B", "Rack C", "Rack D" -Parent "Shanghai"
```

> [!IMPORTANT]  
> 当移动故障域时，孩子移动与他们。 在上方的示例中，如果机架 A server01.contoso.com 家长，后者单独不必将移到上海网站，该文件名存在已通过其父不断，就像在物理世界中。

你可以查看在输出中的家长孩子关系`Get-ClusterFaultDomain`中`ParentName`和`ChildrenNames`列。

你还可以使用`Set-ClusterFaultDomain`修改故障域的某些其他属性。 例如，你可以提供可选`-Location`或`-Description`任何故障域的元数据。 如果提供，此信息将包含在从运行状况服务的硬件警报。 你可以重命名故障域使用`-NewName`参数。 不要重命名`Node`键入故障域。

```PowerShell
Set-ClusterFaultDomain -Name "Rack A" -Location "Building 34, Room 4010"
Set-ClusterFaultDomain -Type Node -Description "Contoso XYZ Server"
Set-ClusterFaultDomain -Name "Shanghai" -NewName "China Region"
```

使用`Remove-ClusterFaultDomain`要删除机箱、机架或你创建的站点。 `-Name`参数是必需的。 您不能删除包含孩子故障域，首先，删除孩子，或将它们移之外使用`Set-ClusterFaultDomain`。 若要移动之外的所有其他故障域故障域，设置其`-Parent`为空的字符串 ("")。 你无法删除`Node`键入故障域。 若要一次性删除多个错误域，列出了他们的姓名。

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent ""
Remove-ClusterFaultDomain -Name "Rack A"
```

### <a name="defining-fault-domains-with-xml-markup"></a>定义与 XML 标记故障域
可以使用 XML 灵感语法指定故障域。 我们建议使用你最喜欢的文本编辑器，如 Visual Studio 代码 (可用免费*[此处](https://code.visualstudio.com/)*) 或记事本，创建一个 XML 文档，你可以保存并重新使用该值。  

此简短视频演示 XML 标记指定故障域的使用量。

[![C单击该映像，即可观看有关如何使用 XML 指定故障域的简短视频](media/Fault-Domains-in-Windows-Server-2016/Part-3-Using-XML-Markup.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-3-Using-XML)

在 PowerShell 中，运行以下 cmdlet: `Get-ClusterFaultDomainXML`。 这将返回当前故障域规范群集，为 XML。 这表明每发现`<Node>`、包装中打开和关闭`<Topology>`标记。  

运行以下步骤将此输出保存到某个文件。  

```PowerShell
Get-ClusterFaultDomainXML | Out-File <Path>  
```

打开该文件，并添加`<Site>`，`<Rack>`，并`<Chassis>`标记指定如何站点、机架和机箱跨分配这些节点。 每个标记必须由唯一标识**名称**。 对于节点，你必须在默认情况下在节节保留节点的名称。  

> [!IMPORTANT]  
> 所有其他标记是可选的其必须遵守传递站点&gt;机架&gt;机箱&gt;节点层次，必须正确关闭。  
除了名称、自由形式`Location="..."`和`Description="..."`描述符可以添加到任何标记。  

#### <a name="example-two-sites-one-rack-each"></a>示例：两个站点一个架  

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

#### <a name="example-two-chassis-blade-servers"></a>示例：两个机箱刀口服务器  
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

若要设置新标准，故障域，保存你 XML 并运行以下命令在 PowerShell 中。  

```PowerShell
$xml = Get-Content <Path> | Out-String  
Set-ClusterFaultDomainXML -XML $xml
```

本指南介绍只需两个示例，但`<Site>`，`<Rack>`，`<Chassis>`，并`<Node>`标记可以混合和好友在许多其他方面，以反映你的部署物理拓扑任何可能。 我们希望以上这些示例阐述了这些标记的灵活性和的任意位置描述符它们区分值。  

### <a name="optional-location-and-description-metadata"></a>可选：位置和说明元数据

你可以提供可选**位置**或**描述**任何故障域的元数据。 如果提供，此信息将包含在从运行状况服务的硬件警报。 此简短视频演示添加此类描述符的价值。

[![C单击以查看演示添加到故障域位置描述符的价值的简短视频](media/Fault-Domains-in-Windows-Server-2016/part-4-location-description.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-4-Location-Description)

## <a name="see-also"></a>请参阅  
-   [Windows Server 2016](../get-started/windows-server-2016.md)  
-   [直接在 Windows Server 2016 的存储空间](../storage/storage-spaces/storage-spaces-direct-overview.md) 

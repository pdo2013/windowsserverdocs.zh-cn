---
title: 群集到群集存储副本在 Azure 中的同一区域内
description: 在 Azure 中的同一区域内的群集存储复制到群集
keywords: 存储副本，服务器管理器、 Windows Server、 Azure、 群集、 位于同一区域
author: arduppal
ms.author: arduppal
ms.date: 04/26/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 4371192d44878d3c953374b8d307b4d5612869f5
ms.sourcegitcommit: 7e54a1bcd31cd2c6b18fd1f21b03f5cfb6165bf3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2019
ms.locfileid: "65461981"
---
# <a name="cluster-to-cluster-storage-replica-within-the-same-region-in-azure"></a>群集到群集存储副本在 Azure 中的同一区域内

> 适用于：Windows Server 2019，Windows Server 2016 中，Windows Server （半年频道）

可以在 Azure 中配置群集到群集存储复制在同一区域中。 在下面的示例中，我们使用双节点群集，但群集到群集存储副本不会被限制到两个节点群集。 下图是可以彼此进行通信的两个节点存储空间直通群集是在同一个域，并在同一区域中。

观看有关该过程的完整演练以下视频。

第一部分
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE26f2Y]

第二部分
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE269Pq]

![同一区域内的体系结构关系图展示在 Azure 中的群集到群集存储副本。](media\Cluster-to-cluster-azure-one-region\architecture.png)
> [!IMPORTANT]
> 所有被引用的示例是特定于上图。

1. 创建[资源组](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup)区域中在 Azure 门户中 (**SR AZ2AZ**中**美国西部 2**)。 
2. 创建两个[可用性集](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM)资源组中 (**SR AZ2AZ**) 更高版本，创建的一个用于每个群集。 
    a. 可用性集 (**az2azAS1**) b。 可用性集 (**az2azAS2**)
3. 创建[虚拟网络](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM)(**az2az Vnet**) 在以前创建的资源组中 (**SR AZ2AZ**)，具有至少一个子网。 
4. 创建[网络安全组](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM)(**az2az NSG**)，并添加 RDP:3389 的一个入站的安全规则。 您可以选择您的安装程序完成后删除此规则。 
5. 创建 Windows Server[虚拟机](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM)以前创建的资源组中 (**SR AZ2AZ**)。 使用以前创建的虚拟网络 (**az2az Vnet**) 和网络安全组 (**az2az NSG**)。 
   
   域控制器 (**az2azDC**)。 您可以选择创建用于你的域控制器的第三个可用性集或的域控制器添加两个可用性集之一。 如果您将这添加到创建两个群集的可用性集，将其分配标准公共 IP 地址在 VM 创建过程。 
   - 安装 Active Directory 域服务。
   - 创建一个域 (Contoso.com)
   - 创建具有管理员权限 (contosoadmin) 的用户 
   - 创建两个虚拟机 (**az2az1**， **az2az2**) 在第一个可用性集中 (**az2azAS1**)。 将标准公共 IP 地址分配给每个虚拟机本身在创建过程。
   - 将至少 2 个托管的磁盘添加到每台计算机
   - 安装故障转移群集和存储副本功能
   - 创建两个虚拟机 (**az2az3**， **az2az4**) 在第二个可用性集中 (**az2azAS2**)。 将标准公共 IP 地址分配给每个虚拟机本身在创建过程。 
   - 将至少 2 个托管的磁盘添加到每台计算机。 
   - 安装故障转移群集和存储副本功能。 
   
6. 连接到域的所有节点，并向以前创建的用户提供管理员权限。 

7. 更改为域控制器的专用 IP 地址的虚拟网络的 DNS 服务器。 
8. 在本例中为域控制器**az2azDC**具有专用 IP 地址 (10.3.0.8)。 虚拟网络中 (**az2az Vnet**) 更改 10.3.0.8 的 DNS 服务器。 连接到"Contoso.com"的所有节点，并提供对"contosoadmin"的管理员特权。
   - 从所有节点 contosoadmin 以用户身份登录。 
    
9. 创建群集 (**SRAZC1**， **SRAZC2**)。 下面是我们的示例中的 PowerShell 命令
```PowerShell
    New-Cluster -Name SRAZC1 -Node az2az1,az2az2 –StaticAddress 10.3.0.100
```
```PowerShell
    New-Cluster -Name SRAZC2 -Node az2az3,az2az4 –StaticAddress 10.3.0.101
```
10. 启用存储空间直通
```PowerShell
    Enable-clusterS2D
```   
   
   对于每个群集创建虚拟磁盘和卷。 一个用于数据，另一个用于日志。 
   
11. 创建内部的标准 SKU[负载均衡器](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM)每个分类 (**azlbr1**，**azlbr2**)。 
   
   提供的群集 IP 地址作为静态专用 IP 地址的负载均衡器。
   - azlbr1 = > 前端 IP:10.3.0.100 (选取虚拟网络中未使用的 IP 地址 (**az2az Vnet**) 子网)
   - 为每个负载均衡器创建后端池。 添加关联的群集节点。
   - 创建运行状况探测： 端口 59999
   - 创建负载均衡规则：允许使用已启用浮动 IP HA 端口。 
   
   提供的群集 IP 地址作为静态专用 IP 地址的负载均衡器。
   - azlbr2 = > 前端 IP:10.3.0.101 (选取虚拟网络中未使用的 IP 地址 (**az2az Vnet**) 子网)
   - 为每个负载均衡器创建后端池。 添加关联的群集节点。
   - 创建运行状况探测： 端口 59999
   - 创建负载均衡规则：允许使用已启用浮动 IP HA 端口。 
   
12. 在每个群集节点上打开端口 59999 （运行状况探测）。 
   
    每个节点上运行以下命令：
```PowerShell
netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
```   
13. 指示要侦听的运行状况探测端口 59999 上的消息和响应来自当前拥有此资源的节点的群集。 运行一次从群集中，为每个群集的任何一个节点。 
    
    在我们的示例，请确保更改"ILBIP"根据你的配置值。 从任何一个节点上运行以下命令**az2az1**/**az2az2**:

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}
    ```

14. 从任何一个节点上运行以下命令**az2az3**/**az2az4**。 

    ```PowerShell
    $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
    $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
    $ILBIP = "10.3.0.101" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
    [int]$ProbePort = 59999
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```   
   请确保这两个群集可以连接 / 相互通信。 

   可以使用故障转移群集管理器中"连接到群集"功能来连接到其他群集或检查其他群集响应来自当前群集的节点之一。  
   
   ```PowerShell
     Get-Cluster -Name SRAZC1 (ran from az2az3)
   ```
   ```PowerShell
     Get-Cluster -Name SRAZC2 (ran from az2az1)
   ```   

15. 创建两个群集的云见证的服务器。 创建两个[存储帐户](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM)(**az2azcw**， **az2azcw2**) 中 azure 另一个用于每个群集位于同一资源组 (**SR AZ2AZ**)。

    - 从"访问密钥"中复制的存储帐户名称和密钥
    - 创建云见证服务器，从"故障转移群集管理器"，并使用上述的帐户名称和密钥来创建它。

16. 运行[群集验证测试](../../failover-clustering/create-failover-cluster.md#validate-the-configuration)再进行下一步。

17. 启动 Windows PowerShell，并使用 [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) cmdlet 确定是否满足所有存储副本要求。 若要快速测试以及长时间运行性能评估模式下，可以在仅要求模式下使用 cmdlet。

18. 配置群集到群集存储副本。
   
   授予访问权限在不同的群集中两个方向上的另一个群集：

   在我们的示例：

   ```PowerShell
      Grant-SRAccess -ComputerName az2az1 -Cluster SRAZC2
   ```
如果您使用 Windows Server 2016，然后也运行此命令：

   ```PowerShell
      Grant-SRAccess -ComputerName az2az3 -Cluster SRAZC1
   ```   
   
19. 为群集创建 SRPartnership:</ol>

 - 为群集**SRAZC1**。
   - 卷的位置:-c:\ClusterStorage\DataDisk1
   - 日志位置:-g:
 - 为群集**SRAZC2**
    - 卷的位置:-c:\ClusterStorage\DataDisk2
    - 日志位置:-g:

运行下面的命令：

```PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName **SRAZC2** -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDisk2 -DestinationLogVolumeName  g:
```
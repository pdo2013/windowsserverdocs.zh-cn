---
title: 在 Azure 中的同一区域内群集到群集存储副本
description: 在 Azure 中的同一区域内群集到群集存储的复制
keywords: 存储副本、服务器管理器、Windows Server、Azure、群集、同一区域
author: arduppal
ms.author: arduppal
ms.date: 04/26/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 55d9c600c86b6b64efdb5c7d4437697539f887ae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402948"
---
# <a name="cluster-to-cluster-storage-replica-within-the-same-region-in-azure"></a>在 Azure 中的同一区域内群集到群集存储副本

> 适用于：Windows Server 2019、Windows Server 2016、Windows Server（半年频道）

可以在 Azure 中的同一区域内将群集配置为群集存储复制。 在下面的示例中，我们使用双节点群集，但群集到群集的存储副本并不局限于双节点群集。 下图是一个双节点存储空间直通群集，可以彼此通信、位于同一个域中以及位于同一区域中。

观看以下视频，了解过程的完整演练。

第一部分
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE26f2Y]

第二部分
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE269Pq]

![体系结构关系图在同一区域中的 Azure 中展示群集到群集的存储副本。](media/Cluster-to-cluster-azure-one-region/architecture.png)
> [!IMPORTANT]
> 所有引用的示例均特定于上图。

1. 在区域的 Azure 门户中创建[资源组](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup)（**美国西部 2**中的**AZ2AZ** ）。 
2. 在上面创建的资源组（**AZ2AZ**）中创建两个[可用性集](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM)，一个用于每个群集。 
    a. 可用性集（**az2azAS1**） b。 可用性集（**az2azAS2**）
3. 在以前创建的资源组（**az2az**）中创建[虚拟网络](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM)（**az2az**），其中至少有一个子网。 
4. 创建[网络安全组](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM)（**az2az-NSG**），并为 RDP：3389添加一个入站安全规则。 完成设置后，可以选择删除此规则。 
5. 在以前创建的资源组中创建 Windows Server[虚拟机](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM)（**AZ2AZ**）。 使用之前创建的虚拟网络（**az2az**）和网络安全组（**az2az-NSG**）。 
   
   域控制器（**az2azDC**）。 你可以选择为域控制器创建第三个可用性集，或者在两个可用性集之一中添加域控制器。 如果要将此添加到为这两个群集创建的可用性集，请在创建 VM 的过程中为其分配一个标准公共 IP 地址。 
   - 安装 Active Directory 域服务。
   - 创建域（Contoso.com）
   - 使用管理员权限创建用户（contosoadmin） 
   - 在第一个可用性集（**az2azAS1**）中创建两个虚拟机（**az2az1**， **az2az2**）。 在创建过程中将标准公共 IP 地址分配给每个虚拟机。
   - 向每台计算机至少添加2个托管磁盘
   - 安装故障转移群集和存储副本功能
   - 在第二个可用性集（**az2azAS2**）中创建两个虚拟机（**az2az3**， **az2az4**）。 在创建过程中将标准公共 IP 地址分配给每个虚拟机。 
   - 向每台计算机至少添加2个托管磁盘。 
   - 安装故障转移群集和存储副本功能。 
   
6. 将所有节点连接到域，并向之前创建的用户提供管理员权限。 

7. 将虚拟网络的 DNS 服务器更改为域控制器专用 IP 地址。 
8. 在本示例中，域控制器**az2azDC**有专用 IP 地址（10.3.0.8）。 在虚拟网络（**az2az**）中，更改 DNS 服务器10.3.0.8。 将所有节点连接到 "Contoso.com"，并向 "contosoadmin" 提供管理员权限。
   - 从所有节点以 contosoadmin 的身份登录。 
    
9. 创建群集（**SRAZC1**， **SRAZC2**）。 
   下面是示例的 PowerShell 命令
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
   
    对于每个群集，请创建虚拟磁盘和卷。 一个用于数据，另一个用于日志。 
   
11. 为每个群集创建内部标准 SKU[负载均衡器](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM)（**azlbr1**，**azlbr2**）。 
   
    提供群集 IP 地址作为负载均衡器的静态专用 IP 地址。
    - azlbr1 = > 前端 IP：10.3.0.100 （从虚拟网络（**az2az**）子网中获取未使用的 IP 地址）
    - 为每个负载均衡器创建后端池。 添加关联的群集节点。
    - 创建运行状况探测：端口59999
    - 创建负载均衡规则：允许启用了浮动 IP 的 HA 端口。 
   
    提供群集 IP 地址作为负载均衡器的静态专用 IP 地址。
    - azlbr2 = > 前端 IP：10.3.0.101 （从虚拟网络（**az2az**）子网中获取未使用的 IP 地址）
    - 为每个负载均衡器创建后端池。 添加关联的群集节点。
    - 创建运行状况探测：端口59999
    - 创建负载均衡规则：允许启用了浮动 IP 的 HA 端口。 
   
12. 在每个群集节点上，打开端口59999（运行状况探测）。 
   
    在每个节点上运行以下命令：
    ```PowerShell
    netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
    ```   
13. 指示群集侦听端口59999上的运行状况探测消息，并从当前拥有此资源的节点响应。 
    对于每个群集，从群集的任何一个节点运行一次。 
    
    在我们的示例中，请确保根据你的配置值更改 "ILBIP"。 从任何一个节点**az2az1**/**az2az2**运行以下命令：

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}
    ```

14. 从任何一个节点**az2az3**/**az2az4**运行以下命令。 

    ```PowerShell
    $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
    $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
    $ILBIP = "10.3.0.101" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
    [int]$ProbePort = 59999
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```   
    确保两个群集之间可以相互连接/通信。 

    使用故障转移群集管理器中的 "连接到群集" 功能连接到其他群集，或检查其他群集是否响应了当前群集的某个节点。  
   
    ```PowerShell
     Get-Cluster -Name SRAZC1 (ran from az2az3)
    ```
    ```PowerShell
     Get-Cluster -Name SRAZC2 (ran from az2az1)
    ```   

15. 为两个群集创建 cloud 见证服务器。 在 azure 中，为同一资源组中的每个群集创建两个[存储帐户](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM)（**az2azcw**， **az2azcw2**）（**sr-iov**）。

    - 从 "访问密钥" 复制存储帐户名称和密钥
    - 从 "故障转移群集管理器" 创建云见证，并使用上述帐户名称和密钥来创建云见证。

16. 请先运行[群集验证测试](../../failover-clustering/create-failover-cluster.md#validate-the-configuration)，然后再继续下一步。

17. 启动 Windows PowerShell，并使用 [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) cmdlet 确定是否满足所有存储副本要求。 可以在仅要求模式下使用 cmdlet 进行快速测试，还可以使用长时间运行的性能评估模式。

18. 配置群集到群集存储副本。
   
    双向向另一个群集授予访问权限：

    在本示例中：

    ```PowerShell
      Grant-SRAccess -ComputerName az2az1 -Cluster SRAZC2
    ```
    如果使用的是 Windows Server 2016，请运行以下命令：

    ```PowerShell
      Grant-SRAccess -ComputerName az2az3 -Cluster SRAZC1
    ```   
   
19. 为群集创建 Remove-srpartnership：</ol>

    - 对于群集**SRAZC1**。
    - 卷位置：-c:\ClusterStorage\DataDisk1
    - 日志位置：-g：
    - 对于群集**SRAZC2**
    - 卷位置：-c:\ClusterStorage\DataDisk2
    - 日志位置：-g：

运行下面的命令：

```PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName **SRAZC2** -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDisk2 -DestinationLogVolumeName  g:
```
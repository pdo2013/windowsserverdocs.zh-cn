---
title: Azure 中跨区域群集到群集存储副本
description: 群集到群集存储复制跨区域的 Azure
keywords: 存储副本、服务器管理器、Windows Server、Azure、群集、跨区域、不同的区域
author: arduppal
ms.author: arduppal
ms.date: 12/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 8c3bf68f606a9016295649efa69edc47eab7d4f7
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869777"
---
# <a name="cluster-to-cluster-storage-replica-cross-region-in-azure"></a>Azure 中跨区域群集到群集存储副本

> 适用于：Windows Server 2019、Windows Server 2016、Windows Server（半年频道）

可以为 Azure 中的跨区域应用程序配置群集以群集存储副本。 在下面的示例中，我们使用双节点群集，但群集到群集的存储副本并不局限于双节点群集。 下图是一个双节点存储空间直通群集，可以相互通信，位于同一个域中，并且是跨区域。

观看以下视频，了解整个过程。
> [!video https://www.microsoft.com/en-us/videoplayer/embed/RE26xeW]

![体系结构关系图展示 C2C SR in Azure 限相同区域。](media/Cluster-to-cluster-azure-cross-region/architecture.png)
> [!IMPORTANT]
> 所有引用的示例均特定于上图。


1. 在 Azure 门户中，在两个不同的区域中创建[资源组](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup)。

    例如，在美国西部**2**和**AZCROSS** **中的** **AZ2AZ** ，如上所述。

2. 为每个群集创建两个[可用性集](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM)，每个资源组中一个。
    - 中的可用性集（**az2azAS1**）（**AZ2AZ**）
    - 中的可用性集（**azcross**）（**azcross**）

3. 创建两个虚拟网络
   - 在第一个资源组（**az2az**）中创建[虚拟网络](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM)（**az2az**），其中包含一个子网和一个网关子网。
   - 在第二个资源组（**azcross**）中创建[虚拟网络](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM)（**azcross**），其中包含一个子网和一个网关子网。

4. 创建两个网络安全组
   - 在第一个资源组（**az2az**）中创建[网络安全组](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM)（**az2az-NSG**）。
   - 在第二个资源组（**azcross**）中创建[网络安全组](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM)（**azcross-NSG**）。

   向两个网络安全组添加一个用于 RDP：3389的入站安全规则。 完成设置后，可以选择删除此规则。

5. 在以前创建的资源组中创建 Windows Server[虚拟机](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM)。

   域控制器（**az2azDC**）。 你可以选择为域控制器创建第三个可用性集，或者在两个可用性集中的一个中添加域控制器。 如果要将此添加到为这两个群集创建的可用性集，请在创建 VM 的过程中为其分配一个标准公共 IP 地址。
      - 安装 Active Directory 域服务。
      - 创建域（contoso.com）
      - 使用管理员权限创建用户（contosoadmin）

   使用可用性集中的虚拟网络（**AZ2AZ**）和网络安全组 **（AZ2AZ** **-NSG**）在资源组（**sr-iov-AZ2AZ**）中创建两个虚拟机（**az2az1**， **az2az2**）。 在创建过程中将标准公共 IP 地址分配给每个虚拟机。
      - 向每台计算机添加至少两个托管磁盘
      - 安装故障转移群集和存储副本功能

   使用可用性集中的虚拟网络（**AZCROSS**）和网络安全组 **（AZCROSS-** **NSG**）在资源组（**sr-iov-AZCROSS**）中创建两个虚拟机（**azcross1**， **azcross2**）. 在创建过程中将标准公共 IP 地址分配给每个虚拟机
      - 向每台计算机添加至少两个托管磁盘
      - 安装故障转移群集和存储副本功能

   将所有节点连接到域，并向之前创建的用户提供管理员权限。

   将虚拟网络的 DNS 服务器更改为域控制器专用 IP 地址。
   - 在此示例中，域控制器**az2azDC**具有专用 IP 地址（10.3.0.8）。 在虚拟网络（**az2az**和**azcross**）中，更改 DNS 服务器10.3.0.8。 

     在此示例中，将所有节点连接到 "contoso.com"，并向 "contosoadmin" 提供管理员权限。
   - 从所有节点以 contosoadmin 的身份登录。 
 
6. 创建群集（**SRAZC1**， **SRAZCross**）。

   下面是示例的 PowerShell 命令
   ```powershell
      New-Cluster -Name SRAZC1 -Node az2az1,az2az2 –StaticAddress 10.3.0.100
   ```
   ```powershell
      New-Cluster -Name SRAZCross -Node azcross1,azcross2 –StaticAddress 10.0.0.10
   ```

7. 启用存储空间直通。

   ```powershell
      Enable-clusterS2D
   ```

   > [!NOTE]
   > 对于每个群集，请创建虚拟磁盘和卷。 一个用于数据，另一个用于日志。

8. 为每个群集创建内部标准 SKU[负载均衡器](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM)（**azlbr1**， **azlbazcross**）。

   提供群集 IP 地址作为负载均衡器的静态专用 IP 地址。
      - azlbr1 = > 前端 IP：10.3.0.100 （从虚拟网络（**az2az**）子网中获取未使用的 IP 地址）
      - 为每个负载均衡器创建后端池。 添加关联的群集节点。
      - 创建运行状况探测：端口59999
      - 创建负载均衡规则：允许启用了浮动 IP 的 HA 端口。

   提供群集 IP 地址作为负载均衡器的静态专用 IP 地址。 
      - azlbazcross = > 前端 IP：10.0.0.10 （从虚拟网络（**azcross**）子网中获取未使用的 IP 地址）
      - 为每个负载均衡器创建后端池。 添加关联的群集节点。
      - 创建运行状况探测：端口59999
      - 创建负载均衡规则：允许启用了浮动 IP 的 HA 端口。 

9. 为 Vnet 到 Vnet 连接创建[虚拟网络网关](https://ms.portal.azure.com/#create/Microsoft.VirtualNetworkGateway-ARM)。

   - 在第一个资源组中创建第一个虚拟网络网关（**az2az-VNetGateway**）（**sr-iov**）
   - 网关类型 = VPN，和 VPN 类型 = 基于路由

   - 在第二个资源组（**sr-iov-azcross**）中创建第二个虚拟网络网关（**azcross-VNetGateway**）
   - 网关类型 = VPN，和 VPN 类型 = 基于路由

   - 创建从第一个虚拟网络网关到第二个虚拟网络网关的 Vnet 到 Vnet 连接。 提供共享密钥

   - 创建从第二个虚拟网络网关到第一个虚拟网络网关的 Vnet 到 Vnet 连接。 提供前面步骤中提供的相同共享密钥。 

10. 在每个群集节点上，打开端口59999（运行状况探测）。

    在每个节点上运行以下命令：

    ```powershell
      netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
    ```

11. 指示群集侦听端口59999上的运行状况探测消息，并从当前拥有此资源的节点响应。

    对于每个群集，从群集的任何一个节点运行一次。 
    
    在我们的示例中，请确保根据你的配置值更改 "ILBIP"。 从任何一个节点**az2az1**/**az2az2**运行以下命令

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```

12. 从任何一个节点**azcross1**/**azcross2**运行以下命令
    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.0.0.10" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```

    确保两个群集之间可以相互连接/通信。

    使用故障转移群集管理器中的 "连接到群集" 功能连接到其他群集，或检查其他群集是否响应了当前群集的某个节点。

    从我们一直使用的示例：
    ```powershell
      Get-Cluster -Name SRAZC1 (ran from azcross1)
    ```
    ```powershell
      Get-Cluster -Name SRAZCross (ran from az2az1) 
    ```

13. 为两个群集创建云见证。 在 Azure 中创建两个[存储帐户](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM)（**az2azcw**，**azcrosssa**），每个资源组中的每个群集都有一个。
   
    - 从 "访问密钥" 复制存储帐户名称和密钥
    - 从 "故障转移群集管理器" 创建云见证，并使用上述帐户名称和密钥来创建云见证。 

14. 在继续下一步之前运行[群集验证测试](../../failover-clustering/create-failover-cluster.md#validate-the-configuration)

15. 启动 Windows PowerShell，并使用 [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) cmdlet 确定是否满足所有存储副本要求。 可以在仅要求模式下使用 cmdlet 以用于快速测试，也可以在长时间运行的性能评估模式下使用。
 
16. 配置群集到群集存储副本。
    双向向另一个群集授予访问权限：

    在我们的示例中：
    ```powershell
     Grant-SRAccess -ComputerName az2az1 -Cluster SRAZCross
    ```
    如果使用的是 Windows Server 2016，请运行以下命令：

    ```powershell
     Grant-SRAccess -ComputerName azcross1 -Cluster SRAZC1
    ```

17. 为这两个群集创建 SR 合作关系：</ol>

    - 对于群集**SRAZC1**
      - 卷位置：-c:\ClusterStorage\DataDisk1
      - 日志位置：-g：
    - 对于群集**SRAZCross**
      - 卷位置：-c:\ClusterStorage\DataDiskCross
      - 日志位置：-g：

运行命令：

```powershell
PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName SRAZCross -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDiskCross -DestinationLogVolumeName  g:
```
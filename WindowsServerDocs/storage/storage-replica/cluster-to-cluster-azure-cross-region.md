---
title: Azure 中跨区域群集到群集存储副本
description: 群集到群集存储复制跨 Azure 中的区域
keywords: 存储副本、 服务器管理器、 Windows Server、 Azure 中，群集中，跨区域，不同的区域
author: arduppal
ms.author: arduppal
ms.date: 12/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 95f3e9e929d6a28279f526eec6dbdf0427bd74c0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447582"
---
# <a name="cluster-to-cluster-storage-replica-cross-region-in-azure"></a>Azure 中跨区域群集到群集存储副本

> 适用于：Windows Server 2019，Windows Server 2016 中，Windows Server （半年频道）

可以在 Azure 中配置群集到群集存储副本跨区域应用程序。 在下面的示例中，我们使用双节点群集，但群集到群集存储副本不会被限制到两个节点群集。 下图是可以相互通信，在同一个域中和跨区域的双节点存储空间直通群集。

观看以下视频，该过程的完整演练。
> [!video https://www.microsoft.com/en-us/videoplayer/embed/RE26xeW]

![体系结构关系图展示在 Azure 中的 C2C SR 同一区域。](media/Cluster-to-cluster-azure-cross-region/architecture.png)
> [!IMPORTANT]
> 所有被引用的示例是特定于上图。


1. 在 Azure 门户中创建[资源组](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup)在两个不同区域中。

    例如， **SR AZ2AZ**中**美国西部 2**并**SR AZCROSS**中**美国中西部**，如上所示。

2. 创建两个[可用性集](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM)，每个资源组的每个群集中。
    - 可用性集 (**az2azAS1**) 中 (**SR AZ2AZ**)
    - 可用性集 (**azcross-AS**) 中 (**SR AZCROSS**)

3. 创建两个虚拟网络
   - 创建[虚拟网络](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM)(**az2az Vnet**) 的第一个资源组中 (**SR AZ2AZ**)，具有一个子网和一个网关子网。
   - 创建[虚拟网络](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM)(**azcross VNET**) 的第二个资源组中 (**SR AZCROSS**)，具有一个子网和一个网关子网。

4. 创建两个网络安全组
   - 创建[网络安全组](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM)(**az2az NSG**) 的第一个资源组中 (**SR AZ2AZ**)。
   - 创建[网络安全组](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM)(**azcross NSG**) 的第二个资源组中 (**SR AZCROSS**)。

   将 RDP:3389 的一个入站的安全规则添加到这两个网络安全组。 您可以选择您的安装程序完成后删除此规则。

5. 创建 Windows Server[虚拟机](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM)以前创建的资源组中。

   域控制器 (**az2azDC**)。 您可以选择创建你的域控制器是第三个可用性集或的域控制器添加两个可用性集之一。 如果您将这添加到创建两个群集的可用性集，将其分配标准公共 IP 地址在 VM 创建过程。
      - 安装 Active Directory 域服务。
      - 创建一个域 (contoso.com)
      - 创建具有管理员权限 (contosoadmin) 的用户

   创建两个虚拟机 (**az2az1**， **az2az2**) 的资源组中 (**SR AZ2AZ**) 使用虚拟网络 (**az2az Vnet**) 和网络安全组 (**az2az NSG**) 在可用性集中 (**az2azAS1**)。 将标准公共 IP 地址分配给每个虚拟机本身在创建过程。
      - 将至少两个托管的磁盘添加到每台计算机
      - 安装故障转移群集和存储副本功能

   创建两个虚拟机 (**azcross1**， **azcross2**) 的资源组中 (**SR AZCROSS**) 使用虚拟网络 (**azcross VNET**) 和网络安全组 (**azcross NSG**) 在可用性集中 (**azcross-AS**)。 本身期间将标准公共 IP 地址分配给每个虚拟机
      - 将至少两个托管的磁盘添加到每台计算机
      - 安装故障转移群集和存储副本功能

   连接到域的所有节点，并向以前创建的用户提供管理员权限。

   更改为域控制器的专用 IP 地址的虚拟网络的 DNS 服务器。
   - 在示例中，域控制器**az2azDC**具有专用 IP 地址 (10.3.0.8)。 虚拟网络中 (**az2az Vnet**并**azcross VNET**) 更改 10.3.0.8 的 DNS 服务器。 

     在示例中，连接到"contoso.com"的所有节点并提供对"contosoadmin"的管理员特权。
   - 从所有节点 contosoadmin 以用户身份登录。 
 
6. 创建群集 (**SRAZC1**， **SRAZCross**)。

   下面是示例 PowerShell 命令
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
   > 对于每个群集创建虚拟磁盘和卷。 一个用于数据，另一个用于日志。

8. 创建内部的标准 SKU[负载均衡器](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM)每个分类 (**azlbr1**， **azlbazcross**)。

   提供的群集 IP 地址作为静态专用 IP 地址的负载均衡器。
      - azlbr1 = > 前端 IP:10.3.0.100 (选取虚拟网络中未使用的 IP 地址 (**az2az Vnet**) 子网)
      - 为每个负载均衡器创建后端池。 添加关联的群集节点。
      - 创建运行状况探测： 端口 59999
      - 创建负载均衡规则：允许使用已启用浮动 IP HA 端口。

   提供的群集 IP 地址作为静态专用 IP 地址的负载均衡器。 
      - azlbazcross = > 前端 IP:10.0.0.10 (选取虚拟网络中未使用的 IP 地址 (**azcross VNET**) 子网)
      - 为每个负载均衡器创建后端池。 添加关联的群集节点。
      - 创建运行状况探测： 端口 59999
      - 创建负载均衡规则：允许使用已启用浮动 IP HA 端口。 

9. 创建[虚拟网络网关](https://ms.portal.azure.com/#create/Microsoft.VirtualNetworkGateway-ARM)用于 Vnet 到 Vnet 连接。

   - 创建第一个虚拟网络网关 (**az2az VNetGateway**) 的第一个资源组中 (**SR AZ2AZ**)
   - 网关类型 = VPN 和 VPN 类型 = 基于路由

   - 创建第二个虚拟网络网关 (**azcross VNetGateway**) 的第二个资源组中 (**SR AZCROSS**)
   - 网关类型 = VPN 和 VPN 类型 = 基于路由

   - 从第一个虚拟网络网关以第二个虚拟网络网关创建 Vnet 到 Vnet 连接。 提供共享的密钥

   - 创建从第二个虚拟网络的网关到第一个虚拟网络网关的 Vnet 到 Vnet 连接。 在前面步骤中所述提供相同的共享的密钥。 

10. 在每个群集节点上打开端口 59999 （运行状况探测）。

    每个节点上运行以下命令：

    ```powershell
      netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
    ```

11. 指示要侦听的运行状况探测端口 59999 上的消息和响应来自当前拥有此资源的节点的群集。

    运行一次从群集中，为每个群集的任何一个节点。 
    
    在我们的示例，请确保更改"ILBIP"根据你的配置值。 从任何一个节点上运行以下命令**az2az1**/**az2az2**

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```

12. 从任何一个节点上运行以下命令**azcross1**/**azcross2**
    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.0.0.10" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```

    请确保这两个群集可以连接 / 相互通信。

    可以使用故障转移群集管理器中"连接到群集"功能来连接到其他群集或检查其他群集响应来自当前群集的节点之一。

    示例中，我们已使用：
    ```powershell
      Get-Cluster -Name SRAZC1 (ran from azcross1)
    ```
    ```powershell
      Get-Cluster -Name SRAZCross (ran from az2az1) 
    ```

13. 创建两个群集的云见证。 创建两个[存储帐户](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM)(**az2azcw**，**azcrosssa**) 在 Azure 中，一个用于每个资源组中每个分类 (**SR AZ2AZ**， **SR-AZCROSS**)。
   
    - 从"访问密钥"中复制的存储帐户名称和密钥
    - 创建云见证服务器，从"故障转移群集管理器"，并使用上述的帐户名称和密钥来创建它。 

14. 运行[群集验证测试](../../failover-clustering/create-failover-cluster.md#validate-the-configuration)再进行下一步

15. 启动 Windows PowerShell，并使用 [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) cmdlet 确定是否满足所有存储副本要求。 可以在仅要求模式下使用 cmdlet 以用于快速测试，也可以在长时间运行的性能评估模式下使用。
 
16. 配置群集到群集存储副本。
    授予访问权限在不同的群集中两个方向上的另一个群集：

    从我们的示例：
    ```powershell
     Grant-SRAccess -ComputerName az2az1 -Cluster SRAZCross
    ```
    如果您使用 Windows Server 2016，然后也运行此命令：

    ```powershell
     Grant-SRAccess -ComputerName azcross1 -Cluster SRAZC1
    ```

17. 创建两个群集 SR 合作关系：</ol>

    - 为群集**SRAZC1**
      - 卷的位置:-c:\ClusterStorage\DataDisk1
      - 日志位置:-g:
    - 为群集**SRAZCross**
      - 卷的位置:-c:\ClusterStorage\DataDiskCross
      - 日志位置:-g:

运行命令：

```powershell
PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName SRAZCross -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDiskCross -DestinationLogVolumeName  g:
```
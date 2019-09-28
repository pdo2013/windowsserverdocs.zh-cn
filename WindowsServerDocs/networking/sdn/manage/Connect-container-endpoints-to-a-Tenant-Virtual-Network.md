---
title: 将容器终结点连接到租户虚拟网络
description: 在本主题中，我们将介绍如何将容器终结点连接到通过 SDN 创建的现有租户虚拟网络。 可使用适用于 Docker 的 Windows libnetwork 插件的 l2bridge （和 l2tunnel）网络驱动程序在租户 VM 上创建容器网络。
manager: ravirao
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f7af1eb6-d035-4f74-a25b-d4b7e4ea9329
ms.author: pashort
author: jmesser81
ms.date: 08/24/2018
ms.openlocfilehash: 83996f7ffb82d01c9f36945efa022f0dd0b9825b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355816"
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>将容器终结点连接到租户虚拟网络

>适用于：Windows Server（半年频道）、Windows Server 2016

在本主题中，我们将介绍如何将容器终结点连接到通过 SDN 创建的现有租户虚拟网络。 可使用适用于 Docker 的 Windows libnetwork 插件的*l2bridge* （和*l2tunnel*）网络驱动程序在租户 VM 上创建容器网络。

在[容器网络驱动程序](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/network-drivers-topologies)主题中，我们讨论了多个网络驱动程序通过 Windows 上的 Docker 提供。 对于 SDN，请使用*l2bridge*和*l2tunnel*驱动程序。 对于这两个驱动程序，每个容器终结点都与容器主机（租户）虚拟机位于同一虚拟子网中。 

主机网络服务（HNS）通过私有云插件动态分配容器终结点的 IP 地址。 容器终结点具有唯一的 IP 地址，但由于第2层地址转换而共享容器主机（租户）虚拟机的相同 MAC 地址。 

这些容器终结点的网络策略（Acl、封装和 QoS）在由网络控制器接收并在上层管理系统中定义的物理 Hyper-v 主机上强制实施。 

*L2bridge*和*l2tunnel*驱动程序的区别在于：


|                                                                                                                                                                                                                                                                            l2bridge                                                                                                                                                                                                                                                                            |                                                                                                 l2tunnel                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 驻留在上的容器终结点： <ul><li>同一容器主机虚拟机和同一子网中的所有网络流量都在 Hyper-v 虚拟交换机内桥接。 </li><li>不同的容器主机 Vm 或不同的子网将其流量转发到物理 Hyper-v 主机。 </li></ul>由于同一主机上和同一子网中的容器之间的网络流量不会流向物理主机，因此不会强制实施网络策略。 网络策略仅适用于跨主机或跨子网容器网络流量。 | 不管主机或子网如何，两个容器终结点之间的*所有*网络流量都将转发到物理 hyper-v 主机。 网络策略应用于跨子网和跨主机网络流量。 |

---

>[!NOTE]
>这些网络模式不适用于将 windows 容器终结点连接到 Azure 公有云中的租户虚拟网络。


## <a name="prerequisites"></a>先决条件
-  使用网络控制器部署的 SDN 基础结构。
-  租户虚拟网络已创建。
-  已启用 Windows 容器功能、已安装 Docker 和 Hyper-v 功能的已部署的租户虚拟机。 需要 Hyper-v 功能才能安装 l2bridge 和 l2tunnel 网络的多个二进制文件。

   ```powershell
   # To install HyperV feature without checks for nested virtualization
   dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All 
   ```

>[!Note]
>除非使用 Hyper-v 容器，否则不需要[嵌套虚拟化](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/nesting)和公开虚拟化扩展。 


## <a name="workflow"></a>工作流

[1.通过网络控制器（Hyper-v 主机） ](#1-add-multiple-ip-configurations) @ no__t-1 @ no__t-22 将多个 IP 配置添加到现有 VM NIC 资源。启用主机上的网络代理，为容器终结点（Hyper-v 主机）分配 CA IP 地址 ](#2-enable-the-network-proxy) @ no__t @ no__t-23。安装私有云插件以将 CA IP 地址分配到容器终结点（容器主机 VM） ](#3-install-the-private-cloud-plug-in) @ no__t @ no__t-24。使用 docker （容器主机 VM）创建*l2bridge*或*l2tunnel*网络 ](#4-create-an-l2bridge-container-network)

>[!NOTE]
>通过 System Center Virtual Machine Manager 创建的 VM NIC 资源不支持多个 IP 配置。 建议使用网络控制器 PowerShell 在带外创建 VM NIC 资源。

### <a name="1-add-multiple-ip-configurations"></a>1.添加多个 IP 配置
在此步骤中，我们假设租户虚拟机的 VM NIC 有一个 ip 地址为192.168.1.9 的 IP 配置，并附加到 192.168.1.0/24 IP 子网中的 VNet 资源 ID "VNet1" 和 "Subnet1" 的 VM 子网资源。 我们为 192.168.1.101-192.168.1.110 中的容器添加了10个 IP 地址。

```powershell
Import-Module NetworkController

# Specify Network Controller REST IP or FQDN
$uri = "<NC REST IP or FQDN>"
$vnetResourceId = "VNet1"
$vsubnetResourceId = "Subnet1"

$vmnic= Get-NetworkControllerNetworkInterface -ConnectionUri $uri | where {$_.properties.IpConfigurations.Properties.PrivateIPAddress -eq "192.168.1.9" }
$vmsubnet = Get-NetworkControllerVirtualSubnet -VirtualNetworkId $vnetResourceId -ResourceId $vsubnetResourceId -ConnectionUri $uri

# For this demo, we will assume an ACL has already been defined; any ACL can be applied here
$allowallacl = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"


foreach ($i in 1..10)
{
    $newipconfig = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
    $props = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties

    $resourceid = "IP_192_168_1_1"
    if ($i -eq 10) 
    {
        $resourceid += "10"
        $ipstr = "192.168.1.110"
    }
    else
    {
        $resourceid += "0$i"
        $ipstr = "192.168.1.10$i"
    }

    $newipconfig.ResourceId = $resourceid
    $props.PrivateIPAddress = $ipstr    

    $props.PrivateIPAllocationMethod = "Static"
    $props.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $props.Subnet.ResourceRef = $vmsubnet.ResourceRef
    $props.AccessControlList = new-object Microsoft.Windows.NetworkController.AccessControlList
    $props.AccessControlList.ResourceRef = $allowallacl.ResourceRef

    $newipconfig.Properties = $props
    $vmnic.Properties.IpConfigurations += $newipconfig
}

New-NetworkControllerNetworkInterface -ResourceId $vmnic.ResourceId -Properties $vmnic.Properties -ConnectionUri $uri
```

### <a name="2-enable-the-network-proxy"></a>2.启用网络代理
在此步骤中，将使网络代理为容器主机虚拟机分配多个 IP 地址。 

若要启用网络代理，请在托管容器主机（租户）虚拟机的**Hyper-v 主机**上运行[ConfigureMCNP](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1)脚本。

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="3-install-the-private-cloud-plug-in"></a>3.安装私有云插件
在此步骤中，你将安装一个插件，以允许 HNS 与 Hyper-v 主机上的网络代理通信。

若要安装该插件，请在**容器主机（租户）虚拟机**中运行[InstallPrivateCloudPlugin](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1)脚本。


```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="4-create-an-l2bridge-container-network"></a>4.创建*l2bridge*容器网络
在此步骤中，将使用**容器主机（租户）虚拟机**上的 `docker network create` 命令创建 l2bridge 网络。 

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork 
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>与 Microsoft SDN 堆栈一起使用时， *l2bridge*或*l2tunnel*容器网络不支持静态 IP 分配。

## <a name="more-information"></a>详细信息
有关部署 SDN 基础结构的更多详细信息，请参阅[部署软件定义的网络基础结构](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure)。


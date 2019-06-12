---
title: 将容器终结点连接到租户虚拟网络
description: 此主题，我们介绍如何将容器终结点连接到通过 SDN 中创建的现有租户虚拟网络。 使用 l2 桥接 （而且还可以 l2tunnel） 可与适用于 Docker 的租户 VM 上创建容器网络的 Windows libnetwork 插件的网络驱动程序。
manager: ravirao
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f7af1eb6-d035-4f74-a25b-d4b7e4ea9329
ms.author: pashort
author: jmesser81
ms.date: 08/24/2018
ms.openlocfilehash: cb9c7157ffb07233e41e1c933f6775f1cd0766a9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446348"
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>将容器终结点连接到租户虚拟网络

>适用于：Windows 服务器 （半年频道），Windows Server 2016

此主题，我们介绍如何将容器终结点连接到通过 SDN 中创建的现有租户虚拟网络。 您使用*l2 桥接*(并选择性地*l2tunnel*) 可与适用于 Docker 的租户 VM 上创建容器网络的 Windows libnetwork 插件的网络驱动程序。

在中[容器网络驱动程序](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/network-drivers-topologies)主题中，我们讨论了多个网络驱动程序可通过在 Windows 上的 Docker。 对于 SDN 的信息，请使用*l2 桥接*并*l2tunnel*驱动程序。 对于这两个驱动程序，每个容器终结点是在容器主机 （租户） 虚拟机位于同一虚拟子网中。 

主机网络服务 (HNS)，通过私有云插件动态地将容器终结点的 IP 地址。 容器终结点具有唯一的 IP 地址，但共享由于第 2 层地址转换的容器主机 （租户） 虚拟机的相同的 MAC 地址。 

这些容器终结点的网络策略 （Acl、 封装和 QoS） 是由网络控制器接收到在物理 HYPER-V 主机中强制实施和上层管理系统中定义。 

之间的差异*l2 桥接*并*l2tunnel*驱动程序：


|                                                                                                                                                                                                                                                                            l2bridge                                                                                                                                                                                                                                                                            |                                                                                                 l2tunnel                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 驻留在的容器终结点： <ul><li>同一个容器托管虚拟机和同一子网上将桥接中的 HYPER-V 虚拟交换机的所有网络流量。 </li><li>不同的容器主机 Vm，或在不同子网上将其流量转发到物理 HYPER-V 主机。 </li></ul>网络策略不会不获取强制执行，因为在同一主机上和相同的子网中的容器之间的网络流量就不会流入物理主机。 网络策略将应用仅为跨主机或跨子网的容器网络流量。 | *所有*两个容器终结点之间的网络流量转发到物理 HYPER-V 主机而不考虑主机或子网。 网络策略适用于跨子网和跨主机的网络流量。 |

---

>[!NOTE]
>这些网络模式不适用于在 Azure 公有云的租户虚拟网络的连接 windows 容器终结点。


## <a name="prerequisites"></a>先决条件
-  与网络控制器部署的 SDN 基础结构。
-  已创建租户虚拟网络。
-  部署的租户虚拟机使用 Windows 容器功能启用，Docker 安装，并启用 HYPER-V 功能。 需要安装多个二进制文件的 l2 桥接和 l2tunnel 网络的 HYPER-V 功能。

   ```powershell
   # To install HyperV feature without checks for nested virtualization
   dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All 
   ```

>[!Note]
>[嵌套虚拟化](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/nesting)和公开虚拟化扩展才不需要使用 HYPER-V 容器。 


## <a name="workflow"></a>工作流

[1.将多个 IP 配置添加到现有 VM NIC 资源通过网络控制器 （HYPER-V 主机）](#1-add-multiple-ip-configurations)
[2。启用主机将 CA IP 地址分配的容器终结点 （HYPER-V 主机） 上的网络代理](#2-enable-the-network-proxy)
[3。安装插件，以将 CA IP 地址分配给容器终结点 (容器主机 VM) 的私有云](#3-install-the-private-cloud-plug-in)
[4。创建*l2 桥接*或*l2tunnel*使用 docker (容器主机 VM) 网络](#4-create-an-l2bridge-container-network)

>[!NOTE]
>通过 System Center Virtual Machine Manager 中创建的 VM NIC 资源不支持多个 IP 配置。 建议为这些部署类型创建带外使用网络控制器 PowerShell VM NIC 资源。

### <a name="1-add-multiple-ip-configurations"></a>1.添加多个 IP 配置
在此步骤中，我们假定租户虚拟机的 VM NIC IP 地址的 192.168.1.9 的一个 IP 配置，并且已连接到 VNet 资源 ID 为 VNet1 和 Subnet1 的 VM 子网资源 192.168.1.0/24 IP 子网中。 我们添加用于容器的 10 个 IP 地址从 192.168.1.101-192.168.1.110。

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

### <a name="2-enable-the-network-proxy"></a>2.启用的网络代理
在此步骤中，启用要为容器主机虚拟机分配多个 IP 地址的网络代理。 

若要启用的网络代理，请运行[ConfigureMCNP.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1)上脚本**HYPER-V 主机**承载容器主机 （租户） 虚拟机。

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="3-install-the-private-cloud-plug-in"></a>3.安装插件的私有云
在此步骤中，安装插件允许 HNS 与 HYPER-V 主机上的网络代理进行通信。

若要安装该插件，请运行[InstallPrivateCloudPlugin.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1)脚本内**容器主机 （租户） 虚拟机**。


```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="4-create-an-l2bridge-container-network"></a>4.创建*l2 桥接*容器网络
在此步骤中，使用`docker network create`命令**容器主机 （租户） 虚拟机**创建 l2 桥接网络。 

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork 
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>不支持静态 IP 分配*l2 桥接*或*l2tunnel*容器网络与 Microsoft SDN 堆栈一起使用时。

## <a name="more-information"></a>详细信息
有关部署 SDN 基础结构的更多详细信息，请参阅[部署软件定义网络基础结构](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure)。


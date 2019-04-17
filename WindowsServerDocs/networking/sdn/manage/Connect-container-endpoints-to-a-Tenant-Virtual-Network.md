---
title: 连接到虚拟网络容器
description: 本主题介绍的软件定义网络指南如何管理租户工作负载和 Windows Server 2016 中的虚拟网络的一部分。
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
ms.openlocfilehash: 801cf4b8f71935eb72d820d47e523a310fa64562
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>连接到租户虚拟网络容器端点

>适用于：Windows Server（半年通道），Windows Server 2016

本主题演示如何在 Microsoft 软件定义网络 (SDN) 堆栈创建现有租户虚拟网络连接容器端点。 我们将使用*l2bridge* (还可以选择*l2tunnel*) 适用于 Windows libnetwork 插件 Docker 容器主机 （租户） 虚拟机上创建容器网络有关的网络驱动程序。

中所述[容器网络](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/management/container_networking)在 MSDN 上了解的主题，多个网络驱动程序都可以通过 Docker Windows 上。 最适合 SDN 驱动程序*l2bridge*和*l2tunnel*。 这两个驱动程序，每个容器端点处于容器主机 （租户） 虚拟机相同虚拟子网。 容器端点的 IP 地址是动态分配的主网络服务 (HNS) 通过专用云插件。 容器端点具有独特的 IP 地址，但共享由于第二层地址翻译容器主机 （租户） 虚拟机的相同的 MAC 地址。 网络策略 (例如： Acl、 封装和 QoS) 这些容器端点接收到网络控制器中的物理 HYPER-V 主机强制执行，并定义上层管理系统中。 没有略有差异*l2bridge*和*l2tunnel*如下所述的驱动程序。

- **L2 大桥**-容器端点位于同一容器主机虚拟机，用于在相同子网有桥 HYPER-V 虚拟交换机用来内的所有网络通信。 容器端点驻留在虚拟机的功能的其他容器主机上或这将在不同的子网将转发给物理 HYPER-V 主机其通信。 由于到物理主机不流之间容器在同一台主机和相同子网网络通信，实施没有网络策略。 仅适用于跨主机或交叉网容器网络通信策略。  
 
- **L2 隧道** - *所有*网络两个容器端点之间的通信到主机或子网无论物理 HYPER-V 主机。 交叉网和跨主机网络通信的情况下执行网络策略。   

>[!NOTE]
>这些网络模式下对租户虚拟采用与网络公共的 Azure 云连接 windows 容器端点不起作用

## <a name="prerequistes"></a>先决条件
 * 已部署 SDN 基础结构，与网络控制器
 * 创建了租户虚拟网络
 * 启用 Windows 容器功能、 Docker 安装和启用 HYPER-V 功能已部署租户虚拟机

>[!Note]
>[嵌套虚拟化](https://msdn.microsoft.com/en-us/virtualization/hyperv_on_windows/user_guide/nesting)和公开虚拟化扩展不需要使用 HYPER-V 容器 HyperV 功能不需要安装 l2bridge 和 l2tunnel 网络的几个二进制文件

```powershell
# To install HyperV feature without checks for nested virtualization
dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All 
```

 

## <a name="workflow"></a>流

1. [将多个 IP 配置添加到现有 VM NIC 资源通过网络控制器](#Add)（HYPER-V 主机）
2. [启用网络代理来容器端点分配 CA IP 地址的主机上](#Enable)（HYPER-V 主机） 
3. [安装专用云中插件将 CA IP 地址分配给容器端点](#Install)(容器主机 VM) 
4. [创建*l2bridge*或*l2tunnel*使用 docker 网络](#Create)(容器主机 VM) 
 
>[!NOTE]
>创建 System Center 虚拟机 Manager 通过 VM NIC 资源中不支持多个 IP 配置。 建议这些部署类型为你创建退出 band 使用网络控制器 PowerShell VM NIC 资源。

### <a name="Add"></a>1.添加多个 IP 配置

例如，我们 192.168.1.0/24 IP 子网假定租户虚拟机的 VM NIC 已经有一个 IP 配置 IP 地址与 192.168.1.9 并已连接到的 VNet1' VNet 资源 ID 和 VM 'Subnet1 子网资源。 我们将从 192.168.1.101-添加容器 10 IP 地址 192.168.1.110。

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

### <a name="Enable"></a>2.启用网络代理服务器

[ConfigureMCNP.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1>)

运行此脚本**HYPER-V 主机**这举办容器主机 （租户） 虚拟机启用网络代理分配容器主机虚拟机的多个 IP 地址。

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="Install"></a>3.安装专用云插件

[InstallPrivateCloudPlugin.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1)

运行此脚本内部**容器主机 （租户） 虚拟机**允许主机网络服务 (HNS) 网络代理 HYPER-V 主机上与进行通信。

```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="Create"></a>4.创建*l2bridge*容器网络

在**容器主机 （租户） 虚拟机**使用`docker network create`创建 l2bridge 网络命令

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork 
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>使用不受支持静态 IP 分配*l2bridge*或*l2tunnel*容器网络时使用 Microsoft SDN 堆栈。

## <a name="more-information"></a>详细信息
有关部署 SDN 基础结构的更多 infortation，请参阅[部署软件定义的网络结构](https://technet.microsoft.com/en-us/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure)。


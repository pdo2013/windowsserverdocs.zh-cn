---
title: 管理 Azure IaaS 虚拟机
description: 使用 Windows Admin Center (Project Honolulu) 管理 Azure IaaS 虚拟机
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6f162294076d7e12df09f31b0bbd7d9244abe679
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296883"
---
# 管理 Azure IaaS 虚拟机使用 Windows Admin Center

你可以使用 Windows Admin Center 管理 Azure 虚拟机以及在本地计算机。 有几个不同的配置可能-选择适合你的环境的配置：
- [从本地 Windows Admin Center 网关管理 Azure 虚拟机](#manage-with-an-on-premises-windows-admin-center-gateway)
- [从 Azure VM 上安装 Windows Admin Center 网关管理 Azure 虚拟机](#use-a-windows-admin-center-gateway-deployed-in-azure)

## 管理与本地 Windows Admin Center 网关

如果你已经安装了 Windows Admin Center 上的本地的网关 （无论在 Windows 10 或 Windows Server 2016 上），你可以使用此相同的网关来管理 Windows 10 或 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2Azure 中的虚拟机。 

### 连接到具有一个公共 IP 的 Vm

如果你的目标虚拟机 (你想要使用 Windows Admin Center 管理 Vm) 的公共 Ip，将其添加到 Windows Admin Center 网关 IP 地址或 FQDN。 有要考虑的几个注意事项：

- 在 PowerShell 或命令提示符中运行以下命令在目标虚拟机上，必须为你的目标 VM 启用 WinRM 访问： `winrm quickconfig`
- 如果你尚未加入域的 Azure 虚拟机，VM 行为类似于在工作组服务器中，因此你将需要确保你考虑[使用 Windows Admin Center 在工作组中](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)。
- 为了让 Windows Admin Center 管理目标 VM，还必须通过 HTTP 启用 winrm 入站的连接端口 5985 以实现：
   1. 在目标要启用对来宾操作系统上的端口 5985 以实现的入站的连接的虚拟机上运行以下 PowerShell 脚本：   
`Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

   2. 你还必须在 Azure 网络中打开端口：

    - 选择你的 Azure 虚拟机，请选择**网络**，然后**添加入站的端口规则**。 
    - 确保**基本**选择顶部的**添加安全入站的规则**窗格。
    - 在**端口范围**字段中，输入**5985**。
    
    如果你的 Windows Admin Center 网关具有静态 IP，你可以选择只允许入站的 WinRM 访问从 Windows Admin Center 网关添加了安全性。
    若要执行此操作，请选择顶部的**添加安全入站的规则**窗格的**高级**。

    为**源**，选择**IP 地址**，然后输入对应于 Windows Admin Center 网关的源 IP 地址。

    - 对于**协议**选择**TCP**。
    - 其余部分可以保留为默认值。

> [!NOTE]
> 你必须创建自定义端口规则。 WinRM 端口规则由 Azure 网络使用端口 5986 （通过 HTTPS) 而不是 5985 （通过 HTTP)。 

### 连接到虚拟机，而无需公共 IP

如果你的目标 Azure 虚拟机没有公共 Ip，并且你想要从本地网络中部署 Windows Admin Center 网关管理这些虚拟机，你需要将你的本地网络已连接到目标虚拟机都 VNet 配置连接。 有三种方法可以执行此操作： ExpressRoute、 站点到站点 VPN 或点到站点 VPN。 [了解哪个连接选项有意义你的环境中。](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) 

>[!TIP]
>如果你想要使用点到站点 VPN 连接到 Azure VNet 管理 Azure 虚拟机中该 VNet Windows Admin Center 网关，你可以在 Windows Admin Center 中使用[Azure 网络适配器的](https://aka.ms/WACNetworkAdapter)功能。 若要执行此操作，连接到在其安装 Windows Admin Center 的服务器、 导航到网络工具，然后选择"添加 Azure 网络适配器"。 当您提供必要的详细信息，并单击"设置"，Windows Admin Center 将配置点到站点 VPN 到指定之后，你可以连接到和管理 Azure 虚拟机从你的本地 Windows Admin Center 网关 Azure VNet。

请确保 WinRM 在 PowerShell 或命令提示符中运行以下命令在目标虚拟机上，在你的目标虚拟机上运行： `winrm quickconfig`

如果你尚未加入域的 Azure 虚拟机，VM 行为类似于在工作组服务器中，因此你将需要确保你考虑[使用 Windows Admin Center 在工作组中](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)。

如果你遇到任何问题，请查阅[疑难解答 Windows Admin Center](../support/troubleshooting.md) ，以查看 （例如，如果你使用本地管理员帐户连接或不是已加入域的） 是否配置需要其他步骤。

## 使用在 Azure 中部署 Windows Admin Center 网关

你可以通过部署 Windows Admin Center 中在连接目标虚拟机 VNet 没有任何本地依赖关系管理 Azure 虚拟机。 

管理虚拟机在其部署 Windows Admin Center 网关 VNet 之外，你必须建立 VNet VNet Windows Admin Center 网关 VNet 和目标服务器 VNet 之间的连接。 你可以建立与 VNet 对等、 VNet VNet 连接或一个站点到站点连接此连接。 [详细了解有关哪些 VNet VNet 连接选项有意义你的环境中。](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

Windows Admin Center 可以安装在你的环境中现有或新部署虚拟机上。 Windows Admin Center 安装为选择的虚拟机必须具有一个公共 IP 和 DNS 名称。

[了解有关部署 Azure 中的 Windows Admin Center 网关](deploy-wac-in-azure.md)
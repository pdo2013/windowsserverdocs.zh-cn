---
title: 管理 Azure IaaS 虚拟机
description: 管理 Azure IaaS 虚拟机与 Windows Admin Center (项目 Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ac98f42c4ad5606cc8d2b142f209f9bdb2b9611c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445902"
---
# <a name="manage-azure-iaas-virtual-machines-with-windows-admin-center"></a>管理 Azure IaaS 虚拟机与 Windows Admin Center

可以使用 Windows Admin Center 来管理 Azure Vm 以及本地计算机上。 有多种不同的配置可能-选择适合你的环境的配置：
- [管理 Azure Vm 从本地 Windows Admin Center 网关](#manage-with-an-on-premises-windows-admin-center-gateway)
- [从 Azure VM 上安装 Windows Admin Center 网关管理的 Azure Vm](#use-a-windows-admin-center-gateway-deployed-in-azure)

## <a name="manage-with-an-on-premises-windows-admin-center-gateway"></a>使用本地 Windows Admin Center 网关管理

如果你已安装 Windows Admin Center 上的本地网关 （无论是在 Windows 10 或 Windows Server 2016 上），可以使用此同一个网关来管理 Windows 10 或 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2在 Azure 中的 Vm。 

### <a name="connecting-to-vms-with-a-public-ip"></a>连接到具有公共 IP 的 Vm

如果你的目标 Vm (你想要管理与 Windows Admin Center Vm) 的公共 Ip，将其添加到 Windows Admin Center 网关 IP 地址或 FQDN。 有几个注意事项需要考虑：

- 在 PowerShell 或命令提示符中运行以下命令在目标 VM 上，必须为目标 VM 启用 WinRM 访问权限： `winrm quickconfig`
- 如果你尚未加入域的 Azure VM，VM 类似于服务器在工作组中，因此将需要确保你考虑[在工作组中使用 Windows Admin Center](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)。
- 为了使 Windows Admin Center 来管理目标 VM，还必须通过 HTTP 启用 winrm 的入站的连接到端口 5985:
  1. 目标 VM，若要启用入站的连接到来宾 OS 上为端口 5985 上运行以下 PowerShell 脚本：   
     `Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

  2. 在 Azure 网络中，则还必须打开端口：

     - 选择 Azure VM，并选择**联网**，然后**添加入站的端口规则**。 
     - 请确保**基本**顶部的所选**添加入站的安全规则**窗格。
     - 在中**端口范围**字段中，输入**5985**。
    
     如果 Windows Admin Center 网关具有静态 IP，你可以选择只允许入站的 WinRM 访问从 Windows Admin Center 网关为了提高安全性。
     若要执行此操作，请选择**高级**顶部**添加入站的安全规则**窗格。

     有关**源**，选择**IP 地址**，然后输入 Windows Admin Center 网关与对应的源 IP 地址。

     - 有关**协议**选择**TCP**。
     - 其余部分可以保留为默认值。

> [!NOTE]
> 您必须创建自定义端口规则。 提供的 Azure 网络的 WinRM 端口规则使用端口 5986 （通过 HTTPS) 而不是 5985 （通过 HTTP)。 

### <a name="connecting-to-vms-without-a-public-ip"></a>连接到 Vm，无需公共 IP

如果你的目标 Azure Vm 没有公共 Ip，并且你想要从 Windows Admin Center 网关的本地网络中部署来管理这些 Vm，则需要配置你的本地网络才能连接到目标虚拟机在其的 VNet连接。 有 3 种方法可以执行此操作：ExpressRoute、 站点到站点 VPN 或点到站点 VPN。 [了解哪种连接选项有意义在环境中。](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) 

>[!TIP]
>如果你想要使用点到站点 VPN 连接到 Azure VNet，VNet，可以使用管理 Azure Vm 在 Windows Admin Center 网关连接[Azure 网络适配器](https://aka.ms/WACNetworkAdapter)Windows Admin Center 中的新功能。 要执行此操作，连接到在其安装 Windows Admin Center 服务器、 导航到该网络工具并选择"添加 Azure 网络适配器"。 当您提供必要的详细信息，并单击"设置"，Windows Admin Center 将配置点到站点 VPN 到 Azure VNet 指定，此后，您可以连接到并管理 Azure Vm 从本地 Windows Admin Center 网关。

确保 WinRM 在 PowerShell 或命令提示符中运行以下命令在目标 VM 上，在你的目标 Vm 上运行： `winrm quickconfig`

如果你尚未加入域的 Azure VM，VM 类似于服务器在工作组中，因此将需要确保你考虑[在工作组中使用 Windows Admin Center](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)。

如果遇到任何问题，请查阅[进行故障排除 Windows Admin Center](../support/troubleshooting.md) （例如，如果你使用本地管理员帐户连接，或不是已加入域的） 是否配置所必需的其他步骤。

## <a name="use-a-windows-admin-center-gateway-deployed-in-azure"></a>使用在 Azure 中部署 Windows Admin Center 网关

可以通过部署 Windows Admin Center，连接目标虚拟机在 VNet 中没有任何本地依赖管理 Azure Vm。 

若要管理其部署 Windows Admin Center 网关的 VNet 外部的 Vm，必须建立 Windows Admin Center 网关的 VNet 和目标服务器的 VNet 之间的 VNet 到 VNet 连接。 您可以建立 VNet 对等互连、 VNet 到 VNet 连接或站点到站点连接与此连接。 [了解更多有关哪些 VNet 到 VNet 连接性选项有意义在环境中。](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

可以在环境中现有或新部署的 VM 上安装 Windows Admin Center。 为 Windows Admin Center 安装选择的 VM 必须具有公共 IP 和 DNS 名称。

[详细了解如何部署在 Azure 中的 Windows Admin Center 网关](deploy-wac-in-azure.md)
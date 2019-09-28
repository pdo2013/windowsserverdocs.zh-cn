---
title: 管理 Azure IaaS 虚拟机
description: 通过 Windows 管理中心管理 Azure IaaS Vm （Project Honolulu）
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 7b85f64d108283d4865b718b565ad3ba40f14f02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357337"
---
# <a name="manage-azure-iaas-virtual-machines-with-windows-admin-center"></a>通过 Windows 管理中心管理 Azure IaaS 虚拟机

可以使用 Windows Admin Center 来管理 Azure VM 以及本地计算机。 可能有多个不同的配置-请选择对你的环境有意义的配置：
- [从本地 Windows 管理中心网关管理 Azure Vm](#manage-with-an-on-premises-windows-admin-center-gateway)
- [从安装在 Azure VM 上的 Windows 管理中心网关上管理 Azure Vm](#use-a-windows-admin-center-gateway-deployed-in-azure)

## <a name="manage-with-an-on-premises-windows-admin-center-gateway"></a>使用本地 Windows 管理中心网关进行管理

如果已在本地网关（在 Windows 10 或 Windows Server 2016 上）上安装了 Windows 管理中心，则可以使用此相同的网关来管理 Windows 10 或 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2Azure 中的 Vm。 

### <a name="connecting-to-vms-with-a-public-ip"></a>使用公共 IP 连接到 Vm

如果目标 Vm （要使用 Windows 管理中心管理的 Vm）具有公共 Ip，请按 IP 地址或 FQDN 将它们添加到 Windows 管理中心网关。 需要考虑几个注意事项：

- 必须通过在 PowerShell 中运行以下命令，或在目标 VM 上的命令提示符中运行以下命令来启用对目标 VM 的 WinRM 访问： `winrm quickconfig`
- 如果尚未将 Azure VM 加入域，则 VM 的行为类似于工作组中的服务器，因此需要确保考虑[在工作组中使用 Windows 管理中心](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)。
- 还必须启用到 HTTP 的端口5985的入站连接，Windows 管理中心才能通过 HTTP 管理目标 VM：
  1. 在目标 VM 上运行以下 PowerShell 脚本，以启用到来宾操作系统上端口5985的入站连接：   
     `Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

  2. 还必须在 Azure 网络中打开端口：

     - 选择 Azure VM，选择 "**网络**"，然后单击 "**添加入站端口规则**"。 
     - 确保在 "**添加入站安全规则**" 窗格的顶部选择 "**基本**"。
     - 在 "**端口范围**" 字段中，输入**5985**。
    
     如果 Windows 管理中心网关有静态 IP，则可以选择仅允许来自 Windows 管理中心网关的入站 WinRM 访问，以提高安全性。
     为此，请选择 "**添加入站安全规则**" 窗格顶部的 "**高级**"。

     对于 "**源**"，请选择 " **IP 地址**"，然后输入与 Windows 管理中心网关对应的源 IP 地址。

     - 对于**协议**，请选择**TCP**。
     - 其余部分可以保留为默认值。

> [!NOTE]
> 必须创建自定义端口规则。 Azure 网络提供的 WinRM 端口规则使用端口5986（over HTTPS）而不是5985（over HTTP）。 

### <a name="connecting-to-vms-without-a-public-ip"></a>连接到无公共 IP 的 Vm

如果目标 Azure Vm 没有公共 Ip，并且你想要从本地网络中部署的 Windows 管理中心网关管理这些 Vm，则需要配置本地网络，使其能够连接到目标 Vm 所在的 VNet联机. 可以通过三种方法执行此操作：ExpressRoute、站点到站点 VPN 或点到站点 VPN。 [了解哪种连接选项在您的环境中是有意义的。](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) 

>[!TIP]
>如果希望使用点到站点 VPN 将 Windows 管理中心网关连接到 Azure VNet 以管理该 VNet 中的 Azure Vm，则可以使用 Windows 管理中心中的[Azure 网络适配器](https://aka.ms/WACNetworkAdapter)功能。 为此，请连接到安装了 Windows 管理中心的服务器，导航到 "网络" 工具，然后选择 "添加 Azure 网络适配器"。 提供必要的详细信息并单击 "设置" 时，Windows 管理中心会将点到站点 VPN 配置为指定的 Azure VNet，此后，你可以从本地 Windows 管理中心网关上连接到 Azure Vm 并对其进行管理。

通过在 PowerShell 中运行以下命令或在目标 VM 上运行命令提示符，确保 WinRM 在目标 Vm 上运行： `winrm quickconfig`

如果尚未将 Azure VM 加入域，则 VM 的行为类似于工作组中的服务器，因此需要确保考虑[在工作组中使用 Windows 管理中心](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)。

如果遇到任何问题，请咨询[Windows 管理中心的疑难解答](../support/troubleshooting.md)，查看配置是否需要其他步骤（例如，如果你使用本地管理员帐户进行连接或未加入域）。

## <a name="use-a-windows-admin-center-gateway-deployed-in-azure"></a>使用 Azure 中部署的 Windows 管理中心网关

你可以通过在你的目标 Vm 连接到的 VNet 中部署 Windows 管理中心来管理 Azure Vm，而无需任何本地依赖。 

若要在部署了 Windows 管理中心网关的 VNet 之外管理 Vm，必须在 Windows 管理中心网关的 VNet 与目标服务器的 VNet 之间建立 VNet 到 VNet 的连接。 可以与 VNet 对等互连、VNet 到 VNet 连接或站点到站点连接建立此连接。 [详细了解在你的环境中有意义的 VNet 到 VNet 连接选项。](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

可以在环境中的现有或新部署的 VM 上安装 Windows 管理中心。 为 Windows 管理中心安装选择的 VM 必须具有公共 IP 和 DNS 名称。

[了解有关在 Azure 中部署 Windows 管理中心网关的详细信息](deploy-wac-in-azure.md)
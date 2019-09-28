---
title: 部署始终启用 VPN
description: 本主题提供有关在 Windows Server 2016 中部署 Always On VPN 的详细说明。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ad748de2-d175-47bf-b05f-707dc48692cf
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 054a41df281ff9720d381fd4854f34f56ed0307b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388188"
---
# <a name="deploy-always-on-vpn"></a>部署始终启用 VPN

>适用于：Windows Server (半年频道), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**以前**了解 Always On VPN 高级功能 @ no__t-0
- [**一个**步骤 1：开始规划 Always On 的 VPN 部署](always-on-vpn-deploy-planning.md)

本部分介绍如何为已加入域的 Windows 10 客户端计算机部署 Always On VPN 连接的工作流。 若要**配置条件性访问**以微调 vpn 用户访问资源的方式，请参阅[使用 Azure AD 进行 Vpn 连接的条件性访问](../../ad-ca-vpn-connectivity-windows10.md)。 若要了解有关使用 Azure AD 进行 VPN 连接的条件性访问的详细信息，请参阅[Azure Active Directory 中的条件访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。 

下图说明了部署 Always On VPN 时的不同方案的工作流程：

![Always On VPN 部署工作流的流程图](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)

>[!IMPORTANT]
>对于此部署，不要求您的基础结构服务器（例如运行 Active Directory 域服务、Active Directory 证书服务和网络策略服务器的计算机）运行的是 Windows Server 2016。 对于基础结构服务器以及运行远程访问的服务器，你可以使用 windows server 的早期版本，如 Windows Server 2012 R2。

## <a name="step-1-plan-the-always-on-vpn-deploymentalways-on-vpn-deploy-planningmd"></a>[步骤 1.规划始终启用 VPN 部署](always-on-vpn-deploy-planning.md)

在此步骤中，你将开始规划和准备 Always On VPN 部署。 在打算用作 VPN 服务器的计算机上安装远程访问服务器角色之前，请使用。 进行适当规划后, 可以部署 Always On VPN, 还可以选择使用 Azure AD 配置 VPN 连接的条件性访问。

## <a name="step-2-configure-the-always-on-vpn-server-infrastructurevpn-deploy-server-infrastructuremd"></a>[步骤 2.配置始终启用 VPN 服务器基础结构](vpn-deploy-server-infrastructure.md)

在此步骤中，将安装和配置支持 VPN 所需的服务器端组件。 服务器端组件包括配置 PKI 以分发用户、VPN 服务器和 NPS 服务器使用的证书。  你还可以配置 RRAS 以支持 IKEv2 连接，并配置 NPS 服务器以执行 VPN 连接授权。

若要配置服务器基础结构，必须执行以下任务：

- **在配置了 Active Directory 域服务的服务器上：** 在组策略为计算机和用户启用证书自动注册，创建 VPN 用户组、VPN 服务器组和 NPS 服务器组，并向每个组添加成员。
- **在 Active Directory 证书服务器 CA 上：** 创建用户身份验证、VPN 服务器身份验证和 NPS 服务器身份验证证书模板。
- **在已加入域的 Windows 10 客户端上：** 注册并验证用户证书。

## <a name="step-3-configure-the-remote-access-server-for-always-on-vpnvpn-deploy-rasmd"></a>[步骤 3.为始终启用 VPN 配置远程访问服务器](vpn-deploy-ras.md)

在此步骤中，你将配置远程访问 VPN 以允许 IKEv2 VPN 连接，拒绝来自其他 VPN 协议的连接，并分配一个静态 IP 地址池，用于颁发 IP 地址，以便连接授权的 VPN 客户端。

若要配置 RAS，必须执行以下任务：

- 注册并验证 VPN 服务器证书
- 安装和配置远程访问 VPN

## <a name="step-4-install-and-configure-the-nps-servervpn-deploy-npsmd"></a>[步骤 4.安装和配置 NPS 服务器](vpn-deploy-nps.md)

在此步骤中，你将使用 Windows PowerShell 或服务器管理器添加角色和功能向导 "安装网络策略服务器（NPS）。 你还可以将 NPS 配置为处理从 VPN 服务器接收的连接请求的所有身份验证、授权和记帐职责。

若要配置 NPS，必须执行以下任务：

- 在 Active Directory 中注册 NPS 服务器
- 配置 NPS 服务器的 RADIUS 记帐
- 在 NPS 中将 VPN 服务器添加为 RADIUS 客户端
- 在 NPS 中配置网络策略
- 自动注册 NPS 服务器证书

## <a name="step-5-configure-dns-and-firewall-settings-for-always-on-vpnvpn-deploy-dns-firewallmd"></a>[步骤 5.为 Always On VPN @ no__t 配置 DNS 和防火墙设置-0

在此步骤中，你将配置 DNS 和防火墙设置。 当远程 VPN 客户端连接时，它们将使用您的内部客户端使用的相同 DNS 服务器，这允许它们以与内部工作站的其余部分相同的方式解析名称。 

## <a name="step-6-configure-windows-10-client-always-on-vpn-connectionsvpn-deploy-client-vpn-connectionsmd"></a>[步骤 6.配置 Windows 10 客户端始终启用 VPN 连接](vpn-deploy-client-vpn-connections.md)

在此步骤中，你将 Windows 10 客户端计算机配置为使用 VPN 连接与该基础结构进行通信。 你可以使用多种技术来配置 Windows 10 VPN 客户端，包括 Windows PowerShell、System Center Configuration Manager 和 Intune。 所有三个都需要一个 XML VPN 配置文件来配置相应的 VPN 设置。

## <a name="step-7-optional-configure-conditional-access-for-vpn-connectivityad-ca-vpn-connectivity-windows10md"></a>[步骤 7.可有可无为 VPN 连接配置条件访问 @ no__t-0

在此可选步骤中，你可以微调授权的 VPN 用户访问资源的方式。 通过 Azure AD VPN 连接的条件性访问，你可以帮助保护 VPN 连接。 条件性访问是基于策略的评估引擎，可让你为任何连接 Azure AD 的应用程序创建访问规则。 有关详细信息，请参阅[Azure Active Directory （Azure AD）条件性访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。

## <a name="next-step"></a>下一步

[步骤 1.规划 Always On VPN 部署 @ no__t-0：在打算用作 VPN 服务器的计算机上安装远程访问服务器角色之前，请使用。 进行适当规划后, 可以部署 Always On VPN, 还可以选择使用 Azure AD 配置 VPN 连接的条件性访问。  

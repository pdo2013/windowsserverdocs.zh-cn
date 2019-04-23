---
title: 部署始终启用 VPN
description: 本主题提供有关部署 Always On VPN Windows Server 2016 中的详细的说明。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ad748de2-d175-47bf-b05f-707dc48692cf
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15c60f2986d3837c5c6e03f9e0a25c7e0a4e0cc0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867138"
---
# <a name="deploy-always-on-vpn"></a>部署始终启用 VPN

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows 10

&#0171;[ **前面：** 了解有关 Always On VPN 的高级功能](always-on-vpn-adv-options.md)<br>
&#0187;[ **下一步：** 步骤 1：开始规划 Always On VPN 部署](always-on-vpn-deploy-planning.md)

在本部分中，您了解有关部署的远程已加入域的 Windows 10 客户端计算机的 Always On VPN 连接的工作流。 如果想要**配置条件性访问**若要优化 VPN 用户访问你的资源的方式，请参阅[针对 VPN 连接使用 Azure AD 条件性访问](../../ad-ca-vpn-connectivity-windows10.md)。 若要了解有关针对 VPN 连接使用 Azure AD 条件性访问的详细信息，请参阅[Azure Active Directory 中的条件性访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。 


部署 Always On VPN 时下, 图说明了不同方案的工作流过程： 

_单击图像进行放大_。

<a href="../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow.png" alt="Full-sized view of the Always On VPN deployment workflow" target="_blank">![缩略图](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)
</a> 

>[!IMPORTANT]
>对于此部署中，它不是您的基础结构服务器，例如运行 Active Directory 域服务、 Active Directory 证书服务和网络策略服务器的计算机正在运行 Windows Server 2016 的要求。 可以使用早期版本的 Windows Server、 Windows Server 2012 R2，例如，为基础结构服务器和运行远程访问服务器。

## <a name="step-1-plan-the-always-on-vpn-deploymentalways-on-vpn-deploy-planningmd"></a>[第 1 步。计划始终打开 VPN 部署](always-on-vpn-deploy-planning.md)

在此步骤中，您开始规划和准备 Always On VPN 部署。 在计算机上安装远程访问服务器角色之前计划在使用作为 VPN 服务器上。 正确的规划之后, 可以部署 Always On VPN，并 （可选） 配置的 VPN 连接使用 Azure AD 条件性访问。

## <a name="step-2-configure-the-always-on-vpn-server-infrastructurevpn-deploy-server-infrastructuremd"></a>[第 2 步。配置 Always On VPN 服务器基础结构](vpn-deploy-server-infrastructure.md)

此步骤中，安装并配置支持的 VPN 所必需的服务器端组件。 服务器端组件包括配置要分发所用的用户、 VPN 服务器和 NPS 服务器证书的 PKI。  您还可以配置 RRAS 以支持建立的 IKEv2 连接和要为 VPN 连接执行授权的 NPS 服务器。

若要配置的服务器基础结构，必须执行以下任务：
- **在服务器上配置了 Active Directory 域服务：** 启用组策略中的计算机和用户的证书自动注册、 创建 VPN 用户组、 VPN 服务器组和 NPS 服务器组，并将成员添加到每个组。
- **在 Active Directory 证书服务器 CA:** 创建用户身份验证，VPN 服务器身份验证和 NPS 服务器身份验证证书模板。
- **在已加入域的 Windows 10 客户端：** 注册并验证用户证书。

## <a name="step-3-configure-the-remote-access-server-for-always-on-vpnvpn-deploy-rasmd"></a>[第 3 步。配置的远程访问服务器的 Always On VPN](vpn-deploy-ras.md)

在此步骤中，可以配置允许 IKEv2 VPN 连接，拒绝来自其他 VPN 协议，连接并将颁发的 IP 地址的静态 IP 地址池分配给连接已授权的 VPN 客户端的远程访问 VPN。

若要配置 RAS，必须执行以下任务：
- 注册并验证 VPN 服务器证书
- 安装和配置远程访问 VPN

## <a name="step-4-install-and-configure-the-nps-servervpn-deploy-npsmd"></a>[第 4 步。安装和配置 NPS 服务器](vpn-deploy-nps.md)

在此步骤中，通过使用 Windows PowerShell 或服务器管理器添加角色和功能向导安装网络策略服务器 (NPS)。 您还可以配置 NPS 以处理所有身份验证、 授权和记帐职责，它接收来自 VPN 服务器的连接请求。

若要配置 NPS，必须执行以下任务：
- 在 Active Directory 中注册 NPS 服务器
- 配置 RADIUS 记帐的 NPS 服务器
- 添加 VPN 服务器在 NPS 中的 RADIUS 客户端
- 在 NPS 中配置网络策略
- 自动注册 NPS 服务器证书

## <a name="step-5-configure-dns-and-firewall-settings-for-always-on-vpnvpn-deploy-dns-firewallmd"></a>[第 5 步。配置 DNS 和防火墙设置为 Always On VPN](vpn-deploy-dns-firewall.md)

在此步骤中，配置 DNS 和防火墙设置。 当远程 VPN 客户端连接时，它们使用相同的 DNS 服务器的内部客户端使用，这使它们在内部工作站的其余部分相同的方式解析名称可以。 

## <a name="step-6-configure-windows-10-client-always-on-vpn-connectionsvpn-deploy-client-vpn-connectionsmd"></a>[步骤 6。配置 Windows 10 客户端 Always On VPN 连接](vpn-deploy-client-vpn-connections.md)

在此步骤中，您配置 Windows 10 客户端计算机与该基础结构与 VPN 连接进行通信。 可以使用多种技术来配置 Windows 10 VPN 客户端，包括 Windows PowerShell、 System Center Configuration Manager 和 Intune。 所有这三个需要 XML VPN 配置文件来配置适当的 VPN 设置。 

## <a name="step-7-optional-configure-conditional-access-for-vpn-connectivityad-ca-vpn-connectivity-windows10md"></a>[步骤 7。（可选）配置针对 VPN 连接的条件性访问](../../ad-ca-vpn-connectivity-windows10.md) 
在此可选步骤中，可以微调如何授权的 VPN 用户访问资源。 使用针对 VPN 连接的 Azure AD 条件性访问，可帮助保护 VPN 连接。 条件性访问是一个基于策略的评估引擎，它允许你创建访问规则的任何 Azure AD 连接的应用程序。 有关详细信息，请参阅[Azure Active Directory (Azure AD) 条件性访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。


## <a name="next-step"></a>下一步
[第 1 步。规划 Always On VPN 部署](always-on-vpn-deploy-planning.md):在计算机上安装远程访问服务器角色之前计划在使用作为 VPN 服务器上。 正确的规划之后, 可以部署 Always On VPN，并 （可选） 配置的 VPN 连接使用 Azure AD 条件性访问。  



---
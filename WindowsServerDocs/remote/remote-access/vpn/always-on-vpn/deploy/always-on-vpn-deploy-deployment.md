---
title: 部署始终启用 VPN
description: 本主题提供有关部署始终启用 VPN Windows Server 2016 中的详细的说明。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ad748de2-d175-47bf-b05f-707dc48692cf
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15c60f2986d3837c5c6e03f9e0a25c7e0a4e0cc0
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067035"
---
# 部署始终启用 VPN

>适用于： Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #0171;[**上一步：** 了解始终启用 VPN 高级功能](always-on-vpn-adv-options.md)<br>
& #0187;[**下一步：** 步骤 1。开始菜单规划始终启用 VPN 部署](always-on-vpn-deploy-planning.md)

在此部分中，你了解部署远程已加入域的 Windows 10 客户端计算机始终启用 VPN 连接的工作流。 如果你希望配置**条件访问**微调 VPN 用户如何访问你的资源，请参阅[使用 Azure AD 的 VPN 连接的条件访问](../../ad-ca-vpn-connectivity-windows10.md)。 若要了解有关条件访问使用 Azure AD 的 VPN 连接的详细信息，请参阅[Azure Active Directory 中的条件访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。 


下图说明了不同的方案的工作流过程，部署始终启用 VPN 时： 

_单击放大图像_。

<a href="../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow.png" alt="Full-sized view of the Always On VPN deployment workflow" target="_blank">![缩略图](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)
</a> 

>[!IMPORTANT]
>对于此部署，它不是你的基础结构服务器，如运行 Active Directory 域服务、 Active Directory 证书服务和网络策略服务器，计算机将运行 Windows Server 2016 的一项要求。 基础结构服务器和运行远程访问服务器，你可以使用较早版本的 Windows Server 2012 R2，如 Windows Server。

## [步骤 1： 规划始终启用 VPN 部署](always-on-vpn-deploy-planning.md)

在此步骤中，你开始规划和准备始终启用 VPN 部署。 在计算机上安装远程访问服务器角色之前你打算使用为 VPN 服务器。 正确的规划之后, 可以部署始终启用 VPN，并 （可选） 配置为使用 Azure AD 的 VPN 连接的条件访问。

## [步骤 2： 配置始终启用 VPN 服务器基础结构](vpn-deploy-server-infrastructure.md)

在此步骤中，你可以安装和配置支持 VPN 所需的服务器端组件。 服务器端组件，包括配置 PKI 分配使用的用户、 VPN 服务器和 NPS 服务器证书。  你还可以配置 RRAS 以支持 IKEv2 连接和 NPS 服务器进行授权 VPN 连接。

若要配置服务器基础结构，你必须执行以下任务：
- **与 Active Directory 域服务配置的服务器上：** 启用组策略中的计算机和用户的证书自动注册，创建 VPN 用户组、 VPN 服务器组和 NPS 服务器组中，向每个组添加成员。
- **在 Active Directory 证书服务器 CA:** 创建用户身份验证，VPN 服务器身份验证和 NPS 服务器身份验证证书模板。
- **已加入域的 Windows 10 客户端上：** 注册并验证用户的证书。

## [步骤 3： 为始终启用 VPN 配置远程访问服务器](vpn-deploy-ras.md)

在此步骤中，你可以配置远程访问 VPN 允许 IKEv2 VPN 连接、 拒绝连接其他 VPN 协议，并将颁发的 IP 地址静态 IP 地址池分配给连接授权的 VPN 客户端。

若要配置 RAS，你必须执行以下任务：
- 注册并验证 VPN 服务器证书
- 安装和配置远程访问 VPN

## [步骤 4： 安装和配置 NPS 服务器](vpn-deploy-nps.md)

在此步骤中，你可以通过使用 Windows PowerShell 或服务器管理器添加角色和功能向导安装网络策略服务器 (NPS)。 你还可以配置 NPS 处理所有身份验证、 授权和记帐职责从 VPN 服务器接收的连接请求。

若要配置 NPS，你必须执行以下任务：
- 在 Active Directory 中注册 NPS 服务器
- 配置 RADIUS NPS 服务器记帐
- 为在 NPS RADIUS 客户端添加 VPN 服务器
- 在 NPS 中配置网络策略
- 自动注册 NPS 服务器证书

## [步骤 5： 配置 DNS 和防火墙设置始终在 VPN](vpn-deploy-dns-firewall.md)

在此步骤中，你可以配置 DNS 和防火墙设置。 当远程 VPN 客户端连接时，他们使用相同的 DNS 服务器的内部的客户端使用，这使它们可以解析内部工作站的其余部分的相同的方式的名称。 

## [步骤 6： 配置 Windows 10 客户端始终启用 VPN 连接](vpn-deploy-client-vpn-connections.md)

在此步骤中，你可以配置 windows 10 客户端计算机与通过 VPN 连接的基础结构进行通信。 你可以使用几种技术来配置 windows 10 VPN 客户端，包括 Windows PowerShell、 System Center Configuration Manager 和 Intune。 所有三个需要 XML VPN 配置文件配置适当的 VPN 设置。 

## [步骤 7： （可选）配置 VPN 连接的条件访问](../../ad-ca-vpn-connectivity-windows10.md) 
在此可选步骤中，你可以微调如何授权的 VPN 用户访问你的资源。 通过 Azure AD 的条件访问用于 VPN 连接，你可以帮助保护 VPN 连接。 条件访问是基于策略的评估引擎，允许你创建访问规则适用于任何 Azure AD 连接的应用程序。 有关详细信息，请参阅[Azure Active Directory (Azure AD) 的条件访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。


## 下一步
[第 1 步。规划始终启用 VPN 部署](always-on-vpn-deploy-planning.md)： 你打算使用为 VPN 服务器的计算机上安装远程访问服务器角色之前。 正确的规划之后, 可以部署始终启用 VPN，并 （可选） 配置为使用 Azure AD 的 VPN 连接的条件访问。  



---
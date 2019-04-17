---
title: 使用 Azure AD 的 VPN 连接条件访问
description: 在此可选步骤中，你可以微调如何授权的 VPN 用户访问使用 Azure Active Directory (Azure AD) 的条件访问的资源。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 07/13/2018
ms.reviewer: deverette
ms.openlocfilehash: c9104a5d9ae3069e753b8b771270502c4264db96
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067028"
---
# 步骤 7： （可选）使用 Azure AD 的 VPN 连接的条件访问

& #171; [**以前：** 步骤 6。始终对 VPN 连接配置 Windows 10 客户端](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)<br>
& #187;[**下一步：** 7.1 步骤。配置 EAP-TLS 忽略检查证书吊销列表 (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

在此可选步骤中，你可以微调 VPN 用户访问使用[Azure Active Directory (Azure AD) 的条件访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)的资源的方式。 通过 Azure AD 的条件访问虚拟专用网络 (VPN) 连接，你可以帮助保护 VPN 连接。 条件访问是基于策略的评估引擎，允许你为任何 Active Directory (Azure AD) 连接的应用程序创建访问规则。 

## 先决条件

你已熟悉以下主题：
- [在 Azure Active Directory 条件访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN 和条件访问](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

若要配置用于 VPN 连接的 Azure Active Directory 条件访问，你需要配置以下内容：
- [服务器基础结构](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [始终启用 VPN 远程访问服务器](always-on-vpn/deploy/vpn-deploy-ras.md)
- [网络策略服务器](always-on-vpn/deploy/vpn-deploy-nps.md)
- [DNS 和防火墙设置](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [始终启用 VPN 连接的 Windows 10 客户端](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## [步骤 7.1： 配置 EAP-TLS 以忽略证书吊销列表 (CRL) 检查](vpn-config-eap-tls-to-ignore-crl-checking.md)

在此步骤中，你可以添加**IgnoreNoRevocationCheck** ，并将其设置为允许客户端的身份验证，当证书不包括 CRL 分发点。 默认情况下，IgnoreNoRevocationCheck 设置为 0 （禁用）。

EAP-TLS 客户端无法连接，除非 NPS 服务器完成吊销的证书链 （包括的根证书） 检查。 云的 Azure AD 颁发给用户的证书没有 CRL，因为它们是短期证书与一个小时的生存期。 在 NPS 上的 EAP 需要配置忽略 CRL 缺乏。 由于身份验证方法，EAP-TLS 下**EAP\13**仅需要此注册表值。 如果使用其他 EAP 身份验证方法，然后该注册表值应该添加下这些以及。 




## [步骤 7.2： 创建使用 Azure AD 进行 VPN 身份验证的根证书](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

在此步骤中，你可以使用 Azure AD 租户中自动创建 VPN 服务器云应用配置为 VPN 身份验证的根证书。  

若要配置用于 VPN 连接的条件访问权限，你需要：
1. （你可以创建多个证书） 在 Azure 门户中创建 VPN 证书。
2. 下载 VPN 证书。
3. 将证书部署到你的 VPN 服务器。

## [步骤 7.3： 配置条件访问策略](vpn-config-conditional-access-policy.md)

在此步骤中，你将配置 VPN 连接的条件访问策略。 

若要配置的条件访问策略，你需要：
1. 创建分配给 VPN 用户的条件访问策略。
2. 设置为**VPN 服务器**云应用。
3. 将授予 （访问控制） 设置为**需要多重身份验证**。  你可以根据需要使用其他控件。

## [步骤 7.4： 将条件访问根证书部署到本地 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

在此步骤中，你可以将 VPN 身份验证的受信任的根证书部署到你的本地 AD。

若要部署的受信任的根证书，你需要：
1. 作为*受信任的根 CA 用于 VPN 身份验证*添加下载的证书。
2. 导入的 VPN 服务器和 VPN 客户端的根证书。
3. 验证证书是否存在并显示为受信任。


## [步骤 7.5： 向 Windows 10 设备创建基于 OMA-DM 的 VPNv2 配置文件](vpn-create-oma-dm-based-vpnv2-profiles.md)

在此步骤中，你可以创建 OMA DM 基于 VPNv2 配置文件使用 Intune 部署 VPN 设备配置策略。 如果你想要使用 SCCM 或 PowerShell 脚本创建 VPNv2 配置文件，请参阅更多详细信息[VPNv2 CSP 设置](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp)。 


## 下一步
[步骤 7.1。配置 EAP-TLS 忽略检查证书吊销列表 (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)： 在此步骤中，你必须添加**IgnoreNoRevocationCheck**并将其设置为允许客户端的身份验证，当证书不包括 CRL 分发点。 默认情况下，IgnoreNoRevocationCheck 设置为 0 （禁用）。

---

## 相关主题
- [配置 VPNv2 配置文件](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)： VPN 客户端是否现在能够与基于云的条件访问平台集成以提供用于远程客户端的设备合规性选项。 在此步骤中，你配置 VPNv2 配置文件**\ < DeviceCompliance > \ < 启用 > true\ < / 启用 >**。 
 
- [增强与自动 VPN 配置文件的 Windows 10 中的远程访问](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile)： 了解 Microsoft 如何实现用于 VPN 连接的条件访问。 VPN 配置文件包含在设备连接到公司网络，包括受支持的身份验证方法和设备连接到 VPN 服务器所需的所有信息。 在 Windows 10 周年更新，包括条件访问和单一登录，更改它我们可以创建我们 Always-On VPN 连接配置文件。 我们创建的连接配置文件已加入域和 Microsoft Intune 管理设备使用 System Center Configuration Manager 控制台。 

- [访问 Azure Active Directory 中的条件](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)： 安全性非常适用于组织使用云最关心的问题。 管理你的云资源时，云安全的一个重要方面就是标识和访问。 在移动优先、 云优先世界中，用户可以访问你的组织的资源从任意位置使用各种设备和应用。 由于这一点，只需专注于谁可以访问的资源不再足够。 为了主机安全性和工作效率之间的平衡，IT 专业人员还需要考虑因素资源的访问控制决定到访问的方式。

- [VPN 和条件访问](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)： VPN 客户端是否现在能够与基于云的条件访问平台集成以提供用于远程客户端的设备合规性选项。 条件访问是基于策略的评估引擎，允许你为任何 Active Directory (Azure AD) 连接的应用程序创建访问规则。 

---
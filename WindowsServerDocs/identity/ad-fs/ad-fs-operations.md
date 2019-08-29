---
ms.assetid: eefcc989-8763-45ee-8a64-3a97b4397160
title: AD FS 操作
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 594e1605f44dad69ab7eee8b22e6a620ade02ad0
ms.sourcegitcommit: c307886e96622e9595700c94128103b84f5722ce
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70108735"
---
# <a name="ad-fs-operations"></a>AD FS 操作



本文档包含 AD FS 的所有文档操作的列表。 

## <a name="service-configuration"></a>服务配置
- [在 AD FS 和 WAP 2016 中更新 SSL 证书](../ad-fs/operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)
- [AD FS 快速还原工具](../ad-fs/operations/AD-FS-Rapid-Restore-Tool.md)
- [在 AD FS 中配置证书身份验证的备用主机名绑定](../ad-fs/operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
- [添加属性存储](../ad-fs/operations/Add-an-Attribute-Store.md)
- [自定义 AD FS 2019 的 HTTP 安全响应标头](../ad-fs/operations/customize-http-security-headers-ad-fs.md)
- [向非管理员用户委派 AD FS Powershell Commandlet 访问权限](../ad-fs/operations/delegate-ad-fs-pshell-access.md)
- [微调 SQL 和地址延迟](../ad-fs/operations/adfs-sql-latency.md) 


## <a name="authentication-configuration"></a>身份验证配置
### <a name="strong-authentication-mfa--password-less"></a>强身份验证 (MFA) & 不小于密码
- [在 AD FS (2019 或更高版本) 中将外部身份验证提供程序配置为主要](../ad-fs/operations/Additional-Authentication-Methods-AD-FS.md)
- [配置 AD FS (2016 或更高版本) 和 Azure MFA](../ad-fs/operations/Configure-AD-FS-2016-and-Azure-MFA.md)
- [为 AD FS 配置其他身份验证方法](../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

### <a name="lockout-protection"></a>锁定保护
- [配置 AD FS Extranet 软锁定保护](../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md)
- [配置 AD FS Extranet 智能锁定保护](../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [配置 AD FS Extranet 禁止 IP](../ad-fs/operations/Configure-AD-FS-Banned-IP.md)

### <a name="policy-configuration"></a>策略配置
- [配置身份验证策略](../ad-fs/operations/Configure-Authentication-Policies.md)
- [配置备用登录 ID](../ad-fs/operations/Configuring-Alternate-Login-ID.md)
- [配置 Azure AD 提示登录行为以使用 AD FS 策略](../ad-fs/operations/AD-FS-Prompt-Login.md)

### <a name="kerberos--certificate-authentication"></a>Kerberos & 证书身份验证
- [启用 AD DS 声明 & 中的 kerberos 复合身份验证 AD FS](../ad-fs/operations/AD-FS-Compound-Authentication-and-AD-DS-claims.md)
- [为用户证书身份验证配置 AD FS](../ad-fs/operations/Configure-User-Certificate-Authentication.md)
- [在 AD FS 中配置证书身份验证的备用主机名绑定](../ad-fs/operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)


### <a name="device"></a>设备
- [AD FS 中的设备身份验证控件](../ad-fs/operations/device-authentication-controls-in-AD-FS.md) 


## <a name="authorization-configuration"></a>授权配置
- [在 AD FS 中配置访问控制策略](../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)
- [配置基于设备的本地条件访问](../ad-fs/operations/Configure-Device-based-Conditional-Access-on-Premises.md)

## <a name="rpt--cpt-configuration"></a>RPT & CPT 配置
- [配置 AD FS 以对存储在 LDAP 目录中的用户进行身份验证](../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)
- [配置声明规则](../ad-fs/operations/Configure-Claim-Rules.md) 
- [创建声明提供方信任](../ad-fs/operations/Create-a-Claims-Provider-Trust.md) 
- [创建非声明感知信赖方信任](../ad-fs/operations/Create-a-Non-Claims-Aware-Relying-Party-Trust.md)
- [创建信赖方信任](../ad-fs/operations/Create-a-Relying-Party-Trust.md)
- [将 AD FS 配置为使用聚合的联合身份验证提供程序 (例如 InCommon)](../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)

## <a name="sign-in-experience-configuration"></a>登录体验配置
- [配置 AD FS 2016 单一登录设置](../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)
- [配置 AD FS 分页登录](../ad-fs/operations/AD-FS-paginated-sign-in.md)
- [配置 AD FS 用户登录自定义](../ad-fs/operations/AD-FS-user-sign-in-customization.md)
- [配置 AD FS 以发送密码过期声明](../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)
- [为不支持 WIA 的设备配置基于 Intranet 表单的身份验证](../ad-fs/operations/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA.md)

## <a name="other"></a>其他
- [跨公司应用程序从任一设备加入工作区以实现 SSO 和无缝第二重身份验证](../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [使用适用于敏感应用程序的附加多重身份验证管理风险](../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
- [使用条件访问控制管理风险](../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
- [设置 AD FS 实验室环境](../ad-fs/operations/Set-up-an-AD-FS-lab-environment.md)
- [操作实例指南：利用适用于敏感应用程序的附加多重身份验证管理风险](../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
- [操作实例指南：使用条件访问控制管理风险](../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)
- [演练：使用 Windows 设备加入工作区](../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)
- [演练：使用 iOS 设备加入工作区](../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

  



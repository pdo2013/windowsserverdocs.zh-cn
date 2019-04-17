---
ms.assetid: 
title: "在广告 FS 客户端访问控制策略"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5e1e1b907e6fccbf2b9906106d3360bd9a6fd69d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>控制访问权限的 Active Directory 联合身份验证服务组织数据

本文档在本地，提供概述访问控制与广告 FS 跨多个场合混合和云。  

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>广告金融服务和条件访问本地资源 
Active Directory 联合身份验证服务推出，从授权策略已可用来限制或允许用户访问基于请求的属性资源和资源。  广告 FS 已移动版版本，因为这些策略如何实现已发生更改。  通过版本访问控制功能的详细信息，请参阅：
- [在 Windows Server 2016 的广告 FS 中访问控制策略](Access-Control-Policies-in-AD-FS.md)
- [在 Windows Server 2012 R2 的广告 FS 中访问控制](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>广告金融服务和混合组织中的条件访问  

广告 FS 提供在本地组成部分混合方案策略条件访问。 广告 FS 基于授权应使用规则，为非 Azure AD 资源，如在本地直接与广告 FS 联盟应用程序。  提供的云组件[Azure AD 条件访问](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access)。  Azure AD 连接提供连接这两种控制平面。

例如，当条件访问云资源使用 Azure AD 注册设备，Azure AD 连接设备编写重新功能使设备注册信息可以在本地消耗和履行广告 FS 策略。  这样一来，你有一致的方法来为两者都上本地访问控制策略和云资源。  

![条件访问](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>Office 365 的客户端访问策略的发展历程
你们中的许多将使用与广告 FS 客户端访问策略限制对 Office 365 和基于如的位置的客户端和正在使用的客户端应用程序的类型的因素其他 Microsoft Online 服务的访问权限。  
- [在 Windows Server 2012 R2 广告 FS 客户端访问策略](Access-Control-Policies-W2K12.md)
- [在广告 FS 2.0 的客户端访问策略](Access-Control-Policies-in-AD-FS-2.md)

这些策略的一些示例包括：
- 阻止所有外部网络的客户端访问 Office 365
- 阻止所有外部网络的客户端访问 Office 365、Exchange 活动同步访问 Exchange Online 的设备除外

通常这些策略背后的基础需要通过确保仅授权客户端应用程序不缓存的数据，缓解数据泄露的风险，或者可以远程禁用的设备可以获取到资源的访问权限。

虽然广告 fs 上述记录的策略适用记录的特定应用场景中，它们将具有限制，因为它们依赖于客户端数据不始终可用。  例如的客户端应用程序身份仅已可用于 Exchange Online 基于服务并不是资源如 SharePoint Online，可能会通过在浏览器或厚厚的客户端，如 Word 或 Excel 访问相同的数据的位置。  此外广告 FS 不知道的访问，如 SharePoint Online 或 Exchange Online Office 365 中的资源。

若要解决这些限制，并提供更强大的方式使用策略以管理访问 Office 365 或其他 Azure AD 基于资源的企业数据，Microsoft 推出了 Azure AD 条件访问。  Azure AD 针对特定的资源，还是在 Office 365、SaaS 或在 Azure AD 自定义应用程序内的任意或所有资源，可配置条件访问策略。  这些策略转动设备信任、位置以及其他因素上。

若要了解有关 Azure AD 条件访问的详细信息，请参阅[Azure Active Directory 中的条件访问](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access)

关键更改启用这些方案是[现代身份验证](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/)、在办公室客户端、Skype、Outlook，以及浏览器的工作方式相同的新方式的验证用户和设备。

## <a name="next-steps"></a>后续步骤
有关详细信息和本地上控制在云中的访问权限，请参阅：

- [Azure Active Directory 中的条件访问](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access)
- [广告 FS 2016 中访问控制策略](Access-Control-Policies-in-AD-FS.md)

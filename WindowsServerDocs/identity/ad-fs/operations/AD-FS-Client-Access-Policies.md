---
ms.assetid: ''
title: AD FS 中的客户端访问控制策略
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c00a13076b3c3cf28f9efa0a5127f50e34219c84
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358628"
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>使用 Active Directory 联合身份验证服务控制对组织数据的访问权限

本文档概述了跨本地、混合和云方案 AD FS 的访问控制。  

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>AD FS 和条件访问本地资源 
由于引入了 Active Directory 联合身份验证服务，授权策略可用于限制或允许用户基于请求的属性和资源对资源的访问权限。  随着 AD FS 从版本移至版本，这些策略的实现方式已发生了变化。  有关按版本列出的访问控制功能的详细信息，请参阅：
- [Windows Server 2016 中 AD FS 的访问控制策略](Access-Control-Policies-in-AD-FS.md)
- [Windows Server 2012 R2 中 AD FS 的访问控制](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>混合组织中的 AD FS 和条件访问  

AD FS 提供混合方案中条件性访问策略的本地组件。 应将基于 AD FS 的授权规则用于非 Azure AD 资源，如直接与 AD FS 联合的本地应用程序。  云组件由[Azure AD 条件访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)提供。  Azure AD Connect 提供连接两个的控制平面。

例如，当你使用 Azure AD 向云资源的条件性访问注册设备时，Azure AD Connect 设备写回功能会使设备注册信息在本地提供，以供 AD FS 策略使用和强制执行。  这样一来，就可以通过一致的方式访问本地和云资源的控制策略。  

![条件性访问](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>Office 365 的客户端访问策略发展
许多人都在使用客户端访问策略和 AD FS 来基于诸如客户端的位置和使用的客户端应用程序的类型限制对 Office 365 和其他 Microsoft Online services 的访问。  
- [Windows Server 2012 R2 中的客户端访问策略 AD FS](Access-Control-Policies-W2K12.md)
- [AD FS 2.0 中的客户端访问策略](Access-Control-Policies-in-AD-FS-2.md)

这些策略的一些示例包括：
- 阻止所有 extranet 客户端访问 Office 365
- 阻止所有 extranet 客户端访问 Office 365，但访问 Exchange Online Exchange Active Sync 的设备除外

通常，这些策略的基础需要是通过确保只有已获授权的客户端、不缓存数据的应用程序或可远程禁用的设备来访问资源，从而降低数据泄露的风险。

尽管上述 AD FS 所记录的策略在记录的具体方案中起作用，但它们具有一些限制，因为它们依赖于不一致提供的客户端数据。  例如，客户端应用程序的标识仅适用于基于 Exchange Online 的服务，而不适用于 SharePoint Online 等资源，在这些资源中，可以通过浏览器或 "胖客户端" （如 Word 或 Excel）访问相同的数据。  此外 AD FS 还不知道要访问的 Office 365 中的资源，例如 SharePoint Online 或 Exchange Online。

为了解决这些限制并提供更可靠的方法来使用策略来管理对 Office 365 或其他基于 Azure AD 的资源中的业务数据的访问，Microsoft 引入了 Azure AD 的条件性访问。  可以为特定资源或 Azure AD 中的 Office 365、SaaS 或自定义应用程序中的任何或所有资源配置 Azure AD 条件访问策略。  这些策略会按设备信任、位置和其他因素进行透视。

要了解 Azure AD 条件性访问的详细信息，请参阅[Azure Active Directory 中的条件访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)

启用这些方案的关键更改是[新式身份验证](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/)，这是一种对用户和设备进行身份验证的新方法，这些方法在 Office 客户端、Skype、Outlook 和浏览器中采用相同的方式。

## <a name="next-steps"></a>后续步骤
有关跨云和本地控制访问权限的详细信息，请参阅：

- [Azure Active Directory 中的条件性访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)
- [AD FS 2016 中的访问控制策略](Access-Control-Policies-in-AD-FS.md)

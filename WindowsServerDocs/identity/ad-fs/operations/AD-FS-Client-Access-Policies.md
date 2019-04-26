---
ms.assetid: ''
title: 在 AD FS 中的客户端访问控制策略
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cc91cd9a446c8ca30471b65374ca99a7bd49d369
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882368"
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>控制对组织数据与 Active Directory 联合身份验证服务的访问权限

本文档提供使用 AD FS 访问控制的概述在本地、 混合和云方案。  

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>AD FS 和本地资源的条件性访问 
由于 Active Directory 联合身份验证服务的引入，已可用于限制或允许用户访问基于该请求的属性的资源和资源授权策略。  为 AD FS 具有已移动版本之间的差异，如何实现这些策略已更改。  有关版本的访问控制功能的详细信息请参阅：
- [在 Windows Server 2016 中的 AD FS 访问控制策略](Access-Control-Policies-in-AD-FS.md)
- [Windows Server 2012 R2 中的 AD FS 中的访问控制](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>AD FS 和混合型组织中的条件访问  

AD FS 提供混合方案中的条件性访问策略上的本地的组件。 AD FS 基于授权规则应应用于非 Azure AD 资源，例如在本地应用程序直接与 AD FS 联合。  提供云组件[Azure AD 条件访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)。  Azure AD Connect 提供连接两个控制平面。

例如，与条件性访问云资源的 Azure AD 注册设备时，Azure AD Connect 设备写回功能可以让设备注册信息可在本地 AD FS 策略来使用和强制执行。  这样一来，您有一种访问控制策略上的本地和云资源的一致性方法。  

![条件性访问](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>适用于 Office 365 客户端访问策略的演变
很多人都与 AD FS 配合使用客户端访问策略来限制对 Office 365 和其他 Microsoft Online 服务基于因素，例如客户端和客户端应用程序正在使用的类型的位置访问。  
- [Windows Server 2012 R2 AD FS 中的客户端访问策略](Access-Control-Policies-W2K12.md)
- [AD FS 2.0 中的客户端访问策略](Access-Control-Policies-in-AD-FS-2.md)

这些策略的一些示例包括：
- 阻止对 Office 365 的所有 extranet 客户端访问
- 阻止对 Office 365 的 Exchange Active Sync 访问 Exchange Online 的设备除外的所有 extranet 客户端访问

通常这些策略背后的基础需求是通过确保只有经过授权的客户端，不缓存数据的应用程序缓解的数据泄漏风险或可远程禁用的设备可以获取对资源的访问。

尽管上述有案可稽的策略适用于 AD FS 工作中所述的特定应用场景，它们具有限制，因为它们依赖于不是始终可用的客户端数据。  例如，客户端应用程序的标识仅实现了适用于 Exchange Online 基于服务和不是 SharePoint Online，可能会相同的数据访问通过浏览器或胖客户端，如 Word 或 Excel 等资源。  AD FS 也不知道的访问，如 SharePoint Online 或 Exchange Online 的 Office 365 中的资源数。

若要解决这些限制并提供更可靠的方式使用策略来管理对 Office 365 或其他基于 Azure AD 资源中的业务数据的访问，Microsoft 推出了 Azure AD 条件访问。  Azure AD 条件访问策略可以以特定的资源，或在 Office 365、 SaaS 或自定义应用程序的任何或所有资源配置在 Azure AD 中。  这些策略在设备信任、 位置和其他因素的数据透视表。

若要了解有关 Azure AD 条件访问的详细信息，请参阅[Azure Active Directory 中条件性访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)

实现这些方案的关键更改是[新式身份验证](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/)、 跨 Office 客户端、 Skype、 Outlook 和浏览器的工作方式相同的用户和设备进行身份验证的新方法。

## <a name="next-steps"></a>后续步骤
在控制跨云的访问权限以及在本地的详细信息请参阅：

- [在 Azure Active Directory 中条件性访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)
- [在 AD FS 2016 中的访问控制策略](Access-Control-Policies-in-AD-FS.md)

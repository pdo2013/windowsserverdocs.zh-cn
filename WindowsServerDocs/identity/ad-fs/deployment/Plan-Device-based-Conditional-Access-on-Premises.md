---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: "计划本地基于设备的条件访问"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6303bba92ce4f4f99ae8e5095060b06885e427d6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="plan-device-based-conditional-access-on-premises"></a>计划本地基于设备的条件访问

>适用于：Windows Server 2016

本文介绍了基于位置的本地目录已连接到 Azure AD 使用 Azure AD 连接混合方案中设备条件访问策略。     

## <a name="ad-fs-and-hybrid-conditional-access"></a>广告金融服务和混合条件访问  

广告 FS 提供在本地组成部分混合方案中的条件访问策略。  使用云资源的条件访问权 Azure AD 注册设备时，Azure AD 连接设备编写重新功能使设备注册信息可以在本地消耗和履行广告 FS 策略。  这样一来，你有一致的方法来为两者都上本地访问控制策略和云资源。  

![条件访问](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

### <a name="types-of-registered-devices"></a>已注册设备的类型  
有三种类型的已注册设备，所有这些在 Azure AD 表现为设备对象，并且可用于广告 FS 本地同样也在使用条件访问。  

| |添加工作或学校帐户  |Azure AD 加入  |Windows 10 Domian 加入    
| --- | --- |--- | --- |
|描述    |  用户添加他们的工作或学校帐户，到其 BYOD 设备交互。  **注意：**添加工作单位或学校帐户是用于工作区加入 Windows 8/8.1 更换       | 用户到 Azure AD 加入他们的 Windows 10 工作设备。|Azure AD 自动注册 Windows 10 已加入域的设备。|           
|如何用户登录到设备     |  对 Windows 的工作或学校帐户即没有登录。  使用 Microsoft 帐户登录。       |   Windows 即已注册设备 （工作或学校） 帐户登录。      |     使用 AD 帐户登录。|      
|如何管理设备    |      （与其他 Intune 注册） MDM 策略   | （与其他 Intune 注册） MDM 策略        |   组策略，系统中心配置管理器 (SCCM) |
|Azure AD 信任类型|加入工作区|Azure AD 加入|域加入  |     
|W10 设置位置    | 设置 > 帐户 > 你的帐户 > 添加工作或学校帐户        | 设置 > 系统 > 关于 > 加入 Azure AD       |   设置 > 系统 > 关于 > 加入域 |       
|此外适用于 iOS 和 Android 的设备？   |    是的     |       不  |   不   |   

  

注册设备的其他方法的详细信息，请参阅：  
* [在你的工作区中使用 Windows 10 设备](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-windows10-devices/)  
* [设置工作的 Windows 10 设备](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/)  
[加入 Azure Active Directory 的 Windows 10 移动版](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)  

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Windows 10 用户和设备上的登录的不同于以前版本方式  
适用于 Windows 10 和广告 FS 2016 有设备注册和身份验证的一些新方面你应该提前了解（尤其是非常熟悉设备注册和"加入工作区"在以前的版本）。  

首先，在 Windows 10 和 Windows Server 2016 中的广告 FS，设备注册和身份验证不再基于单独提供 X509 证书用户。  没有新且功能更强大的协议，可提供更好的安全性和更加无缝的用户体验。  主要区别是，对于 Windows 10 域加入和 Azure AD 加入，都有提供 X509 证书计算机和新的凭据称为 prt 的参照。  你可以阅读有关它的所有信息[此处](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/)和[此处](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/)。  

其次，Windows 10 和 FS 2016 广告支持使用适用于工作，可以阅读有关 Microsoft Passport 的用户身份验证[此处](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/)和[此处](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)。  

广告 FS 2016 提供无缝设备和用户 SSO 基于 prt 的参照和 Passport 的凭据。  使用本文档中的步骤，你可以启用这些功能，并查看它们工作。  

### <a name="device-access-control-policies"></a>设备访问控制策略  
设备可以如使用简单的广告 FS 访问控制规则中：  

- 允许访问仅从已注册设备   
- 未注册设备时需要多因素身份验证  

然后可以与其他因素的影响，如网络的访问权限位置和多因素身份验证，创建丰富的条件访问策略，如组合以下规则：  


- 公司的网络，除非的特定组或组成员外部访问来自未注册的设备需要多因素身份验证  

与广告 FS 2016，这些可以配置策略专为需要以及级别特定设备信任： 任一**验证**，**管理**，或**兼容**。  

有关详细信息配置广告 FS 访问控制策略，请参阅[广告 FS 中的访问控制策略](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)。  

#### <a name="authenticated-devices"></a>身份验证的设备  
身份验证的设备处于未注册 MDM （Intune 和第三方 MDMs 适用于 Windows 10，Intune 仅适用于 iOS 和 Android） 中的已注册的设备。   

身份验证的设备将具有**isManaged**广告 FS 声称值**FALSE**。 （而不根本注册的设备将缺少此声明。）身份验证的设备 （以及所有已注册的设备） 将具有 isKnown 广告 FS 声称值**如此**。  

#### <a name="managed-devices"></a>管理的设备：   

管理的设备是与的 mdm。 注册的已注册的设备  

托管的设备将具有 isManaged 广告 FS 声称值**如此**。  

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>（与 MDM 或组策略） 兼容的设备  
兼容的设备都不仅注册了 MDM 但符合 MDM 策略的已注册的设备。 （遵循信息发起 MDM 并到 Azure AD 写入。）  

将在具有兼容设备**isCompliant**广告 FS 声称值**如此**。    

有关的完整列表的广告 FS 2016 设备和条件访问索赔，请参阅[参考](#reference)。  


## <a name="reference"></a>参考  
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>完整列表的新广告 FS 2016 和设备索赔  

* https://schemas.microsoft.com/ws/2014/01/identity/claims/anchorclaimtype  
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/implicitupn  
* https://schemas.microsoft.com/2014/03/psso  
* https://schemas.microsoft.com/2015/09/prt  
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid  
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/displayname  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/identifier  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/ostype  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/osversion  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/ismanaged  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser  
* https://schemas.microsoft.com/2014/02/devicecontext/claims/isknown  
* https://schemas.microsoft.com/2014/02/deviceusagetime  
* https://schemas.microsoft.com/2014/09/devicecontext/claims/iscompliant  
* https://schemas.microsoft.com/2014/09/devicecontext/claims/trusttype  
* https://schemas.microsoft.com/claims/authnmethodsreferences  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path  
* https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/client-request-id  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/relyingpartytrustid  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-ip  
* https://schemas.microsoft.com/2014/09/requestcontext/claims/userip  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod  

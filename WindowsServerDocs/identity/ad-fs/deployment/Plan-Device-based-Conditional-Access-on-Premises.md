---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: 规划基于设备的条件性访问本地
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3a78334f64d9e51515757b01f2d788bf87f67a35
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501607"
---
# <a name="plan-device-based-conditional-access-on-premises"></a>规划基于设备的条件性访问本地


本文档介绍基于混合方案中，将在本地目录连接到使用 Azure AD Connect 的 Azure AD 中设备的条件性访问策略。     

## <a name="ad-fs-and-hybrid-conditional-access"></a>AD FS 和混合的条件性访问  

AD FS 提供混合方案中的条件性访问策略上的本地的组件。  与条件性访问云资源的 Azure AD 注册设备时，Azure AD Connect 设备写回功能可以让设备注册信息可在本地 AD FS 策略来使用和强制执行。  这样一来，您有一种访问控制策略上的本地和云资源的一致性方法。  

![条件性访问](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

### <a name="types-of-registered-devices"></a>类型的已注册的设备  
有三种类型的已注册的设备，在 Azure AD 中表示为设备对象和可用于条件性访问与 AD FS 在本地以及所有这些。  

| |添加工作或学校帐户  |加入 Azure AD  |Windows 10 的域加入    
| --- | --- |--- | --- |
|描述    |  用户添加他们的工作或学校帐户添加到其 BYOD 设备以交互方式。  **注意：** 添加工作或学校帐户是工作区加入在 Windows 8/8.1 中的替代       | 用户将 Windows 10 工作设备加入 Azure AD。|已加入域的 Windows 10 设备自动注册到 Azure AD。|           
|用户登录到设备的方式     |  没有登录到 Windows 的工作或学校帐户。  使用 Microsoft 帐户登录。       |   登录到 Windows 以将设备注册的 （工作或学校） 帐户。      |     使用 AD 帐户登录。|      
|如何管理设备    |      MDM 策略 （使用其他 Intune 注册）   | MDM 策略 （使用其他 Intune 注册）        |   组策略，System Center Configuration Manager (SCCM) |
|Azure AD 信任类型|已加入工作区|已加入 azure AD|加入域  |     
|W10 设置位置    | 设置 > 帐户 > 你的帐户 > 添加工作或学校帐户        | 设置 > 系统 > 关于 > 加入 Azure AD       |   设置 > 系统 > 关于 > 加入域 |       
|此外可用于 iOS 和 Android 设备？   |    是     |       否  |   否   |   

  

有关注册设备的不同方法的详细信息，请参阅：  
* [工作区中使用 Windows 10 设备](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-windows10-devices/)  
* [设置 Windows 10 设备进行工作](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/)  
[加入 Azure Active Directory 的 Windows 10 移动版](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)  

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Windows 10 用户和设备登录是不同于以前的版本如何  
适用于 Windows 10 和 AD FS 2016 有设备注册和身份验证的一些新方面有所了解 （尤其是如果您非常熟悉设备注册和"工作区加入"在以前的版本）。  

首先，在 Windows 10 和 Windows Server 2016 中的 AD FS 中，设备注册和身份验证不再只基于 X509 用户证书。  没有新的和更可靠的协议，可提供更佳的安全性和更加无缝用户体验。  主要区别是，对于 Windows 10 域加入和 Azure AD Join，没有 X509 调用 PRT 计算机证书和新的凭据。  可以读取所有相关[这里](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/)并[此处](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/)。  

第二，Windows 10 和 AD FS 2016 支持使用 Microsoft Passport for Work，您可以阅读相关信息的用户身份验证[这里](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/)并[此处](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)。  

AD FS 2016 提供了无缝设备和用户 SSO 基于 PRT 和 Passport 凭据。  使用本文档中的步骤，可以启用这些功能，并查看其工作。  

### <a name="device-access-control-policies"></a>设备的访问控制策略  
设备可以如使用简单的 AD FS 访问控制规则中：  

- 允许仅从已注册的设备进行访问   
- 未注册设备需要多因素身份验证  

然后可以与其他因素，例如网络访问位置和多重身份验证，创建丰富的条件性访问策略，例如组合这些规则：  


- 需要针对从特定组或组的成员除外在企业网络外部访问的未注册设备的多重身份验证  

与 AD FS 2016，这些策略可以配置专门为要求特定设备信任级别也： 任一**进行身份验证**，**托管**，或**符合**。  

有关详细信息配置 AD FS 访问控制策略，请参阅[AD FS 中的访问控制策略](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)。  

#### <a name="authenticated-devices"></a>经过身份验证的设备  
已经过身份验证的设备是未在 MDM （Intune 和第三方 mdm 一起适用于 Windows 10，仅适用于 iOS 的 Intune 和 Android） 中注册的已注册的设备。   

经过身份验证的设备都提供**isManaged** AD FS 声明值**FALSE**。 （而不会在所有已注册的设备将缺少此声明。）经过身份验证的设备 （和所有已注册的设备） 将具有 isKnown AD FS 声明具有值 **，则返回 TRUE**。  

#### <a name="managed-devices"></a>托管的设备：   

被管理的设备是已注册的设备已注册 mdm。  

被管理的设备将具有 isManaged AD FS 声明具有值 **，则返回 TRUE**。  

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>设备符合 （使用 MDM 或组策略）  
符合条件的设备是未仅注册 MDM 但符合 MDM 策略的已注册的设备。 （符合性信息源自 MDM 和写入到 Azure AD）  

符合条件的设备将具有**isCompliant** AD FS 声明值**TRUE**。    

有关 AD FS 2016 设备和条件性访问声明的完整列表，请参阅[引用](#reference)。  


## <a name="reference"></a>参考  
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>新的 AD FS 2016 和设备声明的完整列表  

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

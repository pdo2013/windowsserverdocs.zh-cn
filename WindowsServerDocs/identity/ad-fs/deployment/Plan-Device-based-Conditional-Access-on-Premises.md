---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: 规划基于设备的条件性访问本地
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 79dfc7fbf9e2dcc753829cc53d914f374010f925
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408332"
---
# <a name="plan-device-based-conditional-access-on-premises"></a>规划基于设备的条件性访问本地


本文档介绍基于混合方案中的设备的条件访问策略，其中本地目录使用 Azure AD Connect 连接到 Azure AD。     

## <a name="ad-fs-and-hybrid-conditional-access"></a>AD FS 和混合条件性访问  

AD FS 提供混合方案中条件访问策略的本地组件。  向云资源的条件性访问注册设备时，Azure AD Connect 设备写回功能会使设备注册信息在本地提供，以供 AD FS 策略使用和实施 Azure AD。  这样一来，就可以通过一致的方式访问本地和云资源的控制策略。  

![条件性访问](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

### <a name="types-of-registered-devices"></a>已注册设备的类型  
有三种类型的已注册设备，它们都表示为 Azure AD 中的设备对象，并可用于在本地 AD FS 的条件性访问。  

| |添加工作或学校帐户  |加入 Azure AD  |Windows 10 域加入    
| --- | --- |--- | --- |
|描述    |  用户以交互方式将其工作或学校帐户添加到其 BYOD 设备。  **注意：** 添加工作或学校帐户是 Windows 8/8.1 中 Workplace Join 的替代项       | 用户将其 Windows 10 工作设备加入 Azure AD。|已加入 Windows 10 域的设备会自动注册 Azure AD。|           
|用户如何登录到设备     |  不以工作或学校帐户登录到 Windows。  使用 Microsoft 帐户登录。       |   以注册设备的（工作或学校帐户）登录到 Windows。      |     使用 AD 帐户登录。|      
|如何管理设备    |      MDM 策略（附加 Intune 注册）   | MDM 策略（附加 Intune 注册）        |   组策略，System Center Configuration Manager （SCCM） |
|Azure AD 信任类型|已加入工作区|Azure AD 联接|加入域  |     
|W10 设置位置    | 设置 > 帐户 > 帐户 > 添加工作或学校帐户        | 有关 > 联接 > 系统 > 的设置 Azure AD       |   有关 > 加入域的系统 > 设置 > |       
|还适用于 iOS 和 Android 设备？   |    是     |       否  |   否   |   

  

有关注册设备的不同方法的详细信息，另请参阅：  
* [在工作区中使用 Windows 10 设备](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-windows10-devices/)  
* [设置 Windows 10 设备的工作](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/)  
[将 Windows 10 移动版加入到 Azure Active Directory](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)  

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Windows 10 用户和设备登录与以前的版本有何不同  
对于 Windows 10 和 AD FS 2016，你应该了解设备注册和身份验证的一些新方面（特别是在以前版本中非常熟悉设备注册和 "工作区加入"）。  

首先，在 windows 10 和 Windows Server 2016 中的 AD FS 中，设备注册和身份验证不再仅基于 X509 用户证书。  提供了一个新的、更可靠的协议，提供更好的安全性和更无缝的用户体验。  主要区别在于，对于 Windows 10 域加入和 Azure AD 联接，存在 X509 计算机证书和称为 PRT 的新凭据。  可在[此处](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/)和[此处](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/)阅读所有相关内容。  

其次，Windows 10 和 AD FS 2016 支持使用 Microsoft Passport for Work 的用户身份验证，可在[此处](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/)和[此处](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)阅读。  

AD FS 2016 基于 PRT 和 Passport 凭据提供无缝设备和用户 SSO。  使用本文档中的步骤，您可以启用这些功能并查看它们的工作方式。  

### <a name="device-access-control-policies"></a>设备访问控制策略  
设备可用于简单 AD FS 访问控制规则，例如：  

- 仅允许从已注册设备访问   
- 设备未注册时需要多重身份验证  

然后，可以将这些规则与其他因素（如网络访问位置和多重身份验证）结合使用，从而创建丰富的条件性访问策略，例如：  


- 对于从公司网络外部访问的未注册设备，需要多重身份验证，特定组或组的成员除外  

使用 AD FS 2016，可以专门将这些策略配置为需要特定设备信任级别：**经过身份验证**、**托管**或**合规**。  

有关配置 AD FS 访问控制策略的详细信息，请参阅[AD FS 中的访问控制策略](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)。  

#### <a name="authenticated-devices"></a>经过身份验证的设备  
经过身份验证的设备是未在 MDM （Intune 和第三方 MDMs for Windows 10，Intune 仅适用于 iOS 和 Android）中注册的已注册设备。   

经过身份验证的设备将具有值**为 FALSE**的**isManaged** AD FS 声明。 （而未注册的设备将不会缺少此声明。）经过身份验证的设备（和所有已注册的设备）将具有值**为 TRUE**的 isKnown AD FS 声明。  

#### <a name="managed-devices"></a>托管设备：   

托管设备是注册了 MDM 的注册设备。  

托管设备将具有值**为 TRUE**的 isManaged AD FS 声明。  

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>兼容设备（具有 MDM 或组策略）  
兼容设备是指不仅注册到 MDM，而且符合 MDM 策略的已注册设备。 （符合性信息由 MDM 提供，并写入 Azure AD。）  

相容设备将具有值**为 TRUE**的**isCompliant** AD FS 声明。    

有关 AD FS 2016 设备和条件性访问声明的完整列表，请参阅[参考](#reference)。  


## <a name="reference"></a>参考  
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>新 AD FS 2016 和设备声明的完整列表  

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

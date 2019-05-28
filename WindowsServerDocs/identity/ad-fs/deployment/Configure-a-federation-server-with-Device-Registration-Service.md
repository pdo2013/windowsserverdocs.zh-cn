---
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: 使用设备注册服务配置联合服务器
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f1367f03ea8a9ba96bfe4bae1c324deff92576f0
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192257"
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>使用设备注册服务配置联合服务器

可以启用设备注册服务\(DRS\)后完成中的过程在联合服务器上[步骤 4:配置联合身份验证服务器](https://technet.microsoft.com/library/dn303424.aspx)。 设备注册服务提供了载入机制，用于无缝第二因素身份验证、 永久性的单一登录\-上\(SSO\)，并需要访问公司的使用者的条件性访问资源。 有关 DRS 的详细信息，请参阅[加入工作区以从任一设备实现 SSO 和无缝第二因素身份验证跨公司应用程序](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)  
  
## <a name="prepare-your-active-directory-forest-to-support-devices"></a>准备 Active Directory 林以支持设备  
  
> [!NOTE]  
> 这是一个\-时间必须运行才能准备 Active Directory 林以支持设备的操作。 您必须使用企业管理员权限登录和 Active Directory 林必须具有 Windows Server 2012 R2 架构才能完成此过程。  
>   
> 此外，DRS 需要林根域中有至少一台全局编录服务器。 全局编录服务器所需运行初始化\-ADDeviceRegistration 和 AD FS 身份验证期间。 AD FS 初始化中\-内存表示形式，DRS config 对象上每个身份验证请求和 DRS config 对象上找不到 DC 在当前域中，如果对 GC DRS 对象已在其尝试该请求在初始化期间设置\-ADDeviceRegistration。  
  
#### <a name="to-prepare-the-active-directory-forest"></a>若要准备 Active Directory 林  
  
1.  在联合服务器上打开 Windows PowerShell 命令窗口并键入：  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
2.  当提示输入 ServiceAccountName，输入您选择的服务帐户作为 AD FS 服务帐户的名称。  如果它是一个 gMSA 帐户，请输入中的帐户**域\\accountname$** 格式。 对于域帐户，使用格式**域\\accountname**。  
  
## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>在联合服务器场节点上启用设备注册服务  
  
> [!NOTE]  
> 您必须拥有域管理员权限才能完成此过程登录。  
  
#### <a name="to-enable-device-registration-service"></a>若要启用设备注册服务  
  
1.  在联合服务器上打开 Windows PowerShell 命令窗口并键入：  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  重复此步骤中 AD FS 场中每个联合服务器场节点上...  
  
## <a name="enable-seamless-second-factor-authentication"></a>启用无缝第二个身份验证  
无缝第二个身份验证是中提供了额外的级别的访问保护对公司资源和应用程序尝试对其进行访问的外部设备中的 AD FS 的增强功能。 个人设备加入工作区时，它将成为已知的设备，管理员可以使用此信息为资源创造条件性访问和旨在限制访问。  
  
#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>若要启用无缝第二个身份验证持久性单一登录\-上\(SSO\)和适用于已加入工作区的设备的条件性访问  
  
1.  在 AD FS 管理控制台中，导航到身份验证策略。 选择编辑全局主要身份验证。 选择启用设备身份验证，旁边的复选框，然后单击确定。  
  
## <a name="update-the-web-application-proxy-configuration"></a>更新 Web 应用程序代理配置  
  
> [!IMPORTANT]  
> 不需要将设备注册服务发布到 Web 应用程序代理。  一旦启用联合身份验证服务器上，设备注册服务将可通过 Web 应用程序代理。  您可能需要完成以下步骤来更新 Web 应用程序代理配置，如果在启用设备注册服务之前部署它。  
  
#### <a name="to-update-the-web-application-proxy-configuration"></a>若要更新 Web 应用程序代理配置  
  
1.  在 Web 应用程序代理服务器上，打开 Windows PowerShell 命令窗口并键入  
  
    ```  
    Update-WebApplicationProxyDeviceRegistration  
    ```  
  
2.  提示输入凭据时，输入具有对联合身份验证服务器的管理权限的帐户的凭据。  
  
## <a name="see-also"></a>请参阅 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署联合服务器场](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  


---
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: "与设备注册服务配置联盟服务器"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 511a039afd47cf7570fffdcaf17842e0eccc5683
ms.sourcegitcommit: 9278435cbfa8dbeb30d0557ed0d27832b154edd2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/09/2018
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>与设备注册服务配置联盟服务器

>适用于： Windows Server 2012 R2

完成中的步骤后，您可以您联合身份验证的服务器上启用设备注册服务 \(DRS\)[第 4 步：将联盟服务器配置](https://technet.microsoft.com/library/dn303424.aspx)。 设备注册服务提供无缝第二个载入机制因素身份验证、永久性单个 sign\ 上 \(SSO\) 和条件访问需要访问公司资源的消费者版。 有关 DRS 的详细信息，请参阅[加入工作区从任何设备 SSO 和无缝第二个因素身份验证在公司应用程序](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)  
  
## <a name="prepare-your-active-directory-forest-to-support-devices"></a>准备你支持设备的 Active Directory 森林  
  
> [!NOTE]  
> 这是你必须运行准备 Active Directory 林以支持设备 one\ 时间操作。 你必须使用管理员权限的企业登录和 Active Directory 森林必须完成此过程的 Windows Server 2012 R2 方案。  
>   
> 此外，DRS 要求你森林根域中具有至少一个全球目录服务器。 全球目录服务器需要以运行 Initialize\ ADDeviceRegistration 和期间广告 FS 身份验证。 广告 FS 初始化 in\ 内存对象表示形式 DRS 配置身份验证的每个请求上，并且如果 DRS 配置对象上找不到 DC 当前域中，在其上 DRS 对象已在期间预配 Initialize\ ADDeviceRegistration GC 针对尝试请求。  
  
#### <a name="to-prepare-the-active-directory-forest"></a>准备 Active Directory 森林  
  
1.  在你联盟服务器上，打开 Windows PowerShell 命令窗口中，然后键入:  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
2.  时提示输入 ServiceAccountName，输入您的广告 FS 选为服务帐户 service 帐户的名称。  如果是 gMSA 帐户，输入在帐户**domain\\accountname$**格式。 对于域帐户，使用格式**domain\\accountname**。  
  
## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>启用联合身份验证的服务器电场的日落节点上的设备注册服务  
  
> [!NOTE]  
> 您必须登录域管理员权限才能完成此过程。  
  
#### <a name="to-enable-device-registration-service"></a>若要启用的设备注册服务  
  
1.  在你联盟服务器上，打开 Windows PowerShell 命令窗口中，然后键入:  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  重复此步骤中广告 FS 场每个联盟电场的日落节点。  
  
## <a name="enable-seamless-second-factor-authentication"></a>启用无缝第二个因素身份验证  
无缝第二个因素身份验证是广告 FS 提供添加的级别的访问权限保护公司资源和应用程序尝试访问这些文件的外部设备中的增强。 加入工作区的个人设备时，在确认设备和管理员可以使用此信息驾车到资源的条件访问和金门访问。  
  
#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>若要启用无缝秒因素身份验证、永久性单个 sign\ 上 \(SSO\) 和条件加入工作区设备访问  
  
1.  广告 FS 管理控制台中，导航到身份验证的策略。 选择编辑全球主要身份验证。 选择启用设备身份验证旁边的复选框，然后单击确定。  
  
## <a name="update-the-web-application-proxy-configuration"></a>更新的 Web 应用程序代理配置  
  
> [!IMPORTANT]  
> 不需要将该设备注册服务发布到 Web 应用程序代理。  联合身份验证的服务器上启用后，设备注册服务将 Web 应用程序代理提供。  你可能需要完成此过程中，如果它已启用的设备注册服务之前部署更新 Web 应用程序代理配置。  
  
#### <a name="to-update-the-web-application-proxy-configuration"></a>若要更新的 Web 应用程序代理配置  
  
1.  在你 Web 应用程序代理服务器上，打开 Windows PowerShell 命令窗口中，键入  
  
    ```  
    Update-WebApplicationProxyDeviceRegistration  
    ```  
  
2.  当系统提示您输入凭据，输入帐户具有联盟服务器管理权限的凭据。  
  
## <a name="see-also"></a>请参阅 

[广告 FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012R2 广告 FS 部署指导](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署联盟服务器电场的日落](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  


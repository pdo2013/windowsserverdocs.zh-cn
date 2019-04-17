---
ms.assetid: 35de490f-c506-4b73-840c-b239b72decc2
title: "配置本地基于设备的条件访问"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/11/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 47afd0c6963bd8b8b4dde82650cf807c1954b40b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="configure-on-premises-conditional-access-using-registered-devices"></a>使用已注册的设备配置本地条件访问

>适用于：Windows Server 2016，Windows Server 2012 R2  

下面的文档将指导您完成安装并配置已注册设备与本地条件访问。

![条件访问](media/Using-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

## <a name="infrastructure-pre-requisites"></a>基础结构先决条件
你可以使用本地条件访问在开始之前，都需要以下每个-必备项。 

|要求|描述
|-----|-----
|使用 Azure AD 高级 Azure AD 订阅 | 若要启用的设备的写，重新本地条件访问-[免费试用版很好](https://azure.microsoft.com/en-us/trial/get-started-active-directory/)  
|Intune 订阅|仅所需的设备合规性场景-MDM 集成[免费试用版很好](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0)
|Azure AD 连接|2015 年 11 月 QFE 或更高版本。  获取最新版本[此处](https://www.microsoft.com/en-us/download/details.aspx?id=47594)。  
|Windows Server 2016|版本 10586 或更高版本广告 fs  
|Windows Server 2016 Active Directory 方案|方案级别 85 或更高版本，则需要。
|Windows Server 2016 域控制器|这只是必需的 Hello 的业务密钥信任部署。  可以在找到的其他信息[此处](https://aka.ms/whfbdocs)。  
|Windows 10 客户端|版本 10586 或更高版本、 上述域加入是 Windows 10 域加入和 Microsoft Passport 工作方案只需要  
|使用 Azure AD 高级许可证分配 azure AD 用户帐户|注册设备  


 
## <a name="upgrade-your-active-directory-schema"></a>升级你的 Active Directory 模式
为了使用本地条件访问的已注册设备时，必须首先升级广告方案。  必须满足以下条件：
    - 应 85 或更高版本的方案。
    - 这只是必需的广告 FS 加入森林

> [!NOTE]
> 如果你安装了之前升级到 Windows Server 2016 中的方案版本（级别 85 或更高版本）的 Azure AD 连接，你将需要重新运行 Azure AD 连接安装并刷新本地广告方案，以确保同步规则，为配置 msDS KeyCredentialLink。

### <a name="verify-your-schema-level"></a>验证你的方案级别
若要验证架构级别，请执行以下操作：

1.  你可以使用 ADSIEdit 或 LDP，并且可以连接到方案命名上下文。  
2.  使用 ADSIEdit，请右键单击"CN = 方案，CN = 配置，直流 =<domain>，直流 =<com>并选择属性。  Relpace 域，并与您的森林信息 com 部分。
3.  下特性编辑器中找到 objectVersion 特性，它将告诉你你的版本。  

![ADSI 编辑](media/Configure-Device-Based-Conditional-Access-on-Premises/adsiedit.png)  

你还可以使用下面 PowerShell cmdlet（替换与命名上下文信息模式对象）：

``` powershell
Get-ADObject "cn=schema,cn=configuration,dc=domain,dc=local" -Property objectVersion
    
```

![PowerShell](media/Configure-Device-Based-Conditional-Access-on-Premises/pshell1.png) 

有关升级的其他信息，请参阅[升级到 Windows Server 2016 的域控制器](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2016.md)。 

## <a name="enable-azure-ad-device-registration"></a>启用 Azure AD 设备注册  
若要配置这种情况下，你必须在 Azure AD 配置设备注册功能。  

若要执行此操作，请按照下的步骤[Azure AD 加入为孩子设置在你的组织](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-setup/)  

## <a name="setup-ad-fs"></a>广告 FS 设置  
1. 创建[新广告 FS 2016 场](https://technet.microsoft.com/library/dn486775.aspx)。   
2.  或者[迁移](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)到广告 FS 2016 场从广告 FS 2012 R2  
4. 部署[Azure AD 连接](https://azure.microsoft.com/documentation/articles/active-directory-aadconnectfed-whatis/)使用自定义路径连接到 Azure AD 广告 FS。  

## <a name="configure-device-write-back-and-device-authentication"></a>重新设备写入和设备身份验证配置  
> [!NOTE]
> 如果你运行的 Azure AD 连接使用快速设置时，你已创建正确的广告对象。  不过，大多数广告 FS 情况下，Azure AD 连接所运行的自定义设置配置广告 FS 因此以下步骤是必需的。  

### <a name="create-ad-objects-for-ad-fs-device-authentication"></a>创建用于广告 FS 设备身份验证的广告对象  
如果你的广告 FS 电场的日落未配置为设备身份验证 （你可以看到此服务下广告 FS 管理控制台在-> 设备注册），使用以下步骤创建正确的广告 DS 对象以及配置。  

![注册设备](media/Configure-Device-Based-Conditional-Access-on-Premises/device1.png)

>注意： 命令的下方需要 Active Directory 管理工具，因此如果联盟服务器不可还域控制器，首次安装使用下面的步骤 1 的工具。  否则，你可以跳过第 1 步。  

1.  运行**添加角色和功能**向导和选择功能**远程服务器管理工具** -> **角色管理工具** -> **广告 DS 和广告 LDS 工具**-> 选择这两个**Active Directory Windows PowerShell 模块**和**广告 DS 工具**。

![注册设备](media/Configure-Device-Based-Conditional-Access-on-Premises/device2.png)
  
2.  在你的广告 FS 主服务器上，确保你具有企业管理员 (EA) 权限的广告 DS 用户登录并打开提升了权限的 powershell 提示。  然后，执行以下 PowerShell 命令：  
    
    `Import-module activedirectory`  
    `PS C:\> Initialize-ADDeviceRegistration -ServiceAccountName "<your service account>" ` 
3.  在弹出窗口中，点击是。

>注意： 如果您的广告 FS 服务配置为使用 GMSA 帐户，以格式"domain\accountname$"输入的帐户名称

![注册设备](media/Configure-Device-Based-Conditional-Access-on-Premises/device3.png)  

上述 PSH 创建以下对象：  


- 下的广告域分区 RegisteredDevices 容器  
- 设备注册服务容器和配置下的对象--> 服务--> 设备注册配置  
- 设备注册服务 DKM 容器和配置下的对象--> 服务--> 设备注册配置  

![注册设备](media/Configure-Device-Based-Conditional-Access-on-Premises/device4.png)  

4.  完成后，你将看到成功完成消息。

![注册设备](media/Configure-Device-Based-Conditional-Access-on-Premises/device5.png) 

###        <a name="create-service-connection-point-scp-in-ad"></a>创建广告服务连接点 (SCP)  
如果您计划使用 （带到 Azure AD 自动注册） 的 Windows 10 域加入此处所述，，执行以下命令以在广告 DS 创建服务连接点  
1.  打开 Windows PowerShell，执行以下操作：
    
    `PS C:>Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1" ` 

>注意： 在必要情况下 AdSyncPrep.psm1 从复制文件 Azure AD 连接服务器。  此文件位于计划数值 Azure Active Directory Connect\AdPrep

![注册设备](media/Configure-Device-Based-Conditional-Access-on-Premises/device6.png)   

2. 提供 Azure AD 全球管理员凭据  

    `PS C:>$aadAdminCred = Get-Credential`

![注册设备](media/Configure-Device-Based-Conditional-Access-on-Premises/device7.png) 

3.  运行以下 PowerShell 命令 

    `PS C:>Initialize-ADSyncDomainJoinedComputerSync -AdConnectorAccount [AD connector account name] -AzureADCredentials $aadAdminCred ` 

[广告接头帐户的姓名] 所在的设备在 Azure AD 连接时添加你的本地帐户的名称广告 DS 目录。
  
上述命令启用 Windows 10 客户端查找正确 Azure AD 通过广告 DS 中创建 serviceConnectionpoint 对象加入的域。  

### <a name="prepare-ad-for-device-write-back"></a>准备设备写入重新广告   
以确保广告 DS 对象和容器处于正确的状态的背面设备从 Azure AD 写，执行以下操作。

1.  打开 Windows PowerShell，执行以下操作：  

    `PS C:>Initialize-ADSyncDeviceWriteBack -DomainName <AD DS domain name> -AdConnectorAccount [AD connector account name] ` 

[广告接头帐户的姓名] 所在的设备在 Azure AD 连接时添加你的本地帐户的名称格式 domain\accountname 广告 DS 目录  

上述命令以下对象创建回广告 DS 设备写的如果已，不存在，并允许访问指定广告接头帐户名称  

- 广告域分区中 RegisteredDevices 容器  
- 设备注册服务容器和配置下的对象--> 服务--> 设备注册配置  

### <a name="enable-device-write-back-in-azure-ad-connect"></a>启用设备写入在 Azure AD 重新连接  
如果你尚未前，通过运行向导第二次，然后选择启用重新在 Azure AD 连接的设备写入**"自定义同步选项"**，然后重新设备写入选中该框并选择你完上述 cmdlet 森林  

### <a name="configure-device-authentication-in-ad-fs"></a>广告 FS 中配置设备身份验证  
使用提升的 PowerShell 命令窗口中，通过执行以下命令配置广告 FS 策略  

`PS C:>Set-AdfsGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true -DeviceAuthenticationMethod All` 

### <a name="check-your-configuration"></a>检查你配置  
以供参考，下面是完整列表的广告 DS 设备、 容器和所需的设备回写身份验证工作的权限
 


- 键入 ms-DS-DeviceContainer CN 在的对象 = RegisteredDevices 特区 =&lt;域&gt;        
    - 朗读访问广告 FS 服务帐户   
    - Azure AD 连接同步广告接头帐户读取/写入访问权限</br></br>

- 容器 CN = 设备注册配置，CN = 服务，CN = 配置，直流 =&lt;域&gt;  
- 容器设备注册服务 DKM 下上述容器

![注册设备](media/Configure-Device-Based-Conditional-Access-on-Premises/device8.png) 
 


- 在 CN 的键入 serviceConnectionpoint 的对象 =&lt;guid&gt;，CN = 设备注册

- 配置、 CN = 服务，CN = 配置，直流 =&lt;域&gt;  
 - 访问读取/写入新对象上的指定广告接头帐户名称</br></br> 


- 在 CN 类型 msDS DeviceRegistrationServiceContainer 的对象 = 设备注册服务，CN = 设备注册配置，CN = 服务，CN = 配置，直流 = & ltdomain >  


- 在上述容器类型 msDS DeviceRegistrationService 的对象  

### <a name="see-it-work"></a>查看其工作  
若要评估新索赔和策略，首次注册设备。  例如，你可以在系统下使用设置应用的 Windows 10 计算机-> 关于，Azure AD 加入，或者你可以使用下面的额外步骤的自动设备注册设置 Windows 10 域加入[此处](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)。  有关如何加入 Windows 10 移动设备，请查看文档[此处](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)。  

对于移动的最简单的计算，登录到广告 FS 使用测试应用程序，其中显示列表的索赔。 你将能够看到新的索赔，包括 isManaged isCompliant，，trusttype。  如果您启用了 Microsoft Passport 的工作，你还将看到 prt 声称的参照。  
 

## <a name="configure-additional-scenarios"></a>配置其他方案  
### <a name="automatic-registration-for-windows-10-domain-joined-computers"></a>自动注册的 Windows 10 已加入域的计算机  
若要启用自动设备注册的 Windows 10 域加入计算机，请按照步骤 1 和 2[此处](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)。   
这将帮助你获得以下：  

1. 确保你服务连接广告 DS 点存在，并且具有正确的权限 （我们创建了此对象上述，但它不会没有损害仔细检查）。  
2. 确保已正确配置广告 FS  
3. 确保你的广告 FS 系统已正确端点启用和声称配置规则   
4. 配置所需的自动设备注册的计算机已加入域的组策略设置   

### <a name="microsoft-passport-for-work"></a>Microsoft Passport 的工作   
启用 Windows 10 具有 Microsoft Passport 的工作的信息，请参阅[将 Microsoft Passport 启用用于在你的组织的工作。](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)  

### <a name="automatic-mdm-enrollment"></a>自动 MDM 注册   
若要启用自动 MDM 注册的已注册设备，以便你可以在你访问的控件策略使用 isCompliant 索赔，请按照这些步骤[此处。](https://blogs.technet.microsoft.com/ad/2015/08/14/windows-10-azure-ad-and-microsoft-intune-automatic-mdm-enrollment-powered-by-the-cloud/)  

## <a name="troubleshooting"></a>故障排除  
1.  如果你收到错误`Initialize-ADDeviceRegistration`所报告的有关对象中已存在问题的状态，如"drs 服务对象现已发现不需要的所有特性"，你可以执行的 Azure AD 连接 powershell 之前命令和广告 DS 中有一个完整的配置。  请尝试删除手动下的对象**CN = 设备注册配置，CN = 服务，CN = 配置，直流 =&lt;域&gt;** ，然后重试。  
2.  对于 Windows 10 的域加入客户端  
    1. 验证该设备身份验证正在工作，请登录到域加入测试的用户帐户的客户端。 要触发快速预配，锁定和解锁桌面至少一次。   
    2. 检查个主要凭据说明链接广告 DS 对象上 （没有同步还有两次运行？）  
3.  如果你在尝试注册的 Windows 的计算机已注册设备，但你无法或已有 unenrolled 设备时出错，你可能已设备注册配置一段注册表中。  若要删除此调查，使用以下步骤：  
    1. Windows 在计算机上，打开注册表编辑器并导航到**HKLM\Software\Microsoft\Enrollments**   
    2. 下此项中，有许多子项 GUID 形式。  导航到子项这中有 ~ 17 值，并且"6"[加入了 MDM] 的具有"EnrollmentType"或"13"(加入 Azure AD)  
    3. 修改**EnrollmentType**到**0** 
    4. 试一次注册设备或注册  

### <a name="related-articles"></a>相关的文章  
* [保护 Office 365 与其他应用访问连接到 Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access/)  
* [Office 365 服务条件访问设备策略](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access-device-policies/)  
* [设置本地条件访问使用 Azure Active Directory 设备注册](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-device-registration-on-premises-setup)  
* [连接到 Azure AD 的 Windows 10 体验加入域的设备](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-devices-group-policy/)  

---
ms.assetid: 35de490f-c506-4b73-840c-b239b72decc2
title: 配置基于设备的条件性访问本地
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/11/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fa96fbeed1445b1add2e5de3aad45ad369a6cafa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847218"
---
# <a name="configure-on-premises-conditional-access-using-registered-devices"></a>使用已注册的设备配置的本地条件性访问

>适用于：Windows Server 2016, Windows Server 2012 R2  

以下文档将指导你完成安装和使用已注册的设备配置的本地条件性访问。

![条件性访问](media/Using-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

## <a name="infrastructure-pre-requisites"></a>基础结构系统必备组件
下面的每个必要是必需的然后才能开始使用的本地条件性访问。 

|要求|描述
|-----|-----
|使用 Azure AD 高级版 Azure AD 订阅 | 若要启用设备写回的内部部署条件性访问-[免费试用版并没有什么问题](https://azure.microsoft.com/trial/get-started-active-directory/)  
|Intune 订阅|仅集成所需的 MDM 设备符合性方案-[免费试用版并没有什么问题](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0)
|Azure AD Connect|2015 年 11 月 QFE 或更高版本。  获取最新版本[此处](https://www.microsoft.com/en-us/download/details.aspx?id=47594)。  
|Windows Server 2016|内部版本 10586 或更高版本适用于 AD FS  
|Windows Server 2016 Active Directory 架构|架构级别 85 台或更高版本是必需的。
|Windows Server 2016 域控制器|这只是 Hello For Business 密钥信任部署所必需的。  可以在找到更多信息[此处](https://aka.ms/whfbdocs)。  
|Windows 10 客户端|生成 10586 或更高版本，加入到更高版本的域为方案所必需的 Windows 10 域加入和 Microsoft Passport 的工作只  
|使用 Azure AD Premium 许可证分配的 azure AD 用户帐户|注册设备  


 
## <a name="upgrade-your-active-directory-schema"></a>升级 Active Directory 架构
若要对已注册的设备使用的本地条件性访问，必须首先升级 AD 架构。  必须满足以下条件：
    - 架构应为版本 85 台或更高版本
    - 这只是个林的 AD FS 联接到所需

> [!NOTE]
> 如果安装前升级到 Windows Server 2016 中的架构版本 （级别 85 台或更高版本） 的 Azure AD Connect，则需要重新运行 Azure AD Connect 安装，并刷新本地 AD 架构，以确保的同步规则配置 Msds-keycredentiallink。

### <a name="verify-your-schema-level"></a>验证架构级别
若要验证架构级别，请执行以下操作：

1.  您可以使用 ADSIEdit 或 LDP 并连接到架构命名上下文。  
2.  使用 ADSIEdit，右键单击"CN = Schema、 CN = Configuration，DC =<domain>，DC =<com>并选择属性。  Relpace 域和林信息的 com 部分。
3.  在属性编辑器中找到 objectVersion 属性，然后它将告诉您，您的版本。  

![ADSI 编辑器](media/Configure-Device-Based-Conditional-Access-on-Premises/adsiedit.png)  

此外可以使用以下 PowerShell cmdlet （替换与架构命名上下文信息的对象）：

``` powershell
Get-ADObject "cn=schema,cn=configuration,dc=domain,dc=local" -Property objectVersion
    
```

![PowerShell](media/Configure-Device-Based-Conditional-Access-on-Premises/pshell1.png) 

有关升级的其他信息，请参阅[域控制器升级到 Windows Server 2016](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2016.md)。 

## <a name="enable-azure-ad-device-registration"></a>启用 Azure AD 设备注册  
若要配置这种情况下，必须在 Azure AD 中配置设备注册功能。  

若要执行此操作，请执行下面的步骤[设置在组织中的 Azure AD Join](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-setup/)  

## <a name="setup-ad-fs"></a>安装 AD FS  
1. 创建[新的 AD FS 2016 场](https://technet.microsoft.com/library/dn486775.aspx)。   
2.  或者[迁移](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)从 AD FS 2012 R2 AD FS 2016 场  
4. 部署[Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnectfed-whatis/)使用自定义路径来连接到 Azure AD 的 AD FS。  

## <a name="configure-device-write-back-and-device-authentication"></a>配置设备写回和设备身份验证  
> [!NOTE]
> 如果您运行 Azure AD Connect 使用快速设置，则表明已为您创建了正确的 AD 对象。  但是，在大多数的 AD FS 方案，Azure AD Connect 已使用运行自定义设置来配置 AD FS 中，因此以下步骤所必需。  

### <a name="create-ad-objects-for-ad-fs-device-authentication"></a>为 AD FS 设备身份验证创建 AD 对象  
如果你的 AD FS 场未针对设备身份验证（你可以在“服务”->“设备注册”下的 AD FS 管理控制台中看到此项）进行配置，则使用以下步骤创建正确的 AD DS 对象和配置。  

![设备注册](media/Configure-Device-Based-Conditional-Access-on-Premises/device1.png)

>注意：下方的命令需要 Active Directory 管理工具，因此如果联合服务器不是域控制器，应首先使用下面的步骤 1 安装这些工具。  否则，你可以跳过步骤 1。  

1.  运行**添加角色和功能**向导，选择功能**远程服务器管理工具** -> **角色管理工具** -> **AD DS 和 AD LDS 工具** -> 同时选择 **Windows PowerShell 的 Active Directory 模块**和 **AD DS 工具**。

![设备注册](media/Configure-Device-Based-Conditional-Access-on-Premises/device2.png)
  
2.  AD FS 主服务器，请确保 AD DS 企业管理员 (EA) 权限用户登录并打开提升的 powershell 提示符。  然后，执行以下 PowerShell 命令：  
    
    `Import-module activedirectory`  
    `PS C:\> Initialize-ADDeviceRegistration -ServiceAccountName "<your service account>" ` 
3.  在弹出窗口上单击是。

>注意：如果你的 AD FS 服务被配置为使用 GMSA 帐户，则以格式 "domain\accountname$" 输入帐户名称

![设备注册](media/Configure-Device-Based-Conditional-Access-on-Premises/device3.png)  

上述 PSH 创建以下对象：  


- AD 域分区下的 RegisteredDevices 容器  
- “配置”-->“服务”-->“设备注册配置”下的设备注册服务容器和对象  
- “配置”-->“服务”-->“设备注册配置”下的设备注册服务 DKM 容器和对象  

![设备注册](media/Configure-Device-Based-Conditional-Access-on-Premises/device4.png)  

4.  完成后，你将看到一条指示成功完成的消息。

![设备注册](media/Configure-Device-Based-Conditional-Access-on-Premises/device5.png) 

###        <a name="create-service-connection-point-scp-in-ad"></a>在 AD 中创建服务连接点 (SCP)  
如果你计划使用此处所述的 Windows 10 域加入（自动注册到 Azure AD），则执行以下命令以在 AD DS 中创建服务连接点  
1.  打开 Windows PowerShell，并执行如下命令：
    
    `PS C:>Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1" ` 

>注意： 如有必要，将 Azure AD Connect 服务器中将复制 AdSyncPrep.psm1 文件。  此文件位于 Program Files\Microsoft Azure Active Directory Connect\AdPrep

![设备注册](media/Configure-Device-Based-Conditional-Access-on-Premises/device6.png)   

2. 提供你的 Azure AD 全局管理员凭据  

    `PS C:>$aadAdminCred = Get-Credential`

![设备注册](media/Configure-Device-Based-Conditional-Access-on-Premises/device7.png) 

3.  运行以下 PowerShell 命令 

    `PS C:>Initialize-ADSyncDomainJoinedComputerSync -AdConnectorAccount [AD connector account name] -AzureADCredentials $aadAdminCred ` 

其中，[AD connector account name] 是在添加你的本地 AD DS 目录时你在 Azure AD Connect 中配置的帐户的名称。
  
上述命令让 Windows 10 客户端通过在 AD DS 中创建 serviceConnectionpoint 对象来查找要加入的正确的 Azure AD 域。  

### <a name="prepare-ad-for-device-write-back"></a>为设备回写准备 AD   
为确保 AD DS 对象和容器对于从 Azure AD 的设备回写处于正确的状态，请执行以下操作。

1.  打开 Windows PowerShell，并执行如下命令：  

    `PS C:>Initialize-ADSyncDeviceWriteBack -DomainName <AD DS domain name> -AdConnectorAccount [AD connector account name] ` 

其中，[AD connector account name] 是在以 domain\accountname 格式添加你的本地 AD DS 目录时你在 Azure AD Connect 中配置的帐户的名称  

上述命令为到 AD DS 的设备回写创建以下对象（如果这些对象不存在），并允许访问指定的 AD 连接器帐户名称  

- AD 域分区中的 RegisteredDevices 容器  
- “配置”-->“服务”-->“设备注册配置”下的设备注册服务容器和对象  

### <a name="enable-device-write-back-in-azure-ad-connect"></a>支持在 Azure AD Connect 中进行设备回写  
如果你之前没有这样做，则在 Azure AD Connect 中启用设备回写，方法是二次运行向导，并选择 **“自定义同步选项”**，然后选中设备回写的框并选择你已在其中运行上述 cmdlet 的林  

### <a name="configure-device-authentication-in-ad-fs"></a>在 AD FS 中配置设备身份验证  
使用提升的 PowerShell 命令窗口，通过执行以下命令配置 AD FS 策略  

`PS C:>Set-AdfsGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true -DeviceAuthenticationMethod All` 

### <a name="check-your-configuration"></a>检查配置  
供你参考，下面是 AD DS 设备、容器和执行设备回写和身份验证所需权限的综合列表
 


- CN=RegisteredDevices,DC=&lt;domain&gt; 上类型 ms-DS-DeviceContainer 的对象        
    - AD FS 服务帐户的读取访问权限   
    - Azure AD Connect 同步 AD 连接器帐户的读取/写入访问权限</br></br>

- 容器 CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=&lt;domain&gt;  
- 上方容器下的容器设备注册服务 DKM

![设备注册](media/Configure-Device-Based-Conditional-Access-on-Premises/device8.png) 
 


- CN=&lt;guid&gt;, CN=Device Registration 上类型 serviceConnectionpoint 的对象

- Configuration,CN=Services,CN=Configuration,DC=&lt;domain&gt;  
 - 新对象上指定的 AD 连接器帐户名称的读取/写入访问权限</br></br> 


- 对象类型 msDS-DeviceRegistrationServiceContainer 在 CN = Device Registration Services，CN = Device Registration Configuration，CN = Services，CN = Configuration，DC = & ltdomain >  


- 上方容器中类型 msDS-DeviceRegistrationService 的对象  

### <a name="see-it-work"></a>查看其工作方式  
若要评估的新声明和策略，首次注册设备。  例如，你可以使用系统下的设置应用的 Windows 10 计算机->，Azure AD Join 或可使用的其他步骤自动设备注册设置 Windows 10 域加入[此处](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)。  了解加入 Windows 10 移动设备，请参阅文档[此处](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)。  

对于最简单的计算，登录到 AD FS 使用的测试应用程序显示声明的列表。 你将能够查看包括 isManaged、 isCompliant 和 trusttype 的新声明。  如果您启用 Microsoft Passport for work，还将看到 prt 声明。  
 

## <a name="configure-additional-scenarios"></a>其他方案的配置  
### <a name="automatic-registration-for-windows-10-domain-joined-computers"></a>自动注册的 Windows 10 已加入域的计算机  
若要启用 Windows 10 域的设备自动注册已加入计算机，请执行步骤 1 和 2[此处](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)。   
这将帮助您实现以下目标：  

1. 请确保服务连接点在 AD DS 中的存在并且具有正确的权限 （我们创建了更高版本，此对象，但它不会不会损害仔细检查）。  
2. 请确保正确配置 AD FS  
3. 请确保 AD FS 系统，具有正确的终结点启用和声明规则配置   
4. 配置自动设备注册加入域的计算机所需的组策略设置   

### <a name="microsoft-passport-for-work"></a>Microsoft Passport for Work   
启用与 Microsoft Passport for Work 的 Windows 10 的信息，请参阅[启用 Microsoft Passport for Work，在你的组织中。](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)  

### <a name="automatic-mdm-enrollment"></a>自动进行 MDM 注册   
若要启用自动 MDM 注册的已注册的设备，以便可以在访问控制策略中使用 isCompliant 声明，请按照步骤[此处。](https://blogs.technet.microsoft.com/ad/2015/08/14/windows-10-azure-ad-and-microsoft-intune-automatic-mdm-enrollment-powered-by-the-cloud/)  

## <a name="troubleshooting"></a>疑难解答  
1.  如果收到错误`Initialize-ADDeviceRegistration`的抱怨的对象中已存在错误状态，如"drs 服务对象已被发现而无需所需的所有属性"，您可能执行 Azure AD Connect powershell 命令之前和在 AD DS 中具有的部分配置。  请尝试手动删除下的对象**CN = Device Registration Configuration，CN = Services，CN = Configuration，DC =&lt;域&gt;** ，然后重试。  
2.  对于 Windows 10 加入域的客户端  
    1. 若要验证该设备身份验证正常工作，以登录到域加入的客户端的测试用户帐户。 若要触发快速预配，锁定和解锁至少一次的桌面。   
    2. 若要检查的 stk 密钥凭据的说明在 AD DS 对象链接 （确实同步仍必须将运行两次？）  
3.  如果您在尝试注册的 Windows 计算机已注册设备，但不能或具有已取消注册设备时遇到错误，您可能设备注册配置的片段在注册表中。  若要调查并删除此项，使用以下步骤：  
    1. 在 Windows 计算机上，打开注册表编辑器并导航到**HKLM\Software\Microsoft\Enrollments**   
    2. 在此密钥，以 GUID 形式将是有许多子项。  导航到的子项中它具有大约 17 的值并为"6"[MDM 已加入] 中具有"EnrollmentType"或"13"(已加入 Azure AD)  
    3. 修改**EnrollmentType**到**0** 
    4. 试一次设备注册  

### <a name="related-articles"></a>相关文章  
* [保护对 Office 365 和其他应用的访问连接到 Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)  
* [Office 365 服务的条件性访问设备策略](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access-device-policies/)  
* [设置本地条件性访问使用 Azure Active Directory 设备注册](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)  
* [将已加入域的设备连接到 Azure AD 进行 Windows 10 体验](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)  

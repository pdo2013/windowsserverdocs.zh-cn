---
ms.assetid: 35de490f-c506-4b73-840c-b239b72decc2
title: 配置基于设备的条件性访问本地
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/11/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a7646144b591fd7327f881cb54489201140e9287
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358147"
---
# <a name="configure-on-premises-conditional-access-using-registered-devices"></a>使用已注册的设备配置本地条件性访问


以下文档将指导你完成安装和配置已注册设备的本地条件性访问。

![条件性访问](media/Using-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

## <a name="infrastructure-pre-requisites"></a>基础结构先决条件
在开始使用本地条件访问之前，需要以下每个必备项。 

|要求|描述
|-----|-----
|使用 Azure AD Premium 的 Azure AD 订阅 | 启用设备回写以实现本地条件访问-[免费试用版](https://azure.microsoft.com/trial/get-started-active-directory/)  
|Intune 订阅|仅适用于设备符合性方案的 MDM 集成的要求-[免费试用版](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0)
|Azure AD Connect|2015年11月版或更高版本。  [在此处](https://www.microsoft.com/en-us/download/details.aspx?id=47594)获取最新版本。  
|Windows Server 2016|AD FS 的版本10586或更高版本  
|Windows Server 2016 Active Directory 架构|需要架构级别85或更高版本。
|Windows Server 2016 域控制器|仅适用于企业密钥信任部署，这是必需的。  可在[此处](https://aka.ms/whfbdocs)找到其他信息。  
|Windows 10 客户端|仅 Windows 10 域加入和 Microsoft Passport for Work 方案需要将生成10586或更高版本加入以上域  
|Azure AD 分配了 Azure AD Premium 许可证的用户帐户|注册设备  


 
## <a name="upgrade-your-active-directory-schema"></a>升级 Active Directory 架构
若要对已注册的设备使用本地条件访问，必须先升级 AD 架构。  必须满足以下条件：
    - 架构应为版本85或更高版本
    - 仅 AD FS 加入到的林需要此字段

> [!NOTE]
> 如果在升级到 Windows Server 2016 中的架构版本（85版或更高版本）之前安装 Azure AD Connect，则需要重新运行 Azure AD Connect 安装并刷新本地 AD 架构，以确保配置 KeyCredentialLink。

### <a name="verify-your-schema-level"></a>验证你的架构级别
若要验证架构级别，请执行以下操作：

1.  可以使用 ADSIEdit 或 LDP 并连接到架构命名上下文。  
2.  使用 ADSIEdit，右键单击 "CN = Schema，CN = Configuration，DC = <domain>，DC = <com>"，然后选择 "属性"。  Relpace 域和 com 部分，其中包含林信息。
3.  在 "属性编辑器" 下，找到 "objectVersion" 属性，它将告知你版本。  

![ADSI 编辑器](media/Configure-Device-Based-Conditional-Access-on-Premises/adsiedit.png)  

你还可以使用以下 PowerShell cmdlet （将对象替换为架构命名上下文信息）：

``` powershell
Get-ADObject "cn=schema,cn=configuration,dc=domain,dc=local" -Property objectVersion
    
```

![PowerShell](media/Configure-Device-Based-Conditional-Access-on-Premises/pshell1.png) 

有关升级的其他信息，请参阅[将域控制器升级到 Windows Server 2016](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2016.md)。 

## <a name="enable-azure-ad-device-registration"></a>启用 Azure AD 设备注册  
若要配置此方案，你必须在 Azure AD 中配置设备注册功能。  

为此，请按照[在你的组织中设置 Azure AD Join 中](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-setup/)的步骤操作  

## <a name="setup-ad-fs"></a>设置 AD FS  
1. 创建新的[AD FS 2016 场](https://technet.microsoft.com/library/dn486775.aspx)。   
2.  或从 AD FS 2012 R2 将场[迁移](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)到 AD FS 2016  
4. 使用自定义路径部署[Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnectfed-whatis/) ，将 AD FS 连接到 Azure AD。  

## <a name="configure-device-write-back-and-device-authentication"></a>配置设备写回和设备身份验证  
> [!NOTE]
> 如果使用快速设置运行 Azure AD Connect，则已为您创建了正确的 AD 对象。  但在大多数 AD FS 情况下，Azure AD Connect 是用自定义设置运行的，以配置 AD FS，因此需要执行以下步骤。  

### <a name="create-ad-objects-for-ad-fs-device-authentication"></a>为 AD FS 设备身份验证创建 AD 对象  
如果你的 AD FS 场未针对设备身份验证（你可以在“服务”->“设备注册”下的 AD FS 管理控制台中看到此项）进行配置，则使用以下步骤创建正确的 AD DS 对象和配置。  

![设备注册](media/Configure-Device-Based-Conditional-Access-on-Premises/device1.png)

>注意:下方的命令需要 Active Directory 管理工具，因此如果联合服务器不是域控制器，应首先使用下面的步骤 1 安装这些工具。  否则，你可以跳过步骤 1。  

1.  运行**添加角色和功能**向导，选择功能**远程服务器管理工具** -> **角色管理工具** -> **AD DS 和 AD LDS 工具** -> 同时选择 **Windows PowerShell 的 Active Directory 模块**和 **AD DS 工具**。

![设备注册](media/Configure-Device-Based-Conditional-Access-on-Premises/device2.png)
  
2. 在 AD FS 主服务器上，确保以具有企业管理员（EA）权限的 AD DS 用户身份登录，并打开权限提升的 powershell 提示符。  然后，执行以下 PowerShell 命令：  
    
   `Import-module activedirectory`  
   `PS C:\> Initialize-ADDeviceRegistration -ServiceAccountName "<your service account>" ` 
3. 在弹出窗口中，单击 "是"。

>注意:如果你的 AD FS 服务被配置为使用 GMSA 帐户，则以格式 "domain\accountname$" 输入帐户名称

![设备注册](media/Configure-Device-Based-Conditional-Access-on-Premises/device3.png)  

上述 PSH 创建以下对象：  


- AD 域分区下的 RegisteredDevices 容器  
- “配置”-->“服务”-->“设备注册配置”下的设备注册服务容器和对象  
- “配置”-->“服务”-->“设备注册配置”下的设备注册服务 DKM 容器和对象  

![设备注册](media/Configure-Device-Based-Conditional-Access-on-Premises/device4.png)  

4. 完成后，你将看到一条指示成功完成的消息。

![设备注册](media/Configure-Device-Based-Conditional-Access-on-Premises/device5.png) 

###        <a name="create-service-connection-point-scp-in-ad"></a>在 AD 中创建服务连接点（SCP）  
如果你计划使用此处所述的 Windows 10 域加入（自动注册到 Azure AD），则执行以下命令以在 AD DS 中创建服务连接点  
1.  打开 Windows PowerShell，并执行如下命令：
    
    `PS C:>Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1" ` 

>注意：如有必要，请从 Azure AD Connect 服务器复制 AdSyncPrep 文件。  此文件位于 Program Files\Microsoft Azure Active Directory Connect\AdPrep

![设备注册](media/Configure-Device-Based-Conditional-Access-on-Premises/device6.png)   

2. 提供你的 Azure AD 全局管理员凭据  

    `PS C:>$aadAdminCred = Get-Credential`

![设备注册](media/Configure-Device-Based-Conditional-Access-on-Premises/device7.png) 

3. 运行以下 PowerShell 命令 

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
如果你之前没有这样做，则在 Azure AD Connect 中启用设备回写，方法是二次运行向导，并选择 **“自定义同步选项”** ，然后选中设备回写的框并选择你已在其中运行上述 cmdlet 的林  

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


- CN = Device Registration Services，CN = Device Registration Configuration，CN = Services，CN = Configuration，DC = & ltdomain > 的类型为 DeviceRegistrationServiceContainer 的对象  


- 上方容器中类型 msDS-DeviceRegistrationService 的对象  

### <a name="see-it-work"></a>查看工作  
若要评估新的声明和策略，请首先注册设备。  例如，你可以使用 "系统->" 下的 "设置" 应用 Azure AD 加入 Windows 10 计算机，也可以在[此处](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)执行附加步骤后使用自动设备注册来设置 windows 10 域加入。  有关加入 Windows 10 移动版设备的信息，请参阅[此处](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)的文档。  

对于最简单的评估，使用显示声明列表的测试应用程序登录到 AD FS。 你将可以看到新的声明，包括 isManaged、isCompliant 和 trusttype。  如果启用 Microsoft Passport for work，还会看到 prt 声明。  
 

## <a name="configure-additional-scenarios"></a>配置其他方案  
### <a name="automatic-registration-for-windows-10-domain-joined-computers"></a>自动注册已加入域的 Windows 10 计算机  
若要为已加入 Windows 10 域的计算机启用自动设备注册，请执行[此处](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)的步骤1和2。   
这将帮助你实现以下目的：  

1. 确保 AD DS 中的服务连接点存在并且具有正确的权限（我们在上面创建了此对象，但这并不会损害双重检查）。  
2. 确保正确配置 AD FS  
3. 确保 AD FS 系统启用了正确的终结点并且配置了声明规则   
4. 为加入域的计算机配置自动设备注册所需的组策略设置   

### <a name="microsoft-passport-for-work"></a>Microsoft Passport for Work   
有关使用 Microsoft Passport for Work 启用 Windows 10 的信息，请参阅[在组织中启用 Microsoft Passport for Work。](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)  

### <a name="automatic-mdm-enrollment"></a>自动 MDM 注册   
若要启用已注册设备的自动 MDM 注册，以便可以在访问控制策略中使用 isCompliant 声明，请按照此处的步骤操作[。](https://blogs.technet.microsoft.com/ad/2015/08/14/windows-10-azure-ad-and-microsoft-intune-automatic-mdm-enrollment-powered-by-the-cloud/)  

## <a name="troubleshooting"></a>疑难解答  
1.  如果在 `Initialize-ADDeviceRegistration` 时出现错误，投诉原因有关某个对象已存在于错误状态，例如 "已找到 drs 服务对象，但未提供所有必需的属性"，则可能已在以前执行过 Azure AD Connect powershell 命令，并且具有AD DS 中的部分配置。  请尝试手动删除 " **CN = Device Registration Configuration，cn = Services，cn = Configuration，DC = &lt;domain @ no__t-2** " 下的对象，然后重试。  
2.  对于已加入域的 Windows 10 客户端  
    1. 若要验证设备身份验证是否正常工作，请以测试用户帐户的身份登录到加入域的客户端。 若要快速触发预配，请至少锁定并解锁桌面。   
    2. 在 AD DS 对象上检查 stk 密钥凭据链接的说明（同步仍需要运行两次？）  
3.  如果你在尝试注册 Windows 计算机时遇到错误，但设备已注册，但你无法或取消注册设备，则可以在注册表中创建设备注册配置片段。  若要调查并删除此项，请使用以下步骤：  
    1. 在 Windows 计算机上，打开 Regedit 并导航到**HKLM\Software\Microsoft\Enrollments**   
    2. 在此项下，GUID 窗体中将有很多子项。  导航到其中包含大约17个值的子项，并将 "EnrollmentType" 的值 "6" [MDM join] 或 "13" （Azure AD 联接）  
    3. 将**EnrollmentType**修改为**0** 
    4. 请重试设备注册或注册  

### <a name="related-articles"></a>相关文章  
* [保护对 Office 365 和其他连接到 Azure Active Directory 的应用的访问](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)  
* [Office 365 服务的条件性访问设备策略](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access-device-policies/)  
* [使用 Azure Active Directory 设备注册设置本地条件性访问](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)  
* [将已加入域的设备连接到 Windows 10 体验 Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)  

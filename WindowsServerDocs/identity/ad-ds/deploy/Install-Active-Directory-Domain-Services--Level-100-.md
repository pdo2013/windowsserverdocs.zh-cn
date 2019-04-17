---
ms.assetid: ae241ed8-ef19-40a9-b2d5-80b8391551ff
title: "安装 Active Directory 域服务 (级别 100)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f76aa1e5200a9fc2f47a559c4a318aa619d31557
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="install-active-directory-domain-services-level-100"></a>安装 Active Directory 域服务 (级别 100)

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍如何通过使用以下方法之一在 Windows Server 2012 安装广告 DS:  
  
-   [运行 Adprep.exe 和安装的 Active Directory 域服务凭据要求](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)  
  
-   [通过使用 Windows PowerShell 安装广告 DS](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PS)  
  
-   [安装广告 DS 使用服务器管理器](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_GUI)  
  
-   [执行使用图形的用户界面转移 RODC 安装](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_UIStaged)  
  
## <a name="BKMK_Creds"></a>运行 Adprep.exe 和安装的 Active Directory 域服务凭据要求  
需要运行 Adprep.exe 并安装广告 DS 下面的凭据。  
  
-   若要安装新林，必须计算机的本地管理员身份登录。  
  
-   若要安装新孩子域或新域树，必须为企业管理员组成员的身份登录。  
  
-   若要安装额外的域控制器，现有的域中，你必须域管理员组成员。  
  
    > [!NOTE]  
    > 如果你安装的第一个现有域或森林中运行 Windows Server 2012 的域控制器单独不运行 adprep.exe 命令，你将会提示您提供凭据运行 Adprep 命令。 凭据要求如下所示：  
    >   
    > -   引入森林中的第一个 Windows Server 2012 域控制器，你需要为组成员的企业管理员，架构管理员组中，提供凭据并域管理员分组在域中该主机架构母版。  
    > -   若要引入了第一个 Windows Server 2012 域控制器在某个域中的，您需要提供的域管理员组成员的凭据。  
    > -   若要引入森林中的第一个只读域控制器 (RODC)，你需要提供用于企业管理员组成员的凭据。  
    >   
    >     > [!NOTE]  
    >     > 如果您已有运行 adprep /rodcprep 不需要在 Windows Server 2008 或 Windows Server 2008 R2、Windows Server 2012 的再次运行。  
  
## <a name="BKMK_PS"></a>通过使用 Windows PowerShell 安装广告 DS  
开始与 Windows Server 2012，可以使用 Windows PowerShell 广告 DS 进行安装。 Dcpromo.exe 不再支持与 Windows Server 2012，在开始，但你仍然可以通过使用 answer 文件运行 dcpromo.exe (dcpromo//unattend:<answerfile>或 dcpromo//answer:<answerfile>)。 若要继续运行带有答案文件 dcpromo.exe 的功能提供组织了投入的现有自动化时间将自动从 dcpromo.exe 转换为 Windows PowerShell 的资源。 有关与答案文件运行 dcpromo.exe 的详细信息，请参阅[https://support.microsoft.com/kb/947034](https://support.microsoft.com/kb/947034)。  
  
删除广告 DS 使用 Windows PowerShell 的详细信息，请参阅[删除广告 DS 使用 Windows PowerShell](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c#BKMK_RemovePS)。  
  
将启动带有添加使用 Windows PowerShell 角色。 此命令安装广告 DS server 角色，然后安装广告 DS 和广告 LDS 服务器管理工具，包括基于 GUI 工具，例如 Active Directory 用户和计算机和如 dcdia.exe 的命令行工具。 默认情况下未安装服务器管理工具，当你使用的 Windows PowerShell。 你需要指定**"IncludeManagementTools**管理本地服务器或安装[远程服务器管理工具](https://www.microsoft.com/download/details.aspx?id=28972)管理远程服务器。  
  
```  
Install-windowsfeature -name AD-Domain-Services -IncludeManagementTools  
<<Windows PowerShell cmdlet and arguments>>  
```  
  
目前不广告 DS 安装完成后，直到要求重新启动。  
  
然后你可以运行此命令以查看可用 cmdlet ADDSDeployment 模块中。  
  
```  
Get-Command -Module ADDSDeployment
```  
  
若要查看列表中参数，可指定的 cmdlet 和语法：  
  
```  
Get-Help <cmdlet name>  
```  
  
例如，若要查看有关创建未占用只读域控制器 (RODC) 帐户参数，键入  
  
```  
Get-Help Add-ADDSReadOnlyDomainControllerAccount
```  
  
在方括号显示可选参数。  
  
你还可以下载最新帮助示例和 Windows PowerShell cmdlet 的概念。 有关详细信息，请参阅[about_Updatable_Help](https://technet.microsoft.com/library/hh847735.aspx)。  
  
你可以远程服务器对运行 Windows PowerShell cmdlet:  
  
-   在 Windows PowerShell、使用 ADDSDeployment cmdlet Invoke-Command。 例如，在服务器上远程安装广告 DS 命名 ConDC3 在 contoso.com 域中，键入：  
  
    ```  
    Invoke-Command { Install-ADDSDomainController -DomainName contoso.com -Credential (Get-Credential) } -ComputerName ConDC3  
    ```  
  
--或  
  
-   在服务器管理器中，创建包含远程服务器服务器组。 右键单击远程服务器的名称，然后单击**Windows PowerShell**。  
  
下一步部分介绍了如何运行 ADDSDeployment 模块 cmdlet 安装广告 DS。  
  
-   [ADDSDeployment cmdlet 参数](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params)  
  
-   [指定 Windows PowerShell 凭据](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSCreds)  
  
-   [使用测试 cmdlet](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_TestCmdlets)  
  
-   [安装新林根域使用 Windows PowerShell](../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSForest)  
  
-   [安装新的孩子或树域，将使用 Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSDomain)  
  
-   [安装使用 Windows PowerShell 其他（副本）域控制器](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSReplica)  
  
### <a name="BKMK_Params"></a>ADDSDeployment cmdlet 参数  
下表列出了用于在 Windows PowerShell ADDSDeployment cmdlet 参数。 参数粗体是必需的。 如果有名为 Windows PowerShell 不同括号中列出了有关 dcpromo.exe 等效参数。  
  
Windows PowerShell 开关接受 $TRUE 或 $FALSE 参数。 指定不需要的顺序 $TRUE 默认情况。  
  
若要替代默认值，你可以指定 $False 值参数。 例如，因为**-installdns**自动运行为新林安装如果它未指定的唯一方法*防止*DNS 安装，当你安装新林是使用：  
  
```  
-InstallDNS:$false  
```  
  
同样，因为**"installdns**具有默认值的 $False 如果你不能主动 Windows Server DNS 服务器的环境中安装了域控制器需要指定以下参数才能安装 DNS 服务器：  
  
```  
-InstallDNS:$true  
```  
  
|参数|描述|  
|------------|---------------|  
|**ADPrepCredential <PS Credential>****注意：**要求你安装的第一个 Windows Server 2012 域控制器在某个域，或者森林和的当前用户凭据是不足，无法执行此操作。|指定具有企业管理员和方案管理员可以准备林的组成员的帐户，规则的[获取凭据](https://technet.microsoft.com/library/dd315327.aspx)和 PSCredential 对象。<br /><br />如果没有值为的值**"凭据**参数用。|  
|AllowDomainControllerReinstall|指定是否要继续安装此可写的域控制器，尽管检测到具有相同名称的另一个可写的域控制器帐户时。<br /><br />使用**$True**仅当你确定不当前使用帐户，通过另一个可写的域控制器。<br /><br />默认**$False**。<br /><br />此参数 rodc 无效。|  
|AllowDomainReinstall|指定现有域将重新创建。<br /><br />默认**$False**。|  
|AllowPasswordReplicationAccountName < 字符串 [>|指定用户帐户、组帐户和计算机的密码可复制到此 RODC 的帐户的名称。 使用空的字符串""如果你想要保留为空的值。 默认情况下，仅允许 RODC 密码复制允许组，，并创建它时最初空白。<br /><br />提供作为字符串深刻的值。 例如：<br /><br />代码-AllowPasswordReplicationAccountName"JSmith"、"JSmithPC"、"分支用户"|  
|ApplicationPartitionsToReplicate < 字符串 [>**注意：**没有等 UI 中的选项。 如果使用的 UI，或使用 IFM 安装，然后将复制所有应用程序分区。|指定复制的应用程序 directory 分区。 仅当你所指定应用此参数**-InstallationMediaPath**参数安装媒体 (IFM) 中。 默认情况下，将复制分区的所有应用程序根据他们自己的范围。<br /><br />提供作为字符串深刻的值。 例如：<br /><br />代码-<br /><br />-ApplicationPartitionsToReplicate"partition1"、"partition2"、"partition3"|  
|确认|提示你运行的 cmdlet 之前的确认。|  
|CreateDnsDelegation**注意：**运行 Add-ADDSReadOnlyDomainController cmdlet 时，不能指定此参数。|指示是否要创建 DNS 委派引用以及域控制器安装新的 DNS 服务器。 有效的 Active Directory"集成 DNS 仅。 仅在处于联机状态，并可以访问的 Microsoft DNS 服务器上，可以创建委派记录。 无法为仅次于顶级域，如.com、.gov、.biz、.edu 或两个字母国家/地区代码域，如.nz 和.au 的域创建委派记录。<br /><br />默认值计算自动根据环境。|  
|**凭据<PS Credential>****注意：**仅必需的当前用户凭据是否不足，无法执行此操作。|指定域帐户，可以登录到域，规则的[获取凭据](https://technet.microsoft.com/library/dd315327.aspx)和 PSCredential 对象。<br /><br />指定不值，如果使用的当前用户的凭据。|  
|CriticalReplicationOnly|指定广告 DS 安装操作是否执行关键仅复制重新启动前的，然后继续进行。 完成安装并重新启动计算机后，将发生关键复制。<br /><br />不建议使用此参数。<br /><br />没有为此选项在用户界面 (UI) 等效项。|  
|DatabasePath <string>|指定完全限定非"通用命名会议 (UNC) 通往包含域数据库中，例如，在本地计算机硬盘上的目录**C:\Windows\NTDS。**<br /><br />默认**%SYSTEMROOT%\NTDS**。 **重要提示：**时可以存储在使用复原文件系统 (ReFS) 格式化的卷的广告 DS 数据库和日志文件，举办上 ReFS 广告 DS 为任何特定好处，以外的复原正常好处你获取举办 ReFS 上的任何数据。|  
|DelegatedAdministratorAccountName <string>|指定的用户或组，可以安装和管理 RODC 的名称。<br /><br />默认情况下，仅域管理员组成员可以管理 RODC。|  
|DenyPasswordReplicationAccountName < 字符串 [>|指定用户帐户、组帐户和计算机帐户的密码将不会复制到此 RODC 的名称。 使用空的字符串""如果你不希望拒绝复制的任何用户或计算机的凭据。 默认情况下，拒绝管理员、服务器运营商、备份运营商、帐户运算符和拒绝 RODC 密码复制组。 默认情况下，拒绝 RODC 密码复制组包括证书发行者、域管理员、管理员企业版、企业域控制器、企业 Read-Only 域控制器、组策略 Creator 所有者、krbtgt 帐户和方案管理员。<br /><br />提供作为字符串深刻的值。 例如：<br /><br />代码-<br /><br />-DenyPasswordReplicationAccountName"RegionalAdmins"、"AdminPCs"|  
|DnsDelegationCredential <PS Credential>**注意：**运行 Add-ADDSReadOnlyDomainController cmdlet 时，不能指定此参数。|指定的用户名和密码创建 DNS 委派，规则的[获取凭据](https://technet.microsoft.com/library/dd315327.aspx)和 PSCredential 对象。|  
|DomainMode <DomainMode> {Win2003 & #124;Win2008 & #124;Win2008R2 & #124;Win2012 & #124;Win2012R2}<br /><br />或<br /><br />DomainMode <DomainMode> {2 #124; 3 & #124; 4 #124; 5 & #124; 6}|新的域的创建过程指定域功能级别。<br /><br />域功能级别能小于林功能级别，但更高版本。<br /><br />自动计算和设置为现有森林功能级别或值设置为默认值**-目录林**。|  
|**域名**<br /><br />对于 Install-ADDSForest 和 Install-ADDSDomainController cmdlet 必需。|指定你希望安装额外的域控制器的域 FQDN。|  
|**DomainNetbiosName <string>**<br /><br />所需 Install-ADDSForest FQDN 前缀名称是否超过 15 个字符。|Install-ADDSForest 搭配使用。 为新森林根域 NetBIOS 名称。|  
|DomainType <DomainType> {ChildDomain & #124;TreeDomain} 或 {孩子 & #124; 树}|指示你想要创建的域的类型：中的现有新域树林，现有的域中，或新林孩子。<br /><br />对于 DomainType 默认为 ChildDomain。|  
|强制|当将取消任何可能通常显示期间安装并添加了域控制器的警告允许 cmdlet 完成其执行指定此参数。 此参数可以包含脚本安装时，很有用。|  
|目录林<ForestMode>{Win2003 & #124;Win2008 & #124;Win2008R2 & #124;Win2012 & #124;Win2012R2}<br /><br />或<br /><br />目录林<ForestMode>{2 #124; 3 & #124; 4 #124; 5 & #124; 6}|当你创建新林指定森林功能级别。<br /><br />默认值为 Win2012。|  
|InstallationMediaPath|指示用于安装新的域控制器安装媒体的位置。|  
|InstallDns|指定是否应安装，域控制器上配置 DNS 服务器服务。<br /><br />对于新的林是默认**$True** DNS 服务器，并且安装。<br /><br />对于新的孩子域或域树中，如果家长域（或森林根域域树）已承载并存储 DNS 域，名称此参数默认为 $True。<br /><br />对于域控制器安装在现有的域中，此参数是否未指定以及当前域已承载和存储 DNS 名称的域中，然后的默认值为此参数**$True**。 否则，DNS 域名称承载 Active Directory 之外，默认是否**$False**没有 DNS 服务器，并且安装。|  
|LogPath <string>|指定包含域日志文件，例如，在本地计算机硬盘上目录完整，非 UNC 路径**C:\Windows\Logs**。<br /><br />默认**%SYSTEMROOT%\NTDS**。 **重要提示：**不存储的数据使用复原文件系统 (ReFS) 格式化的卷上 Active Directory 日志文件。|  
|MoveInfrastructureOperationMasterRoleIfNecessary|指定是否要创建"在当前位于全球目录服务器的用例"并不打算使域控制器了域控制器转移基础结构主机操作主机角色（也称为灵活一个主机操作或 FSMO）创建全球目录服务器。 指定需要传输; 的情况下要创建了域控制器传输主基础角色此参数在此情况下，指定**NoGlobalCatalog**选项，如果你想要保留在当前的基础结构主角色。|  
|**NewDomainName <string>****注意：**仅对 Install-ADDSDomain 必需的。|指定为新的域的域名。<br /><br />例如，如果你想要创建的名为一个新孩子域**emea.corp.fabrikam.com**，你应指定**emea**为此参数值。|  
|**NewDomainNetbiosName <string>**<br /><br />所需 Install-ADDSDomain FQDN 前缀名称是否超过 15 个字符。|Install-ADDSDomain 搭配使用。 NetBIOS 名为新的域。 默认值派生自值**"NewDomainName**。|  
|NoDnsOnNetwork|指定 DNS 服务不是网络上可用。 仅当 IP 设置此计算机的网络适配器未配置为名称分辨率 DNS 服务器的名称此参数。 这指示 DNS 服务器，将安装到此计算机名称分辨率上。 否则，首先必须 DNS 服务器的地址与配置 IP 设置的网络适配器。<br /><br />忽略此参数（默认）指示这服务器台计算机上的网络适配器 TCP/IP 客户端设置将用于联系 DNS 服务器。 因此，如果你并不指定此参数，可确保，首次使用首选 DNS 服务器地址配置 TCP/IP 客户端设置。|  
|NoGlobalCatalog|指定不希望为全球的目录服务器域控制器。<br /><br />默认情况下，将运行 Windows Server 2012 的域控制器安装与全球的目录。 换言之，这会自动运行无计算，除非你指定：<br /><br />代码-<br /><br />-NoGlobalCatalog|  
|NoRebootOnCompletion|指定是否计算机的命令，无论成功完成后重新启动。 默认情况下，将重启计算机。 若要防止服务器重新启动，请指定：<br /><br />代码-<br /><br />-NoRebootOnCompletion: $True<br /><br />没有为此选项在用户界面 (UI) 等效项。|  
|**ParentDomainName <string>****注意：**所需的 Install-ADDSDomain cmdlet|指定现有家长域的 FQDN。 当你安装孩子域或新域树，你可以使用此参数。<br /><br />例如，如果你想要创建的名为一个新孩子域**emea.corp.fabrikam.com**，你应指定**corp.fabrikam.com**为此参数值。|  
|ReadOnlyReplica|指定是否安装只读域控制器 (RODC)。|  
|ReplicationSourceDC <string>|指示难以域信息复制的合作伙伴域控制器 FQDN。 默认将自动计算。|  
|**SafeModeAdministratorPassword <securestring>**|在安全模式或安全模式，如目录服务还原模式的变体启动计算机时，售完管理员帐户的密码。<br /><br />默认情况下空的密码。 你必须提供的密码。 System.Security.SecureString 格式，如提供的读取主机 assecurestring 或 ConvertTo-SecureString，必须提供的密码。<br /><br />SafeModeAdministratorPassword 参数操作特殊：如果无法将其指定为参数，cmdlet 提示你输入并确认屏蔽的密码。 运行 cmdlet interactively.If 指定没有值，并且没有指定 cmdlet 到的其他参数，cmdlet 会提示你输入密码后的屏蔽而无需确认。 运行 cmdlet interactively.If 指定值，则这必须安全字符串。 运行 cmdlet interactively.For 示例，可以手动提示输入密码使用 Read-Host cmdlet 安全字符串用户提示:-safemodeadministratorpassword (阅读主机的提示"密码:"-assecurestring) 你还可以提供安全字符串为转换清除文本变量，尽管这是强烈建议不要。 -safemodeadministratorpassword (convertto securestring"密码 1"asplaintext-强制)|  
|**站点名 <string>**<br /><br />所需的 Add-addsreadonlydomaincontrolleraccount cmdlet|指定装有域控制器的站点。 没有任何**"站点名**参数运行时**安装 ADDSForest**因为创建的第一个网站 Default-First-Site-Name。<br /><br />网站的名称必须已经存在，当提供作为的参数**-站点名**。 将不会 cmdlet 创建该站点。|  
|SkipAutoConfigureDNS|跳过自动配置 DNS 客户端设置、转发器，以及根提示。 如果已安装或使用自动安装 DNS 服务器服务此参数实际上只是**-InstallDNS**。|  
|SystemKey <string>|指定的系统键将数据复制的媒体。<br /><br />默认**无**。<br /><br />数据必须提供的读取主机 assecurestring 或 ConvertTo-SecureString 格式。|  
|SysvolPath <string>|例如，在本地计算机硬盘上指定的目录完整，非 UNC 路径**C:\Windows\SYSVOL**。<br /><br />默认**%SYSTEMROOT%\SYSVOL**。 **重要提示：** SYSVOL 无法存储的数据使用复原文件系统 (ReFS) 格式化的卷上。|  
|SkipPreChecks|不会运行之前启动安装先决条件检查。 不建议使用此设置。|  
|WhatIf|显示 cmdlet 耗尽，会发生什么情况。 Cmdlet 不会运行。|  
  
### <a name="BKMK_PSCreds"></a>指定 Windows PowerShell 凭据  
你可以指定而没有它们显示在屏幕上的纯文本通过使用凭据[获取凭据](https://technet.microsoft.com/library/dd315327.aspx)。  
  
-SafeModeAdministratorPassword 和 LocalAdministratorPassword 参数操作是特殊的方法：  
  
-   如果未指定作为参数，cmdlet 会提示你输入并确认屏蔽的密码。 运行 cmdlet 交互时，这是首选的使用量。  
  
-   如果指定值，则这必须安全字符串。 运行 cmdlet 交互时，这不是首选的使用量。  
  
例如，你可以手动提示输入密码使用**读取主机**cmdlet 安全字符串用户提示  
  
```  
-SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString)
```  
  
> [!WARNING]  
> 上一个选项没有确认密码，如使用格外小心：的密码不可见。  
  
虽然这是强烈建议不要，你还可以为转换清除文本变量，提供安全字符串：  
  
```  
-SafeModeAdministratorPassword (ConvertTo-SecureString "Password1" -AsPlainText -Force)
```  
  
> [!WARNING]  
> 不建议提供或存储清除短的密码。 脚本中运行此命令或查找通过您的任何人都知道 DSRM 域控制器的密码。 使用该知识，他们可以模拟域控制器本身，并将其特权提升至 Active Directory 森林中的最高级别。  
  
### <a name="BKMK_TestCmdlets"></a>使用测试 cmdlet  
每个 ADDSDeployment cmdlet 有相应测试 cmdlet。 测试 cmdlet 运行仅先决条件检查用于安装操作;配置没有安装设置。 对于每个测试 cmdlet 参数都是相同至于相应安装 cmdlet，但**"SkipPreChecks**对于测试 cmdlet 不可用。  
  
|测试 cmdlet|描述|  
|---------------|---------------|  
|Test-ADDSForestInstallation|运行安装新的 Active Directory 森林的先决条件。|  
|Test-ADDSDomainInstallation|运行 Active Directory 在安装新的域的先决条件。|  
|Test-ADDSDomainControllerInstallation|运行 Active Directory 中安装域控制器的先决条件。|  
|Test-ADDSReadOnlyDomainControllerAccountCreation|运行的先决条件添加仅阅读域控制器 (RODC) 的帐户。|  
  
### <a name="BKMK_PSForest"></a>安装新林根域使用 Windows PowerShell  
命令语法安装新林如下所示。 可选参数显示在方括号。  
  
```  
Install-ADDSForest [-SkipPreChecks] -DomainName <string> -SafeModeAdministratorPassword <SecureString> [-CreateDNSDelegation] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-DomainNetBIOSName <string>] [-ForestMode <ForestMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-InstallDNS] [-LogPath <string>] [-NoRebootOnCompletion] [-SkipAutoConfigureDNS] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
> [!NOTE]  
> -DomainNetBIOSName 参数，才能，如果你想要更改自动生成基于 DNS 域名称前缀 15 字符名称或名称超出了 15 个字符。  
  
例如，若要安装名为 corp.contoso.com 新林和牢固提示你提供 DSRM 密码，键入：  
  
```  
Install-ADDSForest -DomainName "corp.contoso.com"   
```  
  
> [!NOTE]  
> 当你在运行 Install-ADDSForest，默认情况下安装 DNS 服务器。  
  
安装名为 corp.contoso.com 新林、contoso.com 域中创建 DNS 委派、域正常工作级别设置为 Windows Server 2008 R2 和森林功能级别设置为 Windows Server 2008、D:\ 驱动器上安装的 Active Directory 数据库和 SYSVOL、安装在 E:\ 驱动器上，该日志文件和会提示你提供目录服务还原模式密码，然后键入:  
  
```  
Install-ADDSForest -DomainName corp.contoso.com -CreateDNSDelegation -DomainMode Win2008 -ForestMode Win2008R2 -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  
  
### <a name="BKMK_PSDomain"></a>安装新的孩子或树域，将使用 Windows PowerShell  
安装新的域的命令语法如下所示。 可选参数显示在方括号。  
  
```  
Install-ADDSDomain [-SkipPreChecks] -NewDomainName <string> -ParentDomainName <string> -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainReinstall] [-CreateDNSDelegation] [-Credential <PS Credential>] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [DomainType <DomainType> {Child Domain | TreeDomain} [-InstallDNS] [-LogPath <string>] [-NoGlobalCatalog] [-NewDomainNetBIOSName <string>] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-Systemkey <SecureString>] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
> [!NOTE]  
> **-凭据**参数才需要不当前登录时作为企业管理员组中的成员。  
>   
> **-NewDomainNetBIOSName**参数是必需的如果你想要更改基于 DNS 域名称前缀自动生成的 15 个字符的名称，或者如果名称超出了 15 个字符。  
  
例如，若要使用凭据 corp\EnterpriseAdmin1 创建一个新的孩子域名为 child.corp.contoso.com 安装 DNS 服务器、corp.contoso.com 域中创建 DNS 委派、域正常工作级别设置为 Windows Server 2003，使域控制器全球目录服务器休斯顿名为某个站点，DC1.corp.contoso.com 用作复制源域控制器，D:\ 驱动器上安装的 Active Directory 数据库和 SYSVOL 安装在 E:\ 驱动器上，该日志文件，并提示你提供目录服务还原模式的密码，但未提示你确认命令，键入：  
  
```  
Install-ADDSDomain -SafeModeAdministratorPassword -Credential (get-credential corp\EnterpriseAdmin1) -NewDomainName child -ParentDomainName corp.contoso.com -InstallDNS -CreateDNSDelegation -DomainMode Win2003 -ReplicationSourceDC DC1.corp.contoso.com -SiteName Houston -DatabasePath "d:\NTDS" "SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs" -Confirm:$False  
```  
  
### <a name="BKMK_PSReplica"></a>安装使用 Windows PowerShell 其他（副本）域控制器  
安装额外的域控制器的命令语法如下所示。 可选参数显示在方括号。  
  
```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainControllerReinstall] [-ApplicationPartitionsToReplicate <string[]>] [-CreateDNSDelegation] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-NoGlobalCatalog] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
若要在 corp.contoso.com 域中安装了域控制器和 DNS 服务器，并会提示您提供了域管理员凭据和 DSRM 密码，键入：  
  
```  
Install-ADDSDomainController -Credential (Get-Credential CORP\Administrator) -DomainName "corp.contoso.com"
```  
  
如果计算机已经已加入域和是域管理员组成员，你可以使用：  
  
```  
Install-ADDSDomainController -DomainName "corp.contoso.com"  
```  
  
若要将提示你的域名，键入：  
  
```  
Install-ADDSDomainController -Credential (Get-Credential) -DomainName (Read-Host "Domain to promote into")
```  
  
以下命令将使用凭据 Contoso\EnterpriseAdmin1 名为波士顿站点中安装可写的域控制器和全球目录服务器、安装 DNS 服务器、contoso.com 域中创建 DNS 委派、安装媒体 c:\ADDS IFM 文件夹中存储的、D:\ 驱动器上安装的 Active Directory 数据库和 SYSVOL、安装 E:\ 驱动器上的日志文件有服务器自动重新启动后广告 DS 安装完成之后，并提示你提供目录服务还原模式的密码：  
  
```  
Install-ADDSDomainController -Credential (Get-Credential CONTOSO\EnterpriseAdmin1) -CreateDNSDelegation -DomainName corp.contoso.com -SiteName Boston -InstallationMediaPath "c:\ADDS IFM" -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  
  
### <a name="performing-a-staged-rodc-installation-using-windows-powershell"></a>执行使用 Windows PowerShell 分步的 RODC 安装  
若要创建 RODC 帐户命令语法如下所示。 可选参数显示在方括号。  
  
```  
Add-ADDSReadOnlyDomainControllerAccount [-SkipPreChecks] -DomainControllerAccuntName <string> -DomainName <string> -SiteName <string> [-AllowPasswordReplicationAccountName <string []>] [-NoGlobalCatalog] [-Credential <PS Credential>] [-DelegatedAdministratorAccountName <string>] [-DenyPasswordReplicationAccountName <string []>] [-InstallDNS] [-ReplicationSourceDC <string>] [-Force] [-WhatIf] [-Confirm] [<Common Parameters>]  
```  
  
若要连接到 RODC 帐户服务器命令语法如下所示。 可选参数显示在方括号。  
  
```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-ApplicationPartitionsToReplicate <string[]>] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-NoDNSOnNetwork] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-UseExistingAccount] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
例如，若要创建 RODC 帐户名为 RODC1:  
  
```  
Add-ADDSReadOnlyDomainControllerAccount -DomainControllerAccountName RODC1 -DomainName corp.contoso.com -SiteName Boston DelegatedAdministratoraccountName PilarA  
```  
  
你想要连接到 RODC1 帐户服务器上，然后运行以下命令。 服务器不能加入域。 首先，安装广告 DS 服务器的角色和管理工具：  
  
```  
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```  
  
运行以下命令以创建 RODC:  
  
```  
Install-ADDSDomainController -DomainName corp.contoso.com -SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString) -Credential (Get-Credential Corp\PilarA) -UseExistingAccount
```  
  
按**Y**确认或包括**"确认**参数，以防止确认提示。  
  
## <a name="BKMK_GUI"></a>安装广告 DS 使用服务器管理器  
通过添加角色向导中服务器管理器后, 跟 Active Directory 域服务配置向导中，即在 Windows Server 2012 的新开始位置，可以在 Windows Server 2012 安装广告 DS。 安装向导中的 Active Directory 域服务 (dcpromo.exe) 不再支持开始在 Windows Server 2012。  
  
以下部分介绍了如何创建安装和管理广告 DS 多个在服务器上，以便服务器池以及如何使用向导安装广告 DS。  
  
### <a name="BKMK_ServerPools"></a>创建服务器池  
前提是他们是可从计算机运行的服务器管理器中访问，服务器管理器可池其他网络上的服务器。 一旦汇集在一起，你可以选择远程安装广告 DS 或任何其他配置选项可能中服务器管理器中的这些服务器。 自动运行服务器管理器的计算机池本身。 关于 server 池的详细信息，请参阅[添加服务器了服务器管理器](https://technet.microsoft.com/library/hh831453.aspx)。  
  
> [!NOTE]  
> 为了管理已加入域的计算机在工作组服务器或，反之亦然使用服务器管理器，需要使用其他配置步骤。 有关详细信息，请参阅"添加工作组中的服务器和管理"在[添加服务器了服务器管理器](https://technet.microsoft.com/library/hh831453.aspx)。  
  
### <a name="BKMK_installADDSGUI"></a>安装广告 DS  
**管理凭据**  
  
若要安装广告 DS 的凭据要求有所不同哪些部署配置根据你选择。 有关详细信息，请参阅[凭据要求运行 Adprep.exe 并安装 Active Directory 域服务](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)。  
  
使用以下步骤安装广告 DS 使用 GUI 方法。 在本地或远程，则可以执行的步骤。 这些步骤更详细的说明，请参阅以下主题：  
  
-   [部署森林使用服务器管理器](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [将副本 Windows Server 2012 域控制器安装现有域 & #40; 在级别 200 & #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  
  
-   [安装新的 Windows Server 2012 的 Active Directory 孩子或树域 & #40;级别 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  
  
-   [安装 Windows Server 2012 的 Active Directory Read-Only 域控制器 & #40;RODC & #41;& #40;级别 200 & #41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md)  
  
##### <a name="to-install-ad-ds-by-using-server-manager"></a>若要安装广告 DS 使用服务器管理器  
  
1.  在服务器管理器中，单击**管理**单击**添加角色和功能**以开始添加角色向导。  
  
2.  在**在开始之前**页上，单击**下一步**。  
  
3.  在**选择安装类型**页上，单击**角色基于或功能的安装**，然后单击**下一步**。  
  
4.  在**选择目标服务器**页上，单击**从服务器池选择服务器**，单击你希望安装广告 DS，然后单击的服务器的名称**下一步**。  
  
    若要选择远程服务器，首先创建服务器池，并向其添加远程服务器。 有关创建服务器池的详细信息，请参阅[添加服务器了服务器管理器](https://technet.microsoft.com/library/hh831453.aspx)。  
  
5.  在**选择服务器角色**页上，单击**Active Directory 域服务**，然后在**添加角色和功能向导**对话框中，单击**添加功能**，，然后单击**下一步**。  
  
6.  在**选择功能**页上，选择你想要安装单击任何其他功能**下一步**。  
  
7.  在**Active Directory 域服务**页上，检查信息，然后单击**下一步**。  
  
8.  在**确认安装选择**页上，单击**安装**。  
  
9. 在**结果**页上，检查是否已成功，安装，然后单击**推广此域控制器服务器**启动 Active Directory 域服务配置向导。  
  
    ![安装广告 DS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_SMPromotes.gif)  
  
    > [!IMPORTANT]  
    > 如果你无需启动 Active Directory 域服务配置向导关闭添加角色向导此时，你可以通过单击任务服务器管理器中重新启动它。  
  
    ![安装广告 DS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)  
  
10. 在**部署配置**页面上，选择以下选项之一：  
  
    -   如果你安装额外的域控制器现有域中，单击**添加到现有的域的域控制器**，并键入域 (例如，emea.corp.contoso.com) 的名称，或者单击**选择…**以选择某个域中，并凭据（如指定是域管理员组成员的帐户），然后单击**下一步**。  
  
        > [!NOTE]  
        > 只有计算机已加入域，并且你正在执行的本地安装默认情况下提供域和当前用户凭据的名称。 如果你安装广告 DS 远程服务器上，你需要通过设计指定的凭据。 如果当前用户凭据没有足够的安装，请单击**更改…**以便指定不同的凭据。  
  
        有关详细信息，请参阅[现有域 & #40; 在安装副本 Windows Server 2012 域控制器级别 200 & #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
    -   如果你安装新的孩子域，请单击**现有的树林中添加新的域**，对于**选择域类型**、选择**孩子域**、键入或浏览到的家长域 DNS 名称 (例如，corp.contoso.com)、键入新孩子域 (例如 emea) 类型凭据，用于创建新的域中，然后单击相对名称**下一步**。  
  
        有关详细信息，请参阅[安装新的 Windows Server 2012 的 Active Directory 孩子或树域 & #40;级别 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
    -   如果你安装新的域树，请单击**添加到现有的森林新的域**，对于**选择域类型**，选择**树域**、键入根域 (例如，corp.contoso.com) 的名称、键入新域 (例如，fabrikam.com) 类型凭据，用于创建新的域中，然后单击 DNS 名称**下一步**。  
  
        有关详细信息，请参阅[安装新的 Windows Server 2012 的 Active Directory 孩子或树域 & #40;级别 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
    -   如果你安装新林，请单击**添加新的林**，然后键入根域 (例如，corp.contoso.com) 的名称。  
  
        有关详细信息，请参阅[安装新的 Windows Server 2012 的 Active Directory 森林 & #40;级别 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
11. 在**域控制器选项**页面上，选择以下选项之一：  
  
    -   如果你要创建一个新林或域，选择了域和森林功能级别，请单击**域名系统 (DNS) 服务器**、指定 DSRM 密码，然后单击**下一步**。  
  
    -   如果你要添加到现有的域域控制器，单击**域名系统 (DNS) 服务器**，**全球目录 (GC)**，或**阅读仅域控制器 (RODC)**根据需要请选择网站的名称，和键入 DSRM 密码，然后单击**下一步**。  
  
    对在此页面上的选项是提供或不提供在其他情况下的详细信息，请参阅[域控制器选项](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage)。  
  
12. 在**DNS 选项**页面（它才会显示你安装 DNS 服务器）上，单击**更新 DNS 委派**根据需要。 如果你执行此操作，提供了创建家长 DNS 区域中的 DNS 委派记录的权限的凭据。  
  
    如果无法联系托管家长区域 DNS 服务器，**更新 DNS 委派**选项将不可用。  
  
    你需要更新 DNS 委派的详细信息，请参阅[了解区域委派](https://technet.microsoft.com/library/cc771640.aspx)。 如果你尝试更新 DNS 委派和遇到了错误，请参阅[DNS 选项](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)。  
  
13. 在**RODC 选项**页面（它才会显示你安装 RODC）、指定的名称将管理 RODC、添加或删除帐户从允许或拒绝密码复制组，，然后单击的帐户的用户或组**下一步**。  
  
    有关详细信息，请参阅[密码复制策略](https://technet.microsoft.com/library/cc730883(v=ws.10))。  
  
14. 在**其他选项**页面上，选择以下选项之一：  
  
    -   如果要创建新的域中，键入新名称 NetBIOS 或验证域的默认 NetBIOS 名称，然后单击**下一步**。  
  
    -   如果你要添加到现有的域域控制器，选择你想要复制的广告 DS 安装数据（或允许可选择任意域控制器向导）的域控制器。 如果你安装媒体，请单击**从媒体路径安装**键入并确认安装源代码文件，路径，然后单击**下一步**。  
  
        你无法使用安装媒体 (IFM) 在某个域中安装的第一个域控制器的。 IFM 将不起作用跨不同的操作系统版本。 换言之，若要安装额外的域控制器，通过使用 IFM 运行 Windows Server 2012，你必须创建 Windows Server 2012 域控制器上备份的媒体。 有关 IFM 的详细信息，请参阅[安装额外的域控制器，方法是使用 IFM](https://technet.microsoft.com/library/cc816722(WS.10).aspx)。  
  
15. 在**路径**页面上，键入 Active Directory 的数据库、日志文件和 SYSVOL 文件夹位置（或接受默认位置），然后单击**下一步**。  
  
    > [!IMPORTANT]  
    > 不要使用复原文件系统 (ReFS) 格式化的卷数据上存储 Active Directory 的数据库、日志文件或文件夹 SYSVOL。  
  
16. 在**准备选项**页面上，有足够运行 adprep 的键入凭据。 有关详细信息，请参阅[凭据要求运行 Adprep.exe 并安装 Active Directory 域服务](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)。  
  
17. 在**回顾选项**页上，确认你的选择，单击**查看脚本**如果你想要导出到一个 Windows PowerShell 脚本，设置，然后单击**下一步**。  
  
18. 在**先决条件检查**页上，确认已完成该先决条件验证，然后单击**安装**。  
  
19. 在**结果**页上，检查成功为域控制器配置了服务器。 将自动重新服务器以完成广告 DS 安装。  
  
## <a name="BKMK_UIStaged"></a>执行使用图形的用户界面转移 RODC 安装  
Rodc 允许您创建 RODC 在两个阶段。 在第一阶段的域管理员组成员创建 RODC 帐户。 在第二个阶段，服务器附加到 RODC 帐户。 由的域管理员组委派的域用户或组成员可以完成第二个阶段。  
  
#### <a name="to-create-an-rodc-account-by-using-the-active-directory-management-tools"></a>若要使用 Active Directory 管理工具创建 RODC 帐户  
  
1.  你可以创建使用 Active Directory 管理中心或 Active Directory 用户和计算机 RODC 帐户。  
  
    1.  单击**开始**，单击**管理工具**，然后单击**Active Directory 管理中心**。  
  
    2.  在导航窗格（左侧窗格）中，单击的域的名称。  
  
    3.  在管理列表（中心窗格）中，单击**域控制器**OU。  
  
    4.  在任务窗格（右窗格）中，单击**预先创建一个仅阅读域控制器帐户**。  
  
    --或  
  
    1.  单击**开始**，单击**管理工具**，然后单击**Active Directory 用户和计算机**。  
  
    2.  不论采取何右键单击**域控制器**部门 (OU) 或单击**域控制器**OU，然后单击**操作**。  
  
    3.  单击**预创建 Read-only 域控制器帐户**。  
  
2.  在**欢迎 Active Directory 域服务安装向导**页面上，如果您要修改默认密码复制策略 (PRP)，选择**高级模式安装使用**，然后单击**下一步**。  
  
3.  上**网络的凭据**页面上下,**指定帐户凭据，以用于执行安装**，单击**我当前登录凭据**或单击**备用凭据**，然后单击**设置**。 在**Windows 安全**对话框中，提供用户名和密码，可以安装额外的域控制器帐户。 若要安装额外的域控制器，你必须的企业管理员组或域管理员组成员。 当你完成提供凭据，请单击**下一步**。  
  
4.  在**指定计算机名称**页上，键入将 RODC 服务器的计算机名称。  
  
5.  在**选择某一站点**页上，从列表中选择某个站点或选择要将域控制器安装在运行该向导中，该计算机的 IP 地址对应的站点的选项，然后单击**下一步**。  
  
6.  在**其他域控制器选项**页上，选择以下选项，然后单击**下一步**:  
  
    -   **DNS 服务器**：以便域控制器可以作为域名系统 (DNS) 服务器，默认情况下选择此选项。 如果你不希望为 DNS 服务器的域控制器，清除此选项。 但是，如果不安装 DNS 服务器角色 RODC 和 RODC 分支机构中唯一的域控制器，branch 办公室中的用户将无法离线宽的区域广域网向中心网站时，请执行名称分辨率。  
  
    -   **全球目录**：此选项默认处于选中状态。 它将添加全球目录，域控制器，为只读目录分区并允许全球目录搜索功能。 如果你不希望为全球的目录服务器域控制器，清除此选项。 但是，如果未安装全球目录服务器，在分支机构或启用通用组成员缓存包括 RODC 站点，branch 办公室中的用户将无法在离线时向中心网站 WAN 登录到域。  
  
    -   **仅阅读域控制器**。 创建 RODC 帐户时，此选项默认处于选中状态，并且你无法将其清除。  
  
7.  如果你选择**高级模式安装使用**上复选框**欢迎**页上，**指定密码复制策略**页面显示时。 默认情况下，任何帐户密码复制到 RODC，，并且拥有自己的密码复制到 RODC 显式拒绝区分安全性的帐户（如域管理员组成员）。  
  
    若要将其他帐户添加到策略，请单击**添加**，然后单击**允许复制到此 RODC 的帐户的密码**或单击**复制到此 RODC 阻止的帐户的密码**，然后选择帐户。  
  
    完成后（或接受默认设置），请单击**下一步**。  
  
8.  在**RODC 安装委派和管理**页上，键入用户或组人员将连接到所需 RODC 帐户服务器的名称。 你可以键入只有一个安全主要用户的名称。  
  
    若要查找目录中的特定用户或组，请单击**设置**。 在**选择用户或组**、键入用户的名称或组。 我们建议你委派 RODC 安装和管理到某个组中。  
  
    此用户或组还将本地管理权限打开 RODC 安装完成后。 如果你没有指定用户或组，只有的域管理员组或企业管理员组成员将能够服务器附加到该帐户。  
  
    当你完成后，请单击**下一步**。  
  
9. 在**摘要**页上，检查你的选择。 单击**重新**更改任何的选择，如有必要。  
  
    若要将你选择的设置保存到你可以使用自动化后续广告 DS 操作，请单击应答文件**导出设置**。 键入你的答案文件的名称，然后单击**保存**。  
  
    当你确定您的选择是准确，请单击**下一步**创建 RODC 帐户。  
  
10. 在**完成 Active Directory 域服务安装向导**页上，单击**完成**。  
  
创建 RODC 帐户后，你可以向帐户，以完成安装 RODC 附加服务器。 可将其中 RODC 分公司完成此第二个阶段。 在你执行此过程该服务器未必须加入域。 开始在 Windows Server 2012，用于添加角色向导服务器管理器中连接到 RODC 帐户的服务器。  
  
#### <a name="to-attach-a-server-to-an-rodc-account-using-server-manager"></a>若要连接到使用服务器管理器 RODC 帐户服务器  
  
1.  本地管理员身份登录。  
  
2.  在服务器管理器中，单击**添加角色和功能**。  
  
3.  在**在开始之前**页上，单击**下一步**。  
  
4.  在**选择安装类型**页上，单击**角色基于或功能的安装**，然后单击**下一步**。  
  
5.  在**选择目标服务器**页上，单击**从服务器池选择服务器**，单击你希望安装广告 DS，然后单击的服务器的名称**下一步**。  
  
6.  在**选择服务器角色**页上，单击**Active Directory 域服务**，单击**添加功能**，然后单击**下一步**。  
  
7.  在**选择功能**页上，选择你想要安装单击任何其他功能**下一步**。  
  
8.  在**Active Directory 域服务**页上，检查信息，然后单击**下一步**。  
  
9. 在**确认安装选择**页上，单击**安装**。  
  
10. 上**结果**页上，检查**成功安装**，然后单击**推广此域控制器服务器**启动 Active Directory 域服务配置向导。  
  
    > [!IMPORTANT]  
    > 如果你无需启动 Active Directory 域服务配置向导关闭添加角色向导此时，你可以通过单击任务服务器管理器中重新启动它。  
  
    (media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)  
  
11. 在**部署配置**页上，单击**添加到现有的域的域控制器**、域名 (例如，emea.contoso.com) 的键入和凭据（如指定委派管理和安装 RODC 帐户），然后单击**下一步**。  
  
12. 在**域控制器选项**页上，单击**使用现有 RODC 帐户**、键入并确认目录服务还原模式密码，然后单击**下一步**。  
  
13. 在**其他选项**页面上，如果你安装媒体，从单击**从媒体路径安装**键入并确认安装源代码文件路径中，选择你想要复制的广告 DS 安装数据（或允许可选择任意域控制器向导）的域控制器，然后单击**下一步**。  
  
14. 在**路径**页上，键入 Active Directory 的数据库、日志文件和 SYSVOL 文件夹的位置或接受默认位置，然后单击**下一步**。  
  
15. 在**回顾选项**页上，确认你的选择，单击**查看脚本**导出到一个 Windows PowerShell 脚本，设置，然后单击**下一步**。  
  
16. 在**先决条件检查**页上，确认已完成该先决条件验证，然后单击**安装**。  
  
    若要完成广告 DS 安装，服务器将自动重新启动。  
  
## <a name="see-also"></a>请参阅  
[解决了域控制器部署](Troubleshooting-Domain-Controller-Deployment.md)  
[安装新的 Windows Server 2012 的 Active Directory 森林 & #40;级别 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md)  
[安装新的 Windows Server 2012 的 Active Directory 孩子或树域 & #40;级别 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  
[将副本 Windows Server 2012 域控制器安装现有域 & #40; 在级别 200 & #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  
  




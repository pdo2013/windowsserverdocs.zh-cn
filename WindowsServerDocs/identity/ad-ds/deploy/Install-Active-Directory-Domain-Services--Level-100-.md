---
ms.assetid: ae241ed8-ef19-40a9-b2d5-80b8391551ff
title: 安装 Active Directory 域服务（级别 100）
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 85ca76c4de319f2dec0e5b92b5ee73033c245f5a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390815"
---
# <a name="install-active-directory-domain-services-level-100"></a>安装 Active Directory 域服务（级别 100）

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题说明如何使用以下任一方法在 Windows Server 2012 中安装 AD DS：  

-   [凭据要求运行 al.exe 并安装 Active Directory 域服务](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)  

-   [使用 Windows PowerShell 安装 AD DS](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PS)  

-   [使用服务器管理器安装 AD DS](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_GUI)  

-   [使用图形用户界面执行暂存式 RODC 安装](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_UIStaged)  

## <a name="BKMK_Creds"></a>凭据要求运行 al.exe 并安装 Active Directory 域服务  
需要以下凭据才能运行 Adprep.exe 和安装 AD DS。  

-   若要安装新林，必须以该计算机的本地管理员身份登录。  

-   若要安装新子域或新域树，必须以 Enterprise Admins 组成员身份登录。  

-   若要在现有域中安装其他域控制器，必须是 Domain Admins 组的成员。  

    > [!NOTE]  
    > 如果你不单独运行 al.exe 命令并且在现有域或林中安装运行 Windows Server 2012 的第一个域控制器，系统将提示你提供凭据以运行 Adprep 命令。 凭据要求如下：  
    >   
    > -   若要在林中介绍第一个 Windows Server 2012 域控制器，需要为托管架构主机的域中的 Enterprise Admins 组、Schema Admins 组和 Domain Admins 组的成员提供凭据。  
    > -   若要在域中引入第一个 Windows Server 2012 域控制器，需要提供 Domain Admins 组的成员的凭据。  
    > -   若要在林中推出第一个只读域控制器 (RODC)，需要提供 Enterprise Admins 组的成员凭据。  
    >   
    >     > [!NOTE]  
    >     > 如果已在 Windows Server 2008 或 Windows Server 2008 R2 中运行 adprep/rodcprep，则无需再次运行 Windows Server 2012。  

## <a name="BKMK_PS"></a>使用 Windows PowerShell 安装 AD DS  
从 Windows Server 2012 开始，你可以使用 Windows PowerShell 安装 AD DS。 从 Windows Server 2012 开始，dcpromo.exe 已弃用，但仍可以使用应答文件（dcpromo/unattend： <answerfile> 或 dcpromo/answer： <answerfile>）运行 dcpromo.exe。 可使用应答文件继续运行 dcpromo.exe 的功能让在现有自动化方面具有资源投入的组织有时间将自动化从 dcpromo.exe 转换为 Windows PowerShell。 有关使用应答文件运行 dcpromo.exe 的详细信息，请参阅[https://support.microsoft.com/kb/947034](https://support.microsoft.com/kb/947034)。  

有关使用 Windows PowerShell 删除 AD DS 的详情，请参阅[使用 Windows PowerShell 删除 AD DS](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c#BKMK_RemovePS)。  

开始使用 Windows PowerShell 添加角色。 此命令将安装 AD DS 服务器角色并安装 AD DS 和 AD LDS 服务器管理工具，包括基于 GUI 的工具（如 Active Directory 用户和计算机）和命令行工具（如 dcdia.exe）。 使用 Windows PowerShell 时，默认情况下不安装服务器管理工具。 需要指定 **"IncludeManagementTools**来管理本地服务器或安装[远程服务器管理工具](https://www.microsoft.com/download/details.aspx?id=28972)以管理远程服务器。  

```  
Install-windowsfeature -name AD-Domain-Services -IncludeManagementTools  
<<Windows PowerShell cmdlet and arguments>>  
```  

仅当 AD DS 安装完成后，才需要重新启动。  

接着，可以运行此命令来查看 ADDSDeployment 模块中的可用 cmdlet。  

```  
Get-Command -Module ADDSDeployment
```  

若要查看可为 cmdlet 指定的参数及其语法的列表，请执行下列操作：  

```  
Get-Help <cmdlet name>  
```  

例如，若要查看用于创建未占用的只读域控制器 (RODC) 帐户的参数，请键入  

```  
Get-Help Add-ADDSReadOnlyDomainControllerAccount
```  

可选参数将显示在方括号中。  

你也可以下载 Windows PowerShell cmdlet 的最新帮助示例和概念。 有关详细信息，请参阅 [about_Updatable_Help](https://technet.microsoft.com/library/hh847735.aspx)。  

你可以针对远程服务器运行 Windows PowerShell cmdlet：  

-   在 Windows PowerShell 中，使用 ADDSDeployment cmdlet 的 Invoke 命令。 例如，若要在 contoso.com 域中名为 ConDC3 的远程服务器上安装 AD DS，请键入：  

    ```  
    Invoke-Command { Install-ADDSDomainController -DomainName contoso.com -Credential (Get-Credential) } -ComputerName ConDC3  
    ```  

-或-  

-   在服务器管理器中，创建包括远程服务器的服务器组。 右键单击远程服务器的名称，再单击 **“Windows PowerShell”** 。  

以下几个部分将讲解如何运行 ADDSDeployment 模块 cmdlet 以安装 AD DS。  

-   [ADDSDeployment cmdlet 参数](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params)  

-   [指定 Windows PowerShell 凭据](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSCreds)  

-   [使用测试 cmdlet](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_TestCmdlets)  

-   [使用 Windows PowerShell 安装新的目录林根级域](../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSForest)  

-   [使用 Windows PowerShell 安装新子域或树域](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSDomain)  

-   [使用 Windows PowerShell 安装附加（副本）域控制器](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSReplica)  

### <a name="BKMK_Params"></a>ADDSDeployment cmdlet 参数  
以下表格列出了 Windows PowerShell 中 ADDSDeployment cmdlet 的参数。 以粗体显示的参数为必填的。 如果在 Windows PowerShell 中命名方式不同，dcpromo.exe 的等效参数将列在括号中。  

Windows PowerShell 开关接受 $TRUE 或 $FALSE 参数。 默认情况下，不需要指定 $TRUE 的参数。  

若要覆盖默认值，可以使用 $False 值指定参数。 例如，由于未指定时将为新林安装自动运行 **-installdns**，因此安装新林时*阻止* DNS 安装的唯一方法是使用：  

```  
-InstallDNS:$false  
```  

同样，因为如果在不托管 Windows Server DNS 服务器的环境中安装域控制器， **"installdns**的默认值为 $False，则需要指定以下参数才能安装 DNS 服务器：  

```  
-InstallDNS:$true  
```  


|                                                                                                                 参数                                                                                                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ADPrepCredential <PS Credential>** **注意：** 如果要在域或林中安装第一个 Windows Server 2012 域控制器且当前用户的凭据不足以执行操作，则为必需项。 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    指定具有 Enterprise Admins 和 Schema Admins 组成员身份的帐户，以根据 [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) 和 PSCredential 对象的规则准备林。<br /><br />如果未指定任何值，则使用 **"credential** " 参数的值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|                                                                                                      AllowDomainControllerReinstall                                                                                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    指定在检测到具有相同名称的其他可写域控制器帐户的情况下是否继续安装此可写域控制器。<br /><br />仅当确定其他可写域控制器当前未使用帐户时，才使用 **$True**。<br /><br />默认值为 **$False**。<br /><br />此参数不适用于 RODC。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|                                                                                                           AllowDomainReinstall                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        指定是否重新创建现有域。<br /><br />默认值为 **$False**。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|                                                                                             AllowPasswordReplicationAccountName <字符串 []>                                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         指定密码可复制到此 RODC 的用户帐户、组帐户和计算机帐户的名称。 如果要将值留空，请使用空字符串 ""。 默认情况下，仅允许 “Allowed RODC Password Replication Group”，且其原始创建时为空。<br /><br />提供值作为字符串数组。 例如：<br /><br />AllowPasswordReplicationAccountName "JSmith"、"JSmithPC"、"分支用户"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|                      ApplicationPartitionsToReplicate < string [] >**注意：** UI 中无等效选项。 如果使用 UI 或 IFM 安装，则将复制所有应用程序分区。                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    指定要复制的应用程序目录分区。 仅当指定从媒体安装 (IFM) 的 **-InstallationMediaPath** 参数时，才能应用此参数。 默认情况下，所有应用程序分区都将根据自己的作用域进行复制。<br /><br />提供值作为字符串数组。 例如：<br /><br />编写<br /><br />-ApplicationPartitionsToReplicate "partition1"、"partition2"、"partition3"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|                                                                                                                 Confirm                                                                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        运行 cmdlet 之前提示你进行确认。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|                                                         CreateDnsDelegation**注意：** 运行 Add-ADDSReadOnlyDomainController cmdlet 时，无法指定此参数。                                                         |                                                                                                                                                                                                                                                                                                                                                                                                                                   指示是否创建引用与域控制器一同安装的新 DNS 服务器的 DNS 委派。 仅对 Active Directory "集成的 DNS 有效。 只能在联机且可访问的 Microsoft DNS 服务器上创建委派记录。 对于直接从属于顶级域的域，如 .com、gov、.biz、.edu 或 .nz 和 .au 等两个字母的国家/地区代码域，不能创建委派记录。<br /><br />将根据环境自动计算默认值。                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|                                                 **Credential <PS Credential>** **注意：** 仅当当前用户的凭据不足以执行操作时，才为必填项。                                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                根据 [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) 和 PSCredential 对象的规则，指定可登录域的域帐户。<br /><br />如果未指定任何值，将使用当前用户的凭据。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|                                                                                                         CriticalReplicationOnly                                                                                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         指定 AD DS 安装操作是否仅在重新启动前执行关键复制，然后继续。 将在安装完成且计算机重新启动之后进行非关键复制。<br /><br />不建议使用此参数。<br /><br />用户界面 (UI) 无等效选项。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|                                                                                                          DatabasePath <string>                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                      指定包含域数据库的本地计算机的固定磁盘上目录的完全限定的非通用命名约定（UNC）路径，例如， **c:\windows\ntds。**<br /><br />默认值为 **%SYSTEMROOT%\NTDS**。 **重要说明：** 将 AD DS 数据库和日志文件存储到使用复原文件系统 (ReFS) 格式化的数据卷上时，对于在 ReFS 上存放 AD DS 没有特别益处，不过，对于存放在 ReFS 的任何数据，你都可以获得正常的弹性能力优势。                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                DelegatedAdministratorAccountName <string>                                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                指定将安装和管理 RODC 的用户或组的名称。<br /><br />默认情况下，仅 Domain Admins 组的成员可以管理 RODC。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|                                                                                              DenyPasswordReplicationAccountName <字符串 []>                                                                                              |                                                                                                                                                                                                                                                                                                           指定密码不复制到此 RODC 的用户帐户、组帐户和计算机帐户的名称。 如果你不希望拒绝复制任何用户或计算机的凭据，请使用空字符串 ""。 默认情况下，拒绝 Administrators、Server Operators、Backup Operators、Account Operators 和 Denied RODC Password Replication Group。 默认情况下，Denied RODC Password Replication Group 包括 Cert Publishers、Domain Admins、Enterprise Admins、Enterprise Domain Controllers、Enterprise Read-Only Domain Controllers、Group Policy Creator Owners、krbtgt 帐户和 Schema Admins。<br /><br />提供值作为字符串数组。 例如：<br /><br />编写<br /><br />-DenyPasswordReplicationAccountName "RegionalAdmins"，"AdminPCs"                                                                                                                                                                                                                                                                                                            |
|                                               DnsDelegationCredential <PS Credential>**注意：** 运行 Add-ADDSReadOnlyDomainController cmdlet 时，无法指定此参数。                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      根据 [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) 和 PSCredential 对象的规则，指定用于创建 DNS 委派的用户名和密码。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                        DomainMode <DomainMode> {Win2003 &#124; Win2008 &#124; Win2008R2 &#124; Win2012 &#124; Win2012R2}<br /><br />或<br /><br />DomainMode <DomainMode> {2 &#124; 3 &#124; 4 &#124; 5 &#124; 6}                        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             在新域创建期间指定域功能级别。<br /><br />域功能级别不能低于林控制级别，但可以高于林控制级别。<br /><br />默认值将自动计算并设置为现有林功能级别或为 **-ForestMode** 设置的值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|                                                                   **DomainName**<br /><br />对于 Install-ADDSForest 和 Install-ADDSDomainController cmdlet，为必填项。                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     指定要在其中安装其他域控制器的域的 FQDN。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|                                                       **DomainNetbiosName <string>**<br /><br />如果 FQDN 前缀名称长于 15 个字符，则对于 Install-ADDSForest 为必填项。                                                        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           与 Install-ADDSForest 配合使用。 将 NetBIOS 名称分配给新林根域。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                              DomainType <DomainType> {ChildDomain &#124; TreeDomain} 或 {child &#124; tree}                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  指示要创建的域类型：现有林中的新域树、现有域的子域或新林。<br /><br />DomainType 默认值为 ChildDomain。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|                                                                                                                  Force                                                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             指定此参数时，将抑制通常在安装和添加域控制器时可能显示的任何警告，以允许 cmdlet 完成执行。 制作安装脚本时，包括此参数将很有用。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|                        ForestMode <ForestMode> {Win2003 &#124; Win2008 &#124; Win2008R2 &#124; Win2012 &#124; Win2012R2}<br /><br />或<br /><br />ForestMode <ForestMode> {2 &#124; 3 &#124; 4 &#124; 5 &#124; 6}                        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              创建新林时，指定林功能级别。<br /><br />默认值为 Win2012。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|                                                                                                          InstallationMediaPath                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 指明用于安装新域控制器的安装介质的位置。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                                                                InstallDns                                                                                                                |                                                                                                                                                                                                                                                                                                                      指定是否应该在域控制器上安装和配置 DNS 服务器服务。<br /><br />对于新林，默认值为 **$True** 并安装 DNS 服务器。<br /><br />对于新子域或域树，如果父域（或域树的林根域）已托管和存储域的 DNS 名称，则此参数的默认值为 $True。<br /><br />对于现有域中的域控制器安装，如果未指定此参数且当前域已托管和存储域的 DNS 名称，则此参数的默认值为 **$True**。 否则，如果在 Active Directory 外部托管 DNS 域名，默认值为 **$False** 且不安装 DNS 服务器。                                                                                                                                                                                                                                                                                                                      |
|                                                                                                             LogPath <string>                                                                                                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            指定指向本地计算机（包含域日志文件）固定磁盘上目录的完全限定的非 UNC 路径，例如，**C:\Windows\Logs**。<br /><br />默认值为 **%SYSTEMROOT%\NTDS**。 **重要说明：** 请勿将 Active Directory 日志文件存储到使用复原文件系统 (ReFS) 格式化的数据卷上。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|                                                                                             MoveInfrastructureOperationMasterRoleIfNecessary                                                                                             |                                                                                                                                                                                                                                                                                                                                                                                                  指定是否将基础结构主机操作主机角色（也称为灵活单主机操作或 FSMO）传输到要创建的域控制器（如果它当前托管在全局目录服务器上），并且不打算使域控制器成为你要创建的全局编录服务器。 指定此参数将在需要传输时将基础结构主机角色传输到要创建的域控制器；在这种情况下，如果要将基础结构主机角色保留在原位置，则指定 **NoGlobalCatalog** 选项。                                                                                                                                                                                                                                                                                                                                                                                                  |
|                                                                                **NewDomainName <string>** **注意：** 仅对于 Install-ADDSDomain 才为必填项。                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           指定新域的单域名。<br /><br />例如，如果要创建名为 **emea.corp.fabrikam.com** 的新子域，则应该指定 **emea** 作为此参数的值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|                                                      **NewDomainNetbiosName <string>**<br /><br />如果 FQDN 前缀名称长于 15 个字符，则对于 Install-ADDSDomain 为必填项。                                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               与 Install-ADDSDomain 配合使用。 将 NetBIOS 名称分配给新域。 默认值是从 **"NewDomainName"** 的值派生的。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|                                                                                                              NoDnsOnNetwork                                                                                                              |                                                                                                                                                                                                                                                                                                                                    指定 DNS 服务在网络上不可用。 仅当未使用用于名称解析的 DNS 服务器的名称来配置此计算机的网络适配器的 IP 设置时，才使用此参数。 它表明此 DNS 服务器将安装在用于名称解析的计算机上。 否则，必须先使用 DNS 服务器的地址配置网络适配器的 IP 设置。<br /><br />忽略此参数（默认值）表示此服务器计算机上的网络适配器的 TCP/IP 客户端设置将用于联系 DNS 服务器。 因此，如果未指定此参数，请确保先使用首选 DNS 服务器地址配置 TCP/IP 客户端设置。                                                                                                                                                                                                                                                                                                                                     |
|                                                                                                             NoGlobalCatalog                                                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   指定不希望域控制器成为全局编录服务器。<br /><br />默认情况下，运行 Windows Server 2012 的域控制器与全局编录一起安装。 换言之，这将在未计算的情况下自动运行，除非指定：<br /><br />编写<br /><br />-NoGlobalCatalog                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|                                                                                                           NoRebootOnCompletion                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     指定命令完成后（无论是否成功）是否重新启动计算机。 默认情况下，计算机将重新启动。 若要防止服务器重新启动，请指定：<br /><br />编写<br /><br />-NoRebootOnCompletion： $True<br /><br />用户界面 (UI) 无等效选项。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|                                                                              **ParentDomainName <string>** **注意：** 对于 Install-ADDSDomain cmdlet，为必填项                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 指定现有父域的 FQDN。 安装子域或新域树时，将使用此参数。<br /><br />例如，如果要创建名为 **emea.corp.fabrikam.com** 的新子域，则应该指定 **corp.fabrikam.com** 作为此参数的值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|                                                                                                             ReadOnlyReplica                                                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   指定是否安装只读域控制器 (RODC)。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|                                                                                                       ReplicationSourceDC <string>                                                                                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              指示从中复制域信息的伙伴域控制器的 FQDN。 默认值将自动计算。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|                                                                                             **SafeModeAdministratorPassword <securestring>**                                                                                             | 以安全模式或安全模式的变体（如目录服务还原模式）启动计算机时，提供管理员帐户的密码。<br /><br />默认值为空密码。 你必须提供密码。 密码必须以 System.Security.SecureString 格式提供，如 read-host -assecurestring 或 ConvertTo-SecureString 提供的格式。<br /><br />SafeModeAdministratorPassword 参数的操作特殊：如果未指定为参数，cmdlet 将提示你输入并确认掩蔽密码。 以交互方式运行 cmdlet 时，这是首选用法。如果未指定值，且 cmdlet 未指定其他参数，cmdlet 将提示你输入掩蔽密码而予以确认。 以交互方式运行 cmdlet 时，这不是首选用法。如果指定值，则该值必须是安全字符串。 以交互方式运行 cmdlet 时，这不是首选用法。例如，你可以通过使用读取主机 cmdlet 提示用户提供安全字符串来手动提示输入密码：-safemodeadministratorpassword （Read-Host 提示符 "Password："-assecurestring）你还可以提供安全字符串作为转换明文变量，尽管强烈建议这样做。 -safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force) |
|                                                                     **SiteName <string>**<br /><br />对于 Add-addsreadonlydomaincontrolleraccount cmdlet，为必填项                                                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  指定将安装域控制器的站点。 当你运行**install-addsforest**时，没有 **"sitename"** 参数，原因是创建的第一个站点是默认值-站点名称。<br /><br />作为参数提供给 **-sitename** 时，此站点名称必须已存在。 cmdlet 不会创建站点。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|                                                                                                           SkipAutoConfigureDNS                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           跳过 DNS 客户端设置、转发器和根提示的自动配置。 仅当 DNS 服务器服务已安装或已使用 **-InstallDNS** 自动安装时，此参数才会生效。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|                                                                                                            SystemKey <string>                                                                                                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            指定从中复制数据的介质的系统密钥。<br /><br />默认值为 **none**。<br /><br />数据必须采用 read-host -assecurestring 或 ConvertTo-SecureString 提供的格式。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|                                                                                                           SysvolPath <string>                                                                                                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     指定指向本地计算机固定磁盘上目录的完全限定的非 UNC 路径，例如，**C:\Windows\SYSVOL**。<br /><br />默认值为 **%SYSTEMROOT%\SYSVOL**。 **重要说明：** 不能将 SYSVOL 存储到使用恢复文件系统 (ReFS) 格式化的数据卷上。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                              SkipPreChecks                                                                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              在开始安装前，不运行先决条件检查。 不建议使用此设置。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|                                                                                                                  WhatIf                                                                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   显示如果运行 cmdlet 则会发生什么情况。 cmdlet 未运行。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

### <a name="BKMK_PSCreds"></a>指定 Windows PowerShell 凭据  
通过使用 [Get-credential](https://technet.microsoft.com/library/dd315327.aspx)，可以指定凭据，且不在屏幕上以纯文本形式透露它们。  

-SafeModeAdministratorPassword 和 LocalAdministratorPassword 参数的操作特殊：  

-   如果未指定为参数，cmdlet 将提示你输入并确认掩蔽密码。 以交互方式运行 cmdlet 时，这是首选用法。  

-   如果指定值，则该值必须是安全字符串。 以交互方式运行 cmdlet 时，这不是首选用法。  

例如，通过使用 **Read-Host** cmdlet 提示用户提供安全字符串，可手动提示输入密码。  

```  
-SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString)
```  

> [!WARNING]  
> 由于上个选项未确认密码，请务必十分谨慎：密码不可见。  

你也可以提供安全字符串作为转换的明文变量，尽管强烈不建议这样做：  

```  
-SafeModeAdministratorPassword (ConvertTo-SecureString "Password1" -AsPlainText -Force)
```  

> [!WARNING]  
> 不建议提供或存储明文密码。 任何在脚本中运行此命令或看到的人员都知道该域控制器的 DSRM 密码。 知道密码后，他们就可以模拟域控制器本身并将权限提升到 Active Directory 林中的最高级别。  

### <a name="BKMK_TestCmdlets"></a>使用测试 cmdlet  
每个 ADDSDeployment cmdlet 都具有对应的测试 cmdlet。 测试 cmdlet 仅针对安装操作运行先决条件检查；不会配置任何安装设置。 每个测试 cmdlet 的参数与相应安装 cmdlet 的参数相同，但 **"skipprechecks 不可"** 不可用于测试 cmdlet。  

|测试 cmdlet|描述|  
|---------------|---------------|  
|Test-ADDSForestInstallation|运行安装新 Active Directory 林的前提条件。|  
|Test-ADDSDomainInstallation|运行在 Active Directory 中安装新域的先决条件。|  
|Test-ADDSDomainControllerInstallation|运行在 Active Directory 中安装域控制器的先决条件。|  
|Test-ADDSReadOnlyDomainControllerAccountCreation|运行添加只读域控制器 (RODC) 帐户的先决条件。|  

### <a name="BKMK_PSForest"></a>使用 Windows PowerShell 安装新的目录林根级域  
安装新林的命令语法如下。 可选参数将显示在方括号内。  

```  
Install-ADDSForest [-SkipPreChecks] -DomainName <string> -SafeModeAdministratorPassword <SecureString> [-CreateDNSDelegation] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-DomainNetBIOSName <string>] [-ForestMode <ForestMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-InstallDNS] [-LogPath <string>] [-NoRebootOnCompletion] [-SkipAutoConfigureDNS] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  

> [!NOTE]  
> 如果要更改根据 DNS 域名前缀自动生成的 15 字符名称或名称超过 15 个字符，则需要 -DomainNetBIOSName 参数。  

例如，若要安装名为 corp.contoso.com 的新林并让系统安全提示提供 DSRM 密码，请键入：  

```  
Install-ADDSForest -DomainName "corp.contoso.com"   
```  

> [!NOTE]  
> 运行 Install-ADDSForest 时，默认情况下会安装 DNS 服务器。  

若要安装名为 corp.contoso.com 的新林，在 contoso.com 域中创建 DNS 委派，将域功能级别设置为 Windows Server 2008 R2 并将林功能级别设置为 Windows Server 2008，在 D:\ 驱动器上安装 Active Directory 数据库和 SYSVOL，在 E:\ 驱动器上安装日志文件，以及让系统提示提供目录服务还原模式密码，请键入：  

```  
Install-ADDSForest -DomainName corp.contoso.com -CreateDNSDelegation -DomainMode Win2008 -ForestMode Win2008R2 -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  

### <a name="BKMK_PSDomain"></a>使用 Windows PowerShell 安装新子域或树域  
安装新域的命令语法如下。 可选参数将显示在方括号内。  

```  
Install-ADDSDomain [-SkipPreChecks] -NewDomainName <string> -ParentDomainName <string> -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainReinstall] [-CreateDNSDelegation] [-Credential <PS Credential>] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [DomainType <DomainType> {Child Domain | TreeDomain} [-InstallDNS] [-LogPath <string>] [-NoGlobalCatalog] [-NewDomainNetBIOSName <string>] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-Systemkey <SecureString>] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  

> [!NOTE]  
> 仅当当前以 Enterprise Admins 组成员身份登录，才需要 **-credential** 参数。  
>   
> 如果要更改根据 DNS 域名前缀自动生成的 15 字符名称或名称超过 15 个字符，需要 **-NewDomainNetBIOSName** 参数。  

例如，若要使用 corp\EnterpriseAdmin1 的凭据创建名为 child.corp.contoso.com 的新子域，安装 DNS 服务器，在 corp.contoso.com 域中创建 DNS 委派，将域功能级别设置为 Windows Server 2003，将域控制器设为名为 Houston 的域中的全局目录服务器，将 DC1.corp.contoso.com 用作复制源域控制器，在 D:\ 驱动器上安装 Active Directory 数据库和 SYSVOL，在 E:\ 驱动器上安装日志文件，并让系统提示提供目录服务还原模式密码但不提示确认命令，请键入：  

```  
Install-ADDSDomain -SafeModeAdministratorPassword -Credential (get-credential corp\EnterpriseAdmin1) -NewDomainName child -ParentDomainName corp.contoso.com -InstallDNS -CreateDNSDelegation -DomainMode Win2003 -ReplicationSourceDC DC1.corp.contoso.com -SiteName Houston -DatabasePath "d:\NTDS" "SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs" -Confirm:$False  
```  

### <a name="BKMK_PSReplica"></a>使用 Windows PowerShell 安装附加（副本）域控制器  
安装其他域控制器的命令语法如下。 可选参数将显示在方括号内。  

```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainControllerReinstall] [-ApplicationPartitionsToReplicate <string[]>] [-CreateDNSDelegation] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-NoGlobalCatalog] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  

若要在 corp.contoso.com 域中安装域控制器和 DNS 服务器并让系统提示提供域管理员凭据和 DSRM 密码，请键入：  

```  
Install-ADDSDomainController -Credential (Get-Credential CORP\Administrator) -DomainName "corp.contoso.com"
```  

如果计算机已加入域且你是 Domain Admins 组的成员，则可以使用：  

```  
Install-ADDSDomainController -DomainName "corp.contoso.com"  
```  

若要让系统提示提供域名，请键入：  

```  
Install-ADDSDomainController -Credential (Get-Credential) -DomainName (Read-Host "Domain to promote into")
```  

以下命令将使用 Contoso\EnterpriseAdmin1 的凭据在名为 Boston 的站点中安装可写域控制器和全局编录服务器，安装 DNS 服务器，在 contoso.com 域中创建 DNS 委派，从存储在 c:\ADDS IFM 文件夹中的媒体安装，在 D:\ 驱动器上安装 Active Directory 数据库和 SYSVOL，在 E:\ 驱动器上安装日志文件，让服务器在 AD DS 安装完成后自动重新启动，以及让系统提示提供目录服务还原模式密码：  

```  
Install-ADDSDomainController -Credential (Get-Credential CONTOSO\EnterpriseAdmin1) -CreateDNSDelegation -DomainName corp.contoso.com -SiteName Boston -InstallationMediaPath "c:\ADDS IFM" -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  

### <a name="performing-a-staged-rodc-installation-using-windows-powershell"></a>使用 Windows PowerShell 执行 RODC 分步安装  
创建 RODC 帐户的命令语法如下。 可选参数将显示在方括号内。  

```  
Add-ADDSReadOnlyDomainControllerAccount [-SkipPreChecks] -DomainControllerAccuntName <string> -DomainName <string> -SiteName <string> [-AllowPasswordReplicationAccountName <string []>] [-NoGlobalCatalog] [-Credential <PS Credential>] [-DelegatedAdministratorAccountName <string>] [-DenyPasswordReplicationAccountName <string []>] [-InstallDNS] [-ReplicationSourceDC <string>] [-Force] [-WhatIf] [-Confirm] [<Common Parameters>]  
```  

将服务器连接到 RODC 帐户的命令语法如下。 可选参数将显示在方括号内。  

```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-ApplicationPartitionsToReplicate <string[]>] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-NoDNSOnNetwork] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-UseExistingAccount] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  

例如，若要创建名为 RODC1 的 RODC 帐户，请键入：  

```  
Add-ADDSReadOnlyDomainControllerAccount -DomainControllerAccountName RODC1 -DomainName corp.contoso.com -SiteName Boston DelegatedAdministratoraccountName PilarA  
```  

然后在要连接到 RODC1 帐户的服务器上运行以下命令。 服务器无法加入域。 首先，安装 AD DS 服务器角色和管理工具：  

```  
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```  

然后运行下列命令以创建 RODC：  

```  
Install-ADDSDomainController -DomainName corp.contoso.com -SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString) -Credential (Get-Credential Corp\PilarA) -UseExistingAccount
```  

按**Y**确认或包含 **"确认**参数以防止出现确认提示。  

## <a name="BKMK_GUI"></a>使用服务器管理器安装 AD DS  
AD DS 可以通过使用服务器管理器中的 "添加角色向导" 在 Windows Server 2012 中安装，后跟 Active Directory 域服务配置向导，该向导是从 Windows Server 2012 开始的新的。 从 Windows Server 2012 开始，已弃用 Active Directory 域服务安装向导（dcpromo.exe）。  

以下几个部分讲解如何创建服务器池以在多台服务器上安装和管理 AD DS，以及如何使用相应向导安装 AD DS。  

### <a name="BKMK_ServerPools"></a>创建服务器池  
只要可从运行服务器管理器的计算机访问，服务器管理器就将在网络上共用其他服务器。 共用后，可选择用于远程安装 AD DS 的服务器或服务器管理器内的任何其他可能配置选项。 运行服务器管理器的计算机将自动共用本身。 有关服务器池的详细信息，请参阅[将服务器添加到服务器管理器](https://technet.microsoft.com/library/hh831453.aspx)。  

> [!NOTE]  
> 为了使用工作组服务器上的服务器管理器管理加入域的计算机，或进行相反的操作，需要执行额外的配置步骤。 有关详细信息，请参阅[将服务器添加到服务器管理器](https://technet.microsoft.com/library/hh831453.aspx)中的 "在工作组中添加和管理服务器"。  

### <a name="BKMK_installADDSGUI"></a>安装 AD DS  
**管理凭据**  

安装 AD DS 的凭据要求会因选择的部署配置而异。 有关详细信息，请参阅[运行 Adprep.exe 和安装 Active Directory 域服务的凭据要求](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)。  

采用以下过程来使用 GUI 方法安装 AD DS。 这些步骤可在本地或远程执行。 有关这些步骤的详细说明，请参阅以下主题：  

-   [使用服务器管理器部署林](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  

-   [在现有域&#40;级别200中安装副本 Windows Server 2012 域控制器&#41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  

-   [安装新的 Windows Server 2012 Active Directory 子或树域&#40;级别200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  

-   [安装 Windows Server 2012 Active Directory 只读域控制器&#40;RODC&#41; &#40;Level 200&#41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md)  

##### <a name="to-install-ad-ds-by-using-server-manager"></a>使用服务器管理器安装 AD DS  

1.  在服务器管理器中，单击 **“管理”** ，再单击 **“添加角色和功能”** 以启动添加角色向导。  

2.  在“开始之前” 页上，单击“下一步”。  

3.  在 **“选择安装类型”** 页上，单击 **“基于角色或基于功能的安装”** ，再单击 **“下一步”** 。  

4.  在 **“选择目标服务器”** 页上，单击 **“从服务器池中选择服务器”** ，单击要安装 AD DS 的服务器的名称，再单击 **“下一步”** 。  

    若要选择远程服务器，请先创建服务器池，再将远程服务器添加到其中。 有关创建服务器池的详细信息，请参阅[添加服务器到服务器管理器](https://technet.microsoft.com/library/hh831453.aspx)。  

5.  在 **“选择服务器角色”** 页中，单击 **“Active Directory 域服务”** ，然后在 **“添加角色和功能向导”** 对话框中，单击 **“添加功能”** ，再单击 **“下一步”** 。  

6.  在 **“选择功能”** 页上，选择要安装的任何附加功能，再单击 **“下一步”** 。  

7.  在 **“Active Directory 域服务”** 页上，查看信息，再单击 **“下一步”** 。  

8.  在 **“确认安装选择”** 页上，单击 **“安装”** 。  

9. 在 **“结果”** 页上，验证安装是否成功，再单击 **“将此服务器提升为域服务器”** ，以启动 Active Directory 域服务配置向导。  

    ![安装 AD DS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_SMPromotes.gif)  

    > [!IMPORTANT]  
    > 如果此时关闭添加角色向导且未启动 Active Directory 域服务配置向导，可以单击服务器管理器中的“任务”来重新启动它。  

    ![安装 AD DS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)  

10. 在 **“部署配置”** 页上，选择以下选项之一：  

    -   如果要在现有域中安装其他域控制器，请单击 "向**现有域添加域控制器**"，键入域的名称（例如，emea.corp.contoso.com）或单击 "**选择 ...** " 以选择域，然后凭据（例如，指定属于 Domain Admins 组成员的帐户），然后单击 "**下一步**"。  

        > [!NOTE]  
        > 默认情况下，仅当计算机已加入域且正在执行本地安装时，才提供域名和当前用户凭据。 如果要在远程服务器上安装 AD DS，根据设计需要指定凭据。 如果当前用户凭据不足以执行安装，请单击 "**更改 ...** "，以指定不同的凭据。  

        有关详细信息，请参阅[在现有域&#40;级别 200&#41;中安装副本 Windows Server 2012 域控制器](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)。  

    -   如果要安装新子域，请单击 **“将新域添加到现有林”** ，对于 **“选择域类型”** 选择 **“子域”** ，键入或浏览到父域 DNS 名称的名称（例如，corp.contoso.com），键入新子域的相对名称（例如 emea），键入用于创建新域的凭据，再单击 **“下一步”** 。  

        有关详细信息，请参阅[安装新的 Windows Server 2012 Active Directory 子或树&#40;域级别&#41;200](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)。  

    -   如果要安装新域树，请单击 **“将新域添加到现有林”** ，对于 **“选择域类型”** ，选择 **“树域”** ，键入根域的名称（例如，corp.contoso.com），键入新域的 DNS 名称（例如，fabrikam.com），键入用于创建新域的凭据，再单击 **“下一步”** 。  

        有关详细信息，请参阅[安装新的 Windows Server 2012 Active Directory 子或树&#40;域级别&#41;200](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)。  

    -   如果要安装新林，请单击 **“添加新林”** ，再键入根域的名称（例如，corp.contoso.com）。  

        有关详细信息，请参阅[安装新的 Windows Server 2012 Active Directory &#40;林级别&#41;200](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md)。  

11. 在 **“域控制器选项”** 页中，选择以下选项之一：  

    -   如果要创建新林或新域，请选择域功能级别和林功能级别，单击 **“域名系统 (DNS) 服务器”** ，指定 DSRM 密码， 再单击 **“下一步”** 。  

    -   如果要将域控制器添加到现有域，请根据需要单击 **“域名系统 (DNS) 服务器”** 、 **“全局编录 (GC)”** 或 **“只读域控制器 (RODC)”** ，选择站点名称，键入 DSRM 密码，再单击 **“下一步”** 。  

    有关不同情况下该页面的哪些选项可用或不可用的详细信息，请参阅[域控制器选项](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage)。  

12. 在 **“DNS 选项”** 页（仅当安装 DNS 服务器时才显示），根据需要单击 **“更新 DNS 委派”** 。 如果单击，请提供有权限在父 DNS 区域中创建 DNS 委派记录的凭据。  

    如果无法联系托管父区域的 DNS 服务器，则 **“更新 DNS 委派”** 选项不可用。  

    有关是否需要更新 DNS 委派的详细信息，请参阅[了解区域委派](https://technet.microsoft.com/library/cc771640.aspx)。 如果尝试更新 DNS 委派并遇到错误，请参阅 [DNS 选项](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)。  

13. 在 **“RODC 选项”** 页上（仅当安装 RODC 时才显示），指定管理 RODC 的组或用户的名称，将帐户添加到允许或拒绝的密码复制组或从中删除帐户，再单击 **“下一步”** 。  

    有关详细信息，请参阅[密码复制策略](https://technet.microsoft.com/library/cc730883(v=ws.10))。  

14. 在 **“其他选项”** 页上，选择以下选项之一：  

    -   如果要创建新域，请键入新的 NetBIOS 名称或验证域的默认 NetBIOS 名称，然后单击 **“下一步”** 。  

    -   如果要将域控制器添加到现有域，请选择要从中复制 AD DS 安装数据的域控制器（或允许向导选择任何域控制器）。 如果要从媒体安装，请单击 **“从媒体路径安装”** 类型，验证安装源文件的路径，再单击 **“下一步”** 。  

        不能使用从媒体安装 (IFM) 在域中安装第一个域控制器。 IFM 无法跨不同的操作系统版本。 换句话说，若要安装使用 IFM 运行 Windows Server 2012 的其他域控制器，则必须在 Windows Server 2012 域控制器上创建备份介质。 有关 IFM 的详细信息，请参阅[使用 IFM 安装其他域控制器](https://technet.microsoft.com/library/cc816722(WS.10).aspx)。  

15. 在 **“路径”** 页上，键入 Active Directory 数据库、日志文件和 SYSVOL 文件夹的位置（或接受默认位置），再单击 **“下一步”** 。  

    > [!IMPORTANT]  
    > 请勿将 Active Directory 数据库、日志文件或 SYSVOL 文件夹存储到使用恢复文件系统 (ReFS) 格式化的数据卷上。  

16. 在 **“准备选项”** 页上，键入足以运行 adprep 的凭据。 有关详细信息，请参阅[运行 Adprep.exe 和安装 Active Directory 域服务的凭据要求](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)。  

17. 在 **“查看选项”** 页上，确认选择，单击 **“查看脚本”** （如果要将设置导出到 Windows PowerShell 脚本），再单击 **“下一步”** 。  

18. 在 **“先决条件检查”** 页上，确认先决条件验证已完成，再单击 **“安装”** 。  

19. 在 **“结果”** 页上，验证已将服务器成功配置为域控制器。 服务器将自动重新启动，以完成 AD DS 安装。  

## <a name="BKMK_UIStaged"></a>使用图形用户界面执行暂存式 RODC 安装  
RODC 分步安装允许你分两步创建 RODC。 在第一步中，Domain Admins 组成员将创建 RODC 帐户。 在第二步中，将服务器连接到 RODC 帐户。 第二步可由 Domain Admins 组成员或委派的域用户或组完成。  

#### <a name="to-create-an-rodc-account-by-using-the-active-directory-management-tools"></a>使用 Active Directory 管理工具创建 RODC 帐户  

1.  你可以使用 Active Directory 管理中心或 Active Directory 用户和计算机创建 RODC 帐户。  

    1.  单击 **“开始”** ，单击 **“管理工具”** ，再单击 **“Active Directory 管理中心”** 。  

    2.  在导航窗格（左窗格）中，单击域名。  

    3.  在管理列表（中心窗格）中，单击 **Domain Controllers** OU。  

    4.  在“任务”窗格（右窗格）中，单击 **“预创建只读域控制器帐户”** 。  

    -或者-  

    1.  依次单击 **“开始”** 、 **“管理工具”** 和 **“Active Directory 用户和计算机”** 。  

    2.  右键单击**Domain Controllers**组织单位 (OU)，或者单击 **Domain Controllers** 组织单位，然后单击 **“操作”** 。  

    3.  单击 **“预创建只读域控制器帐户”** 。  

2.  在 **“欢迎使用 Active Directory 域服务安装向导”** 页上，如果你想修改默认的密码复制策略 (PRP)，请选择 **“使用高级模式安装”** ，然后单击 **“下一步”** 。  

3.  在 **“网络凭据”** 页上的 **“请指定用于执行安装的帐户凭据”** 下，单击 **“我的当前登录凭据”** 或单击 **“备用凭据”** ，然后单击 **“设置”** 。 在 **“Windows 安全”** 对话框中，提供可用来安装其他域控制器帐户的用户名和密码。 若要安装其他域控制器，你必须是 Enterprise Admins 组或 Domain Admins 组的成员。 提供凭据后，单击 **“下一步”** 。  

4.  在 **“指定计算机名”** 页上，键入将成为 RODC 的服务器的计算机名称。  

5.  在 **“请选择一个站点”** 页上，从列表中选择站点，或在与运行该向导的计算机的 IP 地址相对应的站点中，选择用于安装域控制器的选项，然后单击 **“下一步”** 。  

6.  在 **“其他域控制器选项”** 页上，进行如下选择，然后单击 **“下一步”** ：  

    -   **DNS 服务器**：默认情况下，此选项处于选中状态，以便域控制器可以充当域名系统（DNS）服务器。 如果你不希望该域控制器成为 DNS 服务器，则清除该选项。 但是，如果你没有在 RODC 上安装 DNS 服务器角色，并且该 RODC 是分支机构中唯一的域控制器，则分支机构中的用户将无法在中心站点的广域网 (WAN) 脱机时执行名称解析。  

    -   **全局编录**：默认情况下, 此选项处于选中状态。 它会将全局编录、只读目录分区添加到域控制器，并且将启用全局编录搜索功能。 如果你不希望域控制器成为全局编录服务器，请清除此选项。 但是，如果你没有在分支机构中安装全局编录服务器，或者没有为包含 RODC 的站点启用通用组成员身份缓存，则分支机构中的用户将无法在中心站点的 WAN 脱机时登录域。  

    -   **“只读域控制器”** 。 当创建 RODC 帐户时，该选项为默认选中且无法清除。  

7.  如果你选中了 **“欢迎使用”** 页上的 **“使用高级模式安装”** 复选框，则会出现 **“指定密码复制策略”** 页。 默认情况下，帐户密码不会复制到 RODC，并且明确拒绝安全敏感帐户（如 Domain Admins 组的成员）在任何时候将其密码复制到 RODC。  

    若要向策略添加其他帐户，请单击 **“添加”** ，然后单击 **“允许该帐户的密码复制到此 RODC”** 或单击 **“拒绝该帐户的密码复制到此 RODC”** ，然后选择帐户。  

    当设置完成时（或要接受默认设置时），单击 **“下一步”** 。  

8.  在 **“用于 RODC 安装和管理的委派”** 页上，键入将服务器关联到正在创建的 RODC 帐户的用户或组的名称。 你可以只键入一个安全主体的名称。  

    若要在目录中搜索特定用户或组，请单击“设置”。 在“选择用户、计算机或组”中，键入用户或组的名称。 我们建议你将 RODC 安装和管理委派给一个组。  

    安装之后，该用户或组在此 RODC 上也将具有本地管理权限。 如果未指定用户或组，则只有 Domain Admins 组或 Enterprise Admins 组的成员才能将服务器关联到帐户。  

    完成后，单击 **“下一步”** 。  

9. 在 **“摘要”** 页上，检查你的选择。 如有必要，请单击 **“上一步”** 更改任何选项。  

    若要将选择的设置保存到可用于自动执行后续 AD DS 操作的答案文件，请单击 "**导出设置**"。 键入答案文件名，然后单击 **“保存”** 。  

    确认所做选择正确无误之后，请单击 **“下一步”** 创建 RODC 帐户。  

10. 在 **“完成 Active Directory 域服务安装向导”** 页上，单击 **“完成”** 。  

创建 RODC 帐户后，可以将服务器连接到帐户，以完成 RODC 安装。 第二步可在 RODC 所在的分支机构完成。 执行此过程的服务器必须未加入域。 从 Windows Server 2012 开始，可以使用服务器管理器中的 "添加角色向导" 将服务器附加到 RODC 帐户。  

#### <a name="to-attach-a-server-to-an-rodc-account-using-server-manager"></a>使用服务器管理器将服务器连接到 RODC 帐户  

1.  以本地管理员身份登录。  

2.  在服务器管理器中，单击“添加角色和功能”。  

3.  在“开始之前” 页上，单击“下一步”。  

4.  在 **“选择安装类型”** 页上，单击 **“基于角色或基于功能的安装”** ，再单击 **“下一步”** 。  

5.  在 **“选择目标服务器”** 页上，单击 **“从服务器池中选择服务器”** ，单击要安装 AD DS 的服务器的名称，再单击 **“下一步”** 。  

6.  在 **“选择服务器角色”** 页上，单击 **“Active Directory 域服务”** ，单击 **“添加功能”** ，再单击 **“下一步”** 。  

7.  在 **“选择功能”** 页上，选择要安装的任何附加功能，再单击 **“下一步”** 。  

8.  在 **“Active Directory 域服务”** 页上，查看信息，再单击 **“下一步”** 。  

9. 在 **“确认安装选择”** 页上，单击 **“安装”** 。  

10. 在 **“结果”** 页上，验证 **“安装成功”** ，再单击 **“将此服务器提升为域服务器”** ，以启动 Active Directory 域服务配置向导。  

    > [!IMPORTANT]  
    > 如果此时关闭添加角色向导且未启动 Active Directory 域服务配置向导，可以单击服务器管理器中的“任务”来重新启动它。  

    （media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks）  

11. 在 **“部署配置”** 页上，单击 **“向现有域添加域控制器”** ，键入域名（例如，emea.contoso.com）和凭据（例如，指定将委派来管理和安装 RODC 的帐户），再单击 **“下一步”** 。  

12. 在 **“域控制器选项”** 页上，单击 **“使用现有 RODC 帐户”** ，键入并确认目录服务还原模式密码，再单击 **“下一步”** 。  

13. 在 **“其他选项”** 页上，如果要从媒体安装，请单击 **“从媒体路径安装”** 类型，验证安装源文件的路径，选择要从中复制 AD DS 安装数据的域控制器（或允许向导选择任何域控制器），再单击 **“下一步”** 。  

14. 在 **“路径”** 页上，键入 Active Directory 数据库、日志文件和 SYSVOL 文件夹的位置（或接受默认位置），再单击 **“下一步”** 。  

15. 在 **“查看选项”** 页上，确认选择，单击 **“查看脚本”** 以将设置导出到 Windows PowerShell 脚本，再单击 **“下一步”** 。  

16. 在 **“先决条件检查”** 页上，确认先决条件验证已完成，再单击 **“安装”** 。  

    若要完成 AD DS 安装，服务器将自动重新启动。  

## <a name="see-also"></a>请参阅  
[域控制器部署疑难解答](Troubleshooting-Domain-Controller-Deployment.md)  
[安装新的 Windows Server 2012 Active Directory 林&#40;级别200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md)  
[安装新的 Windows Server 2012 Active Directory 子或树域&#40;级别200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  
[在现有域&#40;级别200中安装副本 Windows Server 2012 域控制器&#41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  





---
ms.assetid: ac727bd1-a892-47ed-a7ba-439b34187d4e
title: AD DS 安装和删除向导页面说明
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b4df753a1635a0935e70a76278b097d2f9f70142
ms.sourcegitcommit: b190fac4bfa5599751a60d3fc3b4c4a64dd9afd7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2019
ms.locfileid: "66009103"
---
# <a name="ad-ds-installation-and-removal-wizard-page-descriptions"></a>AD DS 安装和删除向导页面说明

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题介绍以下向导页面上在服务器管理器中构成 AD DS 服务器角色安装和删除的控件。  
  
-   [部署配置](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DepConfigPage)  
  
-   [域控制器选项](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage)  
  
-   [DNS 选项](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)  
  
-   [RODC 选项](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RODCOptionsPage)  
  
-   [其他选项](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdditionalOptionsPage)  
  
-   [Paths](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Paths)  
  
-   [准备选项](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdprepCreds)  
  
-   [查看选项](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ViewInstallOptionsPage)  
  
-   [必备项检查](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_PrerqCheckPage)  
  
-   [结果](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Results)  
  
-   [角色删除凭据](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalCredsPage)  
  
-   [AD DS 删除选项和警告](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalOptionsPage)  
  
-   [新的管理员密码](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_NewAdminPwdPage)  
  
-   [确认角色删除选择](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ConfirmRoleRemovalPage)  
  
## <a name="BKMK_DepConfigPage"></a>部署配置  
服务器管理器从“部署配置”页面  开始执行每个域控制器的安装。 其余选项和必填字段在此页面和后续页面上会有所变化，这视所选部署操作而定。 例如，如果创建新的林，**准备选项**页未出现，但是如果它安装在现有林或域中运行 Windows Server 2012 的第一个域控制器。  
  
有些验证测试会在此页面上执行，之后作为先决条件检查的一部分再次执行。 例如，如果尝试在具有 Windows 2000 功能级别的林中安装第一个 Windows Server 2012 域控制器，在此页上将出现错误。  
  
创建新林时，将显示以下选项。  
  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
-   创建新林时，必须为林根域指定名称。 林根域的名称不能为单标记 （例如，它必须是"contoso.com"而不是"contoso"）。 必须使用允许的 DNS 域命名约定。 你可以指定国际化域名 (IDN)。 有关 DNS 域命名约定的详细信息，请参阅 [KB 909264](https://support.microsoft.com/kb/909264)。  
  
-   不要使用与外部 NDS 名称相同的名称创建新 Active Directory 林。 例如，如果您的 Internet DNS URL 为 http: \/ /contoso.com，必须选择其他名称为内部林以避免将来的兼容性问题。 该名称应具有唯一性，且不会产生 Web 流量，比如 corp.contoso.com。  
  
-   在创建新林的服务器上，你必须属于 管理员组 的成员。  
  
有关如何创建一个目录林的详细信息，请参阅[安装新的 Windows Server 2012 Active Directory 林&#40;级别 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md)。  
  
创建新域时，将显示以下选项。  
  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_ChildDomain.gif)  
  
> [!NOTE]  
> 如果创建新树域，需要指定林根域的名称，而不是父域，但其余向导页和选项相同。  
  
-   单击“选择”  以浏览到父域或 Active Directory 树，或键入有效父域或树名称。 然后在“新域名”中键入新域的名称  。  
  
-   树域：提供有效完全限定根域名；该名称无法单标记，且必须使用 DNS 域名要求。  
  
-   子域：提供有效的单标记子域名；该名称必须使用 DNS 域名要求。  
  
-   如果当前凭据不是来自域，Active Directory 域服务配置向导将提示你提供域凭据。 单击“更改”  提供域凭据。  
  
有关如何创建一个域的详细信息，请参阅[安装新的 Windows Server 2012 Active Directory 子域或树域&#40;级别 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)。  
  
将新域控制器添加到现有域时，将显示以下选项。  
  
![AD DS 安装](./media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Replica.gif)  
  
-   单击“选择”  以浏览到该域，或键入有效域名。  
  
-   服务器管理器将根据需要提示你提供有效凭据。 安装其他域控制器需要 Domain Admins 组中的成员身份。  
  
    此外，安装在林中运行 Windows Server 2012 的第一个域控制器需要 Enterprise Admins 和 Schema Admins 组中包括组成员身份的凭据。 如果当前凭据没有足够权限或组成员身份，Active Directory 域服务配置向导将在稍后给出提示。  
  
有关如何将域控制器添加到现有域的详细信息，请参阅[在现有域中安装副本 Windows Server 2012 域控制器&#40;级别 200&#41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)。  
  
## <a name="BKMK_DCOptionsPage"></a>域控制器选项  
如果要创建新林，“域控制器选项”页将显示以下选项。  
  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Forest.gif)  
  
-   默认情况下，林和域功能级别设置为 Windows Server 2012。  
  
    存在提供了一个新功能在 Windows Server 2012 的域功能级别： 支持动态访问控制和 Kerberos 保护 KDC 管理模板策略具有两个设置 （始终提供声明和未保护身份验证失败请求） 需要 Windows Server 2012 域功能级别。 详细信息，请参阅"支持声明、 复合身份验证和 Kerberos 保护"中[什么是 Kerberos 身份验证中的新增功能](https://technet.microsoft.com/library/hh831747.aspx)。    
    Windows Server 2012 林功能级别不提供任何新功能，但它可以确保在林中创建的任何新域将自动运行在 Windows Server 2012 的域功能级别。 Windows Server 2012 域功能级别不会不提供任何其他新功能除了支持动态访问控制和 Kerberos 保护，但它可确保域中的所有域控制器都运行 Windows Server 2012。 有关不同功能级别提供的其他功能的详细信息，请参阅 [了解 Active Directory 域服务 (AD DS) 功能级别](../active-directory-functional-levels.md)。  
  
    超过功能级别时，运行 Windows Server 2012 的域控制器提供了在运行早期版本的 Windows Server 的域控制器不可用的其他功能。 例如，运行 Windows Server 2012 的域控制器可以使用虚拟域控制器克隆，而运行早期版本的 Windows Server 的域控制器不能。  
  
-   创建新林时，默认情况下选择 DNS 服务器。 林中的第一个域控制器必须是全局目录 (GC) 服务器，且不能是只读域控制器 (RODC)。  
  
-   需要目录服务还原模式 (DSRM) 密码才能登录未运行 AD DS 的域控制器。 指定的密码必须遵循应用于服务器的密码策略，且默认情况下无需强密码；仅需非空密码。 总是选择复杂强密码或首选密码。 有关如何将 DSRM 密码与域用户帐户的密码同步的信息，请参阅 [KB 961320](https://support.microsoft.com/kb/961320)。  
  
有关如何创建一个目录林的详细信息，请参阅[安装新的 Windows Server 2012 Active Directory 林&#40;级别 200&#41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md)。  
  
如果要创建子域，“域控制器选项”页将显示以下选项。  
  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Child.gif)  
  
-   默认情况下，域功能级别设置为 Windows Server 2012。 你可以任意指定至少属于林功能级别或更高版本值的其他值。  
  
-   可配置的域控制器选项包括 **“DNS 服务器”** 和 **“全局目录”** ；你不能将只读域控制器配置为新域的第一个域控制器。  
  
    Microsoft 建议所有域控制器都提供 DNS 和全局目录服务，以在分布式环境中实现高可用性，这就是在创建新域时向导默认情况下启用这些选项的原因。  
  
-   “域控制器选项”  页还让你可以从林配置中选择相应的 Active Directory 逻辑“站点名称”  。 默认情况下，将选择具有最合适子网的站点。 如果只有一个站点，将自动选择该站点。  
  
    > [!IMPORTANT]  
    > 如果服务器不属于 Active Directory 子网且存在多个站点，则不选择任何内容且“下一步”  按钮不可用，直至从列表中选择一个站点。  
  
有关如何创建一个域的详细信息，请参阅[安装新的 Windows Server 2012 Active Directory 子域或树域&#40;级别 200&#41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)。  
  
如果要将域控制器添加到域，“域控制器选项”页将显示以下选项。  
  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Replica.gif)  
  
-   可配置的域控制器选项包括“DNS 服务器”  、“全局目录”  和“只读域控制器”  。  
  
    Microsoft 建议所有域控制器都提供 DNS 和全局目录服务，以在分布式环境中实现高可用性，这就是向导默认情况下启用这些选项的原因。 有关部署 RODC 的详细信息，请参阅[只读域控制器计划和部署指南](https://technet.microsoft.com/library/cc771744(v=WS.10).aspx)。  
  
有关如何将域控制器添加到现有域的详细信息，请参阅[在现有域中安装副本 Windows Server 2012 域控制器&#40;级别 200&#41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)。  
  
## <a name="BKMK_DNSOptionsPage"></a>DNS 选项  
如果安装 DNS 服务器，则将显示以下“DNS 选项”  页。  
  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DNSOptions_Replica.gif)  
  
安装 DNS 服务器时，应该在父域名系统 (DNS) 区域中创建指向 DNS 服务器且具有区域权限的委派记录。 委派记录将传输名称解析机构和提供对授权管理新区域的新服务器对其他 DNS 服务器和客户端的正确引用。 这些资源记录包括以下信息：  
  
-   影响委派的名称服务器 (NS) 资源记录。 此资源记录播发以下内容：名为 ns1.na.example.microsoft.com 的服务器对于委派的子域是授权服务器。  
  
-   主机 （A 或 AAAA） 资源记录也称为粘连记录必须存在要解析为其 IP 地址的名称服务器 (NS) 资源记录中指定的服务器的名称。 将此资源记录中的主机名称解析为名称服务器 (NS) 资源记录中的委派 DNS 服务器的过程有时称为“粘连跟踪”。  
  
你可以让 Active Directory 域服务配置向导自动配置它们。 单击“域控制器选项”  页上的“下一步”  后，该向导将验证父 DNS 区域中是否存在相应的记录。 如果该向导无法验证父域中是否存在记录，则该向导将为你提供用于自动创建新域的新 DNS 委派（或更新现有委派）的选项，并继续安装新的域控制器。  
  
此外，还可以先创建这些 DNS 委派记录，再安装 DNS 服务器。 若要创建区域委派，请打开 **“DNS 管理器”** ，右键单击父域，然后单击 **“新建委派”** 。 按照“新建委派向导”中的步骤创建委派。  
  
安装过程尝试创建委派，以确保其他域中的计算机可解析主机的 DNS 查询，包括 DNS 子域中的域控制器和成员计算机。 请注意，只能在 Microsoft DNS 服务器上自动创建委派记录。 如果父 DNS 域区域位于 BIND 等第三方 DNS 服务器上，则将在“先决条件检查”页面上显示无法创建 DNS 委派记录的警告。 有关该警告的详细信息，请参阅[安装 AD DS 的已知问题](https://technet.microsoft.com/library/cc754463(v=WS.10).aspx)。  
  
父域和要提升的子域之间的委派可在安装前后创建和验证。 新域控制器安装出现延迟不是因为无法创建或更新 DNS 委派。  
  
有关委派的详细信息，请参阅[了解区域委派](https://go.microsoft.com/fwlink/?LinkId=164773)(https://go.microsoft.com/fwlink/?LinkId=164773)。 如果在所处情形中无法进行区域委派，可以考虑使用其他方法进行从其他域到你域中主机的名称解析。 例如，其他域的 DNS 管理员可配置条件转发、存根区域或辅助区域来解析你域中的名称。 有关详细信息，请参阅下列主题：  
  
-   [了解区域类型](https://go.microsoft.com/fwlink/?LinkID=157399)(https://go.microsoft.com/fwlink/?LinkID=157399)  
  
-   [了解存根区域](https://go.microsoft.com/fwlink/?LinkId=164776)(https://go.microsoft.com/fwlink/?LinkId=164776)  
  
-   [了解转发器](https://go.microsoft.com/fwlink/?LinkId=164778)(https://go.microsoft.com/fwlink/?LinkId=164778)  
  
## <a name="BKMK_RODCOptionsPage"></a>RODC 选项  
在安装只读域控制器 (RODC) 时，将显示以下选项。  
  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_RODCOptions.gif)  
  
-   委派的管理员帐户获得 RODC 的本地管理权限。 这些用户可以使用等效于本地计算机的 Administrators 组的权限运行。 他们不是 Domain Admins 或域内置管理员组的成员。 在不指派域管理权限的情况下委派分支机构管理时，这样选择很有用。 不需要配置管理委派。 有关详细信息，请参阅[管理员角色分隔](https://technet.microsoft.com/library/cc753170(v=WS.10).aspx)。  
  
-   密码复制策略用作访问控制列表 (ACL)。 它确定是否应该允许 RODC 缓存密码。 在 RODC 收到经身份验证的用户或计算机登录请求时，它将参考密码复制策略来确定是否应该缓存帐户的密码。 那么，同一帐户就可以更高效地执行后续登录。  
  
    密码复制策略 (PRP) 列出了可缓存密码的帐户，以及密码缓存被显式拒绝的帐户。 允许缓存用户和计算机帐户列表，并不表示 RODC 一定缓存了这些帐户的密码。 例如，管理员可以提前指定 RODC 将缓存的任何密码。 这样，RODC 可以对这些帐户进行身份验证，即使集线器站点的 WAN 链接处于脱机状态也是如此。  
  
    不允许（包括隐式）或被拒绝的任何用户或计算机都不会缓存密码。 如果这些用户或计算机不能访问可写域控制器，则无法访问 AD DS-提供的资源或功能。 有关 PRP 的详细信息，请参阅[密码复制策略](https://technet.microsoft.com/library/cc730883(v=ws.10).aspx)。 有关管理 PRP 的详细信息，请参阅[管理密码复制策略](https://technet.microsoft.com/library/rodc-guidance-for-administering-the-password-replication-policy(v=ws.10).aspx)。  
  
有关安装 Rodc 的详细信息，请参阅[安装 Windows Server 2012 Active Directory 只读域控制器&#40;RODC&#41; &#40;级别 200&#41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md)。  
  
## <a name="BKMK_AdditionalOptionsPage"></a>其他选项  
如果要创建新域，以下选项将显示在“其他选项”  页中。  
  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Child.gif)  
  
如果在现有域中安装附加域控制器，以下选项将显示在“其他选项”  页中：  
  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Replica.gif)  
  
-   你可以将域控制器指定为复制源，或允许向导选择任何域控制器作为复制源。  
  
-   你还可以使用“从媒体安装 (IFM)”选项选择使用备份的媒体安装域控制器。 如果在本地存储安装媒体，可以选择“从媒体安装路径”  选项浏览到文件位置。 此浏览选项不适用于远程安装。 你可以单击“验证”  来确保提供的路径是有效媒体。 IFM 选项使用的媒体必须从另一台现有 Windows Server 2012 计算机; 仅创建使用 Windows Server Backup 或 Ntdsutil.exe您不能使用 Windows Server 2008 R2 或以前的操作系统为 Windows Server 2012 域控制器创建媒体。 如果使用 SYSKEY 保护媒体，服务器管理器将在验证期间提示提供映像密码。  
  
有关如何创建一个域的详细信息，请参阅[安装新的 Windows Server 2012 Active Directory 子域或树域&#40;级别 200&#41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)。 有关如何将域控制器添加到现有域的详细信息，请参阅[在现有域中安装副本 Windows Server 2012 域控制器&#40;级别 200&#41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)。  
  
## <a name="BKMK_Paths"></a>Paths  
以下选项显示在“路径”  页。  
  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_Paths.gif)  
  
-   “路径”  页可以用于覆盖 AD DS 数据库、数据库事务日志和 SYSVOL 共享的默认文件夹位置。 默认位置始终位于 %systemroot% 中。  
  
指定 AD DS 数据库 (NTDS.DIT)、日志文件和 SYSVOL 的位置。 对于本地安装，可以浏览到要用于存储文件的位置。  
  
## <a name="BKMK_AdprepCreds"></a>准备选项  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PreparationOptions.gif)  
  
如果当前未使用足够凭据登录来运行 adprep.exe 命令和，且需要运行 adprep 才能完成 AD DS 安装，系统将提示你提供凭据来运行 adprep.exe。 若要添加到现有域或林中运行 Windows Server 2012 的第一个域控制器运行需要 Adprep。 特别需要注意的是：  
  
-   若要添加到现有林运行 Windows Server 2012 的第一个域控制器，必须运行 Adprep /forestprep。 此命令必须由托管架构主机的域的 Enterprise Admins 组、Schema Admins 组和 Domain Admins 组的成员来运行。 若要成功运行此命令，运行命令的计算机和林的架构主机之间必须存在连接。  
  
-   必须运行 Adprep /domainprep 才能将添加到现有域中运行 Windows Server 2012 的第一个域控制器。 此命令必须由其中安装运行 Windows Server 2012 的域控制器的域的 Domain Admins 组的成员运行。 若要成功运行此命令，运行命令的计算机和域的基础结构主机之间必须存在连接。  
  
-   必须运行 Adprep /rodcprep 才能将第一个 RODC 添加到现有林。 此命令必须由 Enterprise Admins 组的成员来运行。 若要成功运行此命令，运行命令的计算机和林中每个应用程序目录分区的基础结构主机之间必须存在连接。  
  
有关 Adprep.exe 的详细信息，请参阅[Adprep.exe 集成](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_NewAdprep)，并查看[运行 Adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx)。  
  
## <a name="BKMK_ViewInstallOptionsPage"></a>查看选项  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_ReviewOptions.gif)  
  
-   “审查” 选项  页可以用于验证设置并确保在开始安装前满足要求。 这不是停止使用服务器管理器安装的最后一次机会。 此页只是让你先查看和确认设置，然后再继续配置。  
  
-   服务器管理器中的“审查选项”  页还提供可选的“查看脚本”  按钮，可将包含当前 ADDSDeployment 配置的 Unicode 文本文件创建为一个 Windows PowerShell 脚本。 这样，你可以将服务器管理器图形界面用作 Windows PowerShell 部署工作台。 使用 Active Directory 域服务配置向导来配置选项、导出配置和取消向导。 此过程将创建有效且语法正确的示例，以便做进一步修改或直接使用。  
  
## <a name="BKMK_PrerqCheckPage"></a>必备项检查  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PrerequisitesCheck.gif)  
  
此页面上显示的一些警告包括：  
  
-   运行 Windows Server 2008 或更高版本已建立安全通道会话时，可以防止较弱加密算法的默认设置为"允许加密算法兼容 Windows NT 4"的域控制器。 有关潜在影响和解决方法的详细信息，请参阅 KB 文章 [942564](https://support.microsoft.com/kb/942564)。  
  
-   无法创建或更新 DNS 委派。 有关详细信息，请参阅 [DNS 选项](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)。  
  
-   先决条件检查需要 WMI 调用。 如果调用被防火墙规则阻止，则无法执行调用，并返回一个 RPC 服务器不可用错误。  
  
有关 AD DS 安装需要执行的先决条件检查的详细信息，请参阅[先决条件测试](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_ADDSInstallPrerequisiteTests)。  
  
## <a name="BKMK_Results"></a>结果  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_SMResultsBeta.gif)  
  
在此页面上，可以查看安装结果。  
  
你还可以选择在向导完成后重新启动目标服务器。但是，如果安装成功，则不管是否选择该选项，服务器都将始终重新启动。 在某些情况下，如果在安装前未加入域的目标服务器上完成向导，目标服务器的系统状态可能导致无法在网络上访问服务器，或者系统状态可能阻止拥有权限来管理远程服务器。  
  
如果目标服务器无法在这种情况下重新启动，必须手动重新启动。 shutdown.exe 或 Windows PowerShell 等工具无法重新启动它。 你可以使用远程桌面服务登录并远程关闭目标服务器。  
  
## <a name="BKMK_RemovalCredsPage"></a>角色删除凭据  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Credentials.gif)  
  
你可在“凭据”  页上配置降级选项。 从以下列表提供执行降级所需的凭据：  
  
-   降级其他域控制器需要 Domain Admin 凭据。 选择**强制删除域控制器**将降级域控制器，而不从 Active Directory 删除域控制器对象的元数据。  
  
    > [!IMPORTANT]  
    > 如果域控制器可以联系其他域控制器，则不要选择此选项，而且*还没有任何合理的方法*可解决这种网络问题。 强制降级会将 Active Directory 中已丢弃的元数据保留在林中的其余域控制器上。 此外，该域控制器上所有未复制的更改（如密码或新用户帐户）都将永久丢失。 已丢弃的元数据是 AD DS、Exchange、SQL 和其他软件的大部分 Microsoft 客户支持案例的根本原因。 如果强制降级域控制器，*必须*立即手动执行元数据清理。 有关步骤，请查看 [清理服务器元数据](https://technet.microsoft.com/library/cc816907(WS.10).aspx)。  
  
-   降级域中的最后一个域控制器需要 Enterprise Admins 组成员身份，因为这将删除域本身（如果这是林中的最后一个域，这将删除林）。 服务器管理器将通知当前域控制器是否是域中的最后一个域控制器。 选择“域中的最后一个域控制器”  以确认域控制器是域中的最后一个域控制器。  
  
有关删除 AD DS 的详细信息，请参阅[删除 Active Directory 域服务 (级别 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c)并[降级域控制器和域&#40;级别 200&#41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md)。  
  
## <a name="BKMK_RemovalOptionsPage"></a>AD DS 删除选项和警告  
如果需要“审查” 选项页的相关帮助，请参阅“审查” 选项。  
  
如果域控制器托管附加角色（如 DNS 服务器角色或全局目录服务器），将显示以下警告消息：  
  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Warnings.gif)  
  
必须单击“继续删除”  确认附加角色不再提供，才能单击“下一步”  继续。  
  
如果强制删除域控制器，未复制到域中其他域控制器的任何 Active Directory 对象更改都将丢失。 此外，如果域控制器托管操作主机角色、全局目录或 DNS 服务器角色，可能会给域和林中的关键操作带来如下影响。 在删除托管任何操作主机角色的域控制器之前，尝试将角色传输到其他域控制器。 如果不能将此角色转移，首先从这台计算机中删除 Active Directory 域服务，，然后使用 Ntdsutil.exe 占用角色。 在计划占用角色的域控制器上使用 Ntdsutil；如果可能，使用与此域控制器相同的站点中的最新复制伙伴。 有关传输和占用操作主机角色的详细信息，请参阅 Microsoft 知识库文章 [255504](https://go.microsoft.com/fwlink/?LinkId=80395)。 如果向导无法确定域控制器是否托管操作主机角色，请运行 netdom.exe 命令以确定此域控制器是否执行任何操作主机角色。  
  
-   全局目录：用户可能无法登录到林中的域。 在删除全局目录服务器之前，确保此林和站点具有足够全局目录服务器来服务用户登录。 如有必要，指定其他全局目录服务器并使用新信息更新客户端和应用程序。  
  
-   DNS 服务器：所有存储在 Active Directory 集成区域的 DNS 数据都将丢失。 删除 AD DS 后，此 DNS 服务器将不能执行集成到 Active Directory 的 DNS 区域的名称解析。 因此，建议更新当前引用此 DNS 服务器的 IP 地址的所有计算机的 DNS 配置，以使用新 DNS 服务器的 IP 地址进行名称解析。  
  
-   基础结构主机：域中的客户端可能无法找到其他域中的对象。 在继续之前，先将基础结构主机角色传输到非全局目录服务器的域控制器上。  
  
-   RID 主机：可能无法创建新用户帐户、计算机帐户和安全组。 在继续之前，先将 RID 主机角色传输到与此域控制器位于同个域的域控制器。  
  
-   主域控制器 (PDC) 仿真器：PDC 仿真器执行的操作（如组策略更新和非 AD DS 帐户的密码重设）无法正常运行。 在继续之前，先将 PDC 仿真器主机角色传输到与此域控制器位于同个域的域控制器。  
  
-   架构主机：不再能够修改此林的架构。 在继续之前，先将架构主机角色传输到林根域中的域控制器。  
  
-   域命名主机：不再能够将域添加到此林或从此林删除域。 在继续之前，先将域命名主机角色传输到林根域中的域控制器。  
  
-   此 Active Directory 域控制器上的所有应用程序目录分区都将删除。 如果域控制器保存一个或多个应用程序目录分区的最后一个副本，则在删除操作完成时，这些分区将不再存在。  
  
请注意，在从域中的最后一个域控制器卸载 Active Directory 域服务后，域将不再存在。  
  
如果域控制器是委派来托管 DNS 区域的 DNS 服务器，以下页将提供选项来从 DNS 区域委派中删除 DNS 服务器。  
  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_RemovalOptions.gif)  
  
有关删除 AD DS 的详细信息，请参阅[删除 Active Directory 域服务 (级别 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c)并[降级域控制器和域&#40;级别 200&#41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md)。  
  
## <a name="BKMK_NewAdminPwdPage"></a>新的管理员密码  
**新的管理员密码**页要求您降级完成且计算机成为域成员服务器或工作组计算机后，为内置本地计算机的管理员帐户提供密码。  
  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_NewAdminPwd.gif)  
  
有关删除 AD DS 的详细信息，请参阅[删除 Active Directory 域服务 (级别 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c)并[降级域控制器和域&#40;级别 200&#41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md)。  
  
## <a name="BKMK_ConfirmRoleRemovalPage"></a>查看选项  
“审查” 选项  页面可以将降级配置设置导出到 Windows PowerShell 脚本，以便可以自动执行附加降级。 单击“降级”  以删除 AD DS。  
  
![AD DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_ReviewOptions.gif)  
  



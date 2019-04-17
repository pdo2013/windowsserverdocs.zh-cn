---
ms.assetid: ac727bd1-a892-47ed-a7ba-439b34187d4e
title: "广告 DS 安装和删除向导页面说明"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fa023398822e79ca8c3e93d44bb1e87fc9190cee
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-installation-and-removal-wizard-page-descriptions"></a>广告 DS 安装和删除向导页面说明

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题提供的控件，在以下向导页面中包含的广告 DS server 角色安装和删除服务器管理器中的说明。  
  
-   [部署配置](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DepConfigPage)  
  
-   [域控制器选项](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage)  
  
-   [DNS 选项](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)  
  
-   [RODC 选项](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RODCOptionsPage)  
  
-   [其他选项](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdditionalOptionsPage)  
  
-   [路径](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Paths)  
  
-   [准备选项](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdprepCreds)  
  
-   [查看选项](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ViewInstallOptionsPage)  
  
-   [先决条件复选](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_PrerqCheckPage)  
  
-   [结果](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Results)  
  
-   [角色删除凭据](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalCredsPage)  
  
-   [广告 DS 删除选项和警告](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalOptionsPage)  
  
-   [新的管理员密码](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_NewAdminPwdPage)  
  
-   [确认角色删除选择](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ConfirmRoleRemovalPage)  
  
## <a name="BKMK_DepConfigPage"></a>部署配置  
服务器管理器开始与每次域控制器安装**部署配置**页面。 其他选项和必填的字段在此页和后续页面，具体取决于哪个部署操作你选择的更改。 例如，如果你创建新的林**准备选项**页面未显示，但如果你安装了第一个现有森林或域中运行 Windows Server 2012 的域控制器那样。  
  
在此页上，并且稍后再的先决条件检查一部分执行一些验证测试。 例如，如果你尝试安装有 Windows 2000 功能级别森林中的第一个 Windows Server 2012 域控制器，将在此页面上出现错误。  
  
创建新的林时，将显示以下选项。  
  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
-   当你创建新林时，你必须指定森林根域的名称。 森林根域名无法单标记 （例如，必须是"contoso.com"，而不是"contoso"）。 它必须使用允许的 DNS 域命名惯例。 你可以指定国际域名名称 (IDN)。 有关 DNS 域命名惯例的详细信息，请参阅[KB 909264](https://support.microsoft.com/kb/909264)。  
  
-   不要使用与您外部 DNS 具有相同名称中创建新 Active Directory 森林。 例如，如果你的 Internet DNS URL http://contoso.com，你必须选择另一个名称内部林以避免未来兼容性问题。 该名称应该唯一和 web 通信，例如 corp.contoso.com 确信。  
  
-   你必须是管理员组你想要创建新的林服务器上的成员。  
  
有关如何创建林的详细信息，请参阅[安装新 Windows Server 2012 Active Directory 森林 & #40;级别 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
当你创建一个新的域，将显示以下选项。  
  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_ChildDomain.gif)  
  
> [!NOTE]  
> 如果你创建一个新的树域，若要指定的名称森林根域，而不是家长域中，你需要但剩余向导页面和选项都相同。  
  
-   单击**选择**家长域的 Active Directory 树中，浏览或键入有效家长域或树名称。 然后键入名称中的新域**新域名**。  
  
-   树域： 提供有效的完整根域名;该名称将不能单标记，并且必须使用 DNS 域名称要求。  
  
-   孩子域： 提供有效的、 单标签孩子域名;该名称必须使用 DNS 域名称要求。  
  
-   Active Directory 域服务配置向导会提示您输入凭据域如果你当前的凭据无法从域。 单击**更改**提供域的凭据。  
  
有关如何创建域的详细信息，请参阅[安装新 Windows Server 2012 Active Directory 孩子或树域 & #40;级别 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
当你添加到现有的域的新的域控制器，将显示以下选项。  
  
![广告 DS 安装](./media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Replica.gif)  
  
-   单击**选择**要浏览到域，或者键入有效的域名。  
  
-   服务器管理器会提示你输入有效的凭据根据需要。 安装额外的域控制器要求域管理员组中的成员。  
  
    此外，安装的树林中运行 Windows Server 2012 的第一个域控制器要求包含的企业管理员和方案管理员组中的组成员的凭据。 Active Directory 域服务配置向导你稍后当提示你当前的凭据没有足够的权限或组成员。  
  
有关如何添加到现有的域的域控制器的详细信息，请参阅[现有域 & #40; 在安装副本 Windows Server 2012 域控制器级别 200 & #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_DCOptionsPage"></a>域控制器选项  
要创建新林，如果域控制器选项页面具有以下选项：  
  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Forest.gif)  
  
-   森林和域的功能级别默认情况下设置与 Windows Server 2012。  
  
    没有一项新功能可在 Windows Server 2012 域功能级别： 支持动态访问控制和保护作用 KDC 管理模板策略 Kerberos 有两个设置 （始终提供索赔和失败 unarmored 身份验证请求） 需要 Windows Server 2012 域功能级别。 有关详细信息，在中看到"支持索赔、 复合身份验证和 Kerberos 程度" [Kerberos 身份验证中的新增](https://technet.microsoft.com/library/hh831747.aspx)。    
    Windows Server 2012 森林功能级别不提供任何新的功能，但它确保创建林中任何新域将自动能在 Windows Server 2012 域功能级别。 Windows Server 2012 域功能级别不提供任何新旁对动态访问控制和保护作用 Kerberos，支持其他功能，但它可以确保任何域控制器在域中的运行 Windows Server 2012。 有关可在不同的功能级别其他功能的详细信息，请参阅[了解 Active Directory 域服务 (广告 DS) 功能级别](../active-directory-functional-levels.md)。  
  
    超出功能级别，运行 Windows Server 2012 的域控制器提供其他功能的运行较早版本的 Windows Server 的域控制器上不可用。 例如，运行 Windows Server 2012 的域控制器可以用于虚拟域控制器复制，而不能运行较早版本的 Windows Server 的域控制器。  
  
-   默认情况下，当你创建新林，DNS 服务器处于选中状态。 森林中的第一个域控制器必须全球目录 (GC) 服务器，并且无法读取仅域控制器 (RODC)。  
  
-   目录服务还原模式 (DSRM) 密码登录到其中广告 DS 未运行的域控制器才能需要。 指定你的密码必须遵守密码策略应用到服务器，默认情况下它不需要使用强密码;仅非空密码。 始终可以选择复杂的强密码或最好，密码。 有关如何使用域用户帐户的密码同步 DSRM 密码的信息，请参阅[KB 961320](https://support.microsoft.com/kb/961320)。  
  
有关如何创建林的详细信息，请参阅[安装新 Windows Server 2012 Active Directory 森林 & #40;级别 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
如果你在创建孩子域，域控制器选项页面具有以下选项：  
  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Child.gif)  
  
-   默认情况下，域功能级别设置与 Windows Server 2012。 你可以指定其他至少森林功能级别或更高版本的值值。  
  
-   配置域控制器选项包括**DNS 服务器**和**全球目录**;你无法将仅阅读域控制器配置为新的域中的第一个域控制器。  
  
    Microsoft 建议所有域控制器都提供 DNS 和分布式环境，这正是向导默认情况下使这些选项时创建一个新的域, 的高可用性全球目录服务。  
  
-   **域控制器选项**页面，还可以选择相应的 Active Directory 逻辑**站点名称**从森林配置。 默认情况下，选择最正确子网与该站点。 如果只有一个网站，它将自动选择该站点。  
  
    > [!IMPORTANT]  
    > 如果该服务器不属于 Active Directory 网，并且多个站点，选中任何和**下一步**按钮不可用，直到你选择站点从列表。  
  
有关如何创建域的详细信息，请参阅[安装新 Windows Server 2012 Active Directory 孩子或树域 & #40;级别 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
如果你要添加到某个域域控制器，域控制器选项页面具有以下选项：  
  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Replica.gif)  
  
-   配置域控制器选项包括**DNS 服务器**和**全球目录**，并**只读域控制器**。  
  
    Microsoft 建议所有域控制器都提供 DNS 和实现分布式环境，该向导将默认情况下使这些选项的高可用性全球目录服务。 有关部署 Rodc 的详细信息，请参阅[只读域控制器计划和部署指南](https://technet.microsoft.com/library/cc771744(v=WS.10).aspx)。  
  
有关如何添加到现有的域的域控制器的详细信息，请参阅[现有域 & #40; 在安装副本 Windows Server 2012 域控制器级别 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_DNSOptionsPage"></a>DNS 选项  
如果你安装了 DNS 服务器，以下**DNS 选项**页面显示时：  
  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DNSOptions_Replica.gif)  
  
当你安装了 DNS 服务器时，应父域名系统 (DNS) 的区域创建为权威的区域 DNS 服务器更改为委派记录。 委派记录传输名称分辨率机构，并提供对其他 DNS 服务器和客户端进行为新的区域权威的新服务器正确引用。 这些资源记录如下所示：  
  
-   影响委派名称 (NS) 服务器资源记录。 此资源记录公布名为 ns1.na.example.microsoft.com 服务器是委派子域权威服务器。  
  
-   主机（A 或 AAAA）资源记录也称为将结合记录必须来解决服务器名称 (NS) 服务器资源记录为其 IP 地址中指定的名称。 解决该记录的主机名委派名称 (NS) 服务器资源记录中的 DNS 服务器的进程有时称为"粘贴跟踪。"  
  
你可以让 Active Directory 域服务配置向导自动创建它们。 向导将验证后在你单击相应的记录存在家长 DNS 区域中**下一步**上**域控制器选项**页面。 如果向导无法验证记录存在家长域中，该向导将自动为你提供了创建新 DNS 委派新域 （或更新现有委派） 的选项，并继续新的域控制器安装。  
  
或者，你可以创建这些 DNS 委派记录之前安装 DNS 服务器。 若要创建区域委派，打开**DNS 管理**，家长域中，右键单击，然后单击**新委派**。 按照创建委派新委派向导中的步骤。  
  
在安装过程尝试创建委派以确保计算机其他域中的，可以主机，其中包括域控制器以及成员计算机中 DNS 子域解决 DNS 查询的问题。 请注意，可以仅在 Microsoft DNS 服务器上自动创建委派记录。 如果家长 DNS 域区域驻留在第三方 DNS 服务器，如绑定，先决条件复选页面上将显示一个警告，但无法创建 DNS 委派记录有关。 有关警告的详细信息，请参阅[安装广告 DS 版的已知问题](https://technet.microsoft.com/library/cc754463(v=WS.10).aspx)。  
  
可以创建和验证安装之前或之后委派家长域和所升级子域之间。 没有任何原因，因为你无法创建或更新 DNS 委派延迟的一个新的域控制器安装。  
  
有关委派的详细信息，请参阅[了解区域委派](https://go.microsoft.com/fwlink/?LinkId=164773)(https://go.microsoft.com/fwlink/?LinkId=164773)。 如果区域委派不可能在你的情况下，你可能会考虑提供名称分辨率来自其他域到主机域中的其他方法。 例如的另一个域 DNS 管理员可以配置条件转发、 桩模块区域或辅助区域以解决你的域中的名称。 有关详细信息，请参阅以下主题：  
  
-   [了解区域类型](https://go.microsoft.com/fwlink/?LinkID=157399)(https://go.microsoft.com/fwlink/?LinkID=157399)  
  
-   [了解桩模块区域](https://go.microsoft.com/fwlink/?LinkId=164776)(https://go.microsoft.com/fwlink/?LinkId=164776)  
  
-   [了解转发器](https://go.microsoft.com/fwlink/?LinkId=164778)(https://go.microsoft.com/fwlink/?LinkId=164778)  
  
## <a name="BKMK_RODCOptionsPage"></a>RODC 选项  
当你安装只读域控制器 (RODC)，将显示以下选项。  
  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_RODCOptions.gif)  
  
-   委派的管理员帐户获得 RODC 到本地管理权限。 这些用户可以使用本地计算机管理员组等效权限操作。 他们并不域管理员或域内置管理员组中的成员。 此选项可用于委派分支机构管理不提供了域管理权限。 配置管理委派不是必需的。 有关详细信息，请参阅[管理员角色分离](https://technet.microsoft.com/library/cc753170(v=WS.10).aspx)。  
  
-   密码复制策略充当访问控制 (ACL) 列表。 它可以确定是否应允许 RODC 缓存密码。 RODC 收到身份验证的用户或计算机登录请求后，它是指密码复制策略，以确定是否应该缓存帐户的密码。 相同的帐户可以更高效地执行后续登录。  
  
    密码复制策略 (PRP) 列出允许其密码被缓存、 帐户和帐户被缓存显式拒绝其密码。 获准缓存的用户和计算机帐户列表并不意味着 RODC 具有一定缓存的这些帐户的密码。 例如，也管理员可以指定提前 RODC 将缓存的任何帐户。 这样一来，RODC 可以进行身份验证这些帐户，即使 WAN 链接到中心网站处于离线状态。  
  
    任何用户或计算机那些不允许 （包括隐式） 或拒绝执行操作不缓存他们的密码。 如果这些用户或计算机没有访问可写的域控制器，他们无法访问广告提供 DS 资源或功能。 有关 PRP 的详细信息，请参阅[密码复制策略](https://technet.microsoft.com/library/cc730883(v=ws.10).aspx)。 有关管理 PRP 的详细信息，请参阅[管理密码复制策略](https://technet.microsoft.com/library/rodc-guidance-for-administering-the-password-replication-policy(v=ws.10).aspx)。  
  
有关安装 Rodc 的详细信息，请参阅[安装 Windows Server 2012 Active Directory 只读域控制器 & #40;RODC & #41;& #40;级别 200 & #41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md).  
  
## <a name="BKMK_AdditionalOptionsPage"></a>其他选项  
以下选项将显示在**其他选项**页面，如果你要创建新的域：  
  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Child.gif)  
  
以下选项将显示在**其他选项**页面，如果你安装额外的域控制器了现有域中：  
  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Replica.gif)  
  
-   你可以指定为复制源，域控制器，或让选择复制源作为任何域控制器向导。  
  
-   你还可以选择要域控制器使用安装媒体使用安装媒体 (IFM) 选项从备份。 如果安装媒体存储在本地，**从媒体路径安装**选项允许你浏览到文件位置。 浏览选项不可用于远程安装。 你可以单击**验证**以确保所提供的路径是有效的媒体。 IFM 选项通过使用媒体必须从另一台现有 Windows Server 2012 计算机仅; 创建与 Windows Server 备份或 Ntdsutil.exe你无法使用 Windows Server 2008 R2 或以前的操作系统创建媒体的 Windows Server 2012 域控制器。 如果 SYSKEY 受保护的媒体，服务器管理器在验证过程提示提供该图像的密码。  
  
有关如何创建域的详细信息，请参阅[安装新 Windows Server 2012 Active Directory 孩子或树域 & #40;级别 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md). 有关如何添加到现有的域的域控制器的详细信息，请参阅[现有域 & #40; 在安装副本 Windows Server 2012 域控制器级别 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_Paths"></a>路径  
以下选项将显示在**路径**页面。  
  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_Paths.gif)  
  
-   **路径**页使你可以覆盖默认文件夹位置的广告 DS 数据库数据库事务日志中，并 SYSVOL 共享。 始终处于 %系统根 %默认位置。  
  
指定的位置广告 DS 数据库 (NTDS。DIT)、 文件和 SYSVOL 登录。 对于本地安装，可以浏览到存储文件位置。  
  
## <a name="BKMK_AdprepCreds"></a>准备选项  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PreparationOptions.gif)  
  
如果你当前不登录以足够身份运行 adprep.exe 命令，为了完成广告 DS 安装运行所需的 adprep，你会提示您提供运行 adprep.exe 的凭据。 Adprep 需要添加到现有域或森林运行 Windows Server 2012 的第一个域控制器才能运行。 具体而言：  
  
-   若要添加到现有的树林中运行 Windows Server 2012 的第一个域控制器，必须运行 Adprep /forestprep 成功。 必须通过的成员企业管理员组、 架构管理员组中，并且承载方案主机的域管理员组运行该命令。 要成功完成，此命令必须有连接计算机运行命令和林的方案主之间。  
  
-   必须运行 Adprep /domainprep 添加到现有的域中运行 Windows Server 2012 的第一个域控制器。 必须运行 Windows Server 2012 的域控制器安装了域域管理员组成员运行该命令。 要成功完成，此命令必须有连接计算机运行命令和域的基础结构主之间。  
  
-   必须运行 Adprep /rodcprep 以添加到现有的森林的第一个 RODC。 必须通过企业管理员组成员的运行该命令。 要成功完成，此命令必须有连接计算机运行命令和森林中的每个应用目录分区的基础结构母版之间。  
  
有关 Adprep.exe 的详细信息，请参阅[Adprep.exe 集成](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_NewAdprep)并查看[运行 Adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx)。  
  
## <a name="BKMK_ViewInstallOptionsPage"></a>查看选项  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_ReviewOptions.gif)  
  
-   **回顾选项**页面，可以让你以验证你的设置并确保它们符合你的要求，开始安装之前。 这不是最后一次机会停止安装使用服务器管理器。 此页只可以进行检查，配置在继续之前，请确认你的设置。  
  
-   **回顾选项**页面中服务器管理器中还提供了一个可选**查看脚本**按钮来创建 Unicode 文本文件包含当前 ADDSDeployment 配置为单个 Windows PowerShell 脚本。 这使你为 Windows PowerShell 部署 studio 使用服务器管理器图形界面。 使用 Active Directory 域服务配置向导配置选项中, 导出配置，然后取消向导。 此过程中创建进一步修改或直接使用有效和语法正确的示例。  
  
## <a name="BKMK_PrerqCheckPage"></a>先决条件复选  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PrerequisitesCheck.gif)  
  
在此页面显示警告包括：  
  
-   运行 Windows Server 2008 或更高版本具有默认设置的"允许加密算法兼容使用 Windows NT 4"阻止较弱加密算法建立安全通道会话时，域控制器。 有关可能产生影响和解决方法，请参阅知识库文章更多信息[942564](https://support.microsoft.com/kb/942564)。  
  
-   DNS 委派无法创建或更新。 有关详细信息，请参阅[DNS 选项](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)。  
  
-   先决条件检查要求 WMI 呼叫。 如果已阻止的防火墙规则块，和退还 RPC 服务器不可用错误，它们可能会失败。  
  
有关广告 DS 安装用于执行特定的先决条件检查的详细信息，请参阅[先决条件测试](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_ADDSInstallPrerequisiteTests)。  
  
## <a name="BKMK_Results"></a>结果  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_SMResultsBeta.gif)  
  
在此页上，你可以查看安装的结果。  
  
你还可以选择重启目标服务器后向导完毕时，但安装成功，如果该服务器始终会重新启动无论你是否选择该选项。 在某些情况下在未加入域之前安装, 的目标服务器完成向导后，系统状态目标服务器的可以进行服务器无法连接到网络上，或系统状态可以阻止你从具有权限来管理远程服务器。  
  
如果目标服务器无法在此情况下重启，则必须手动重新启动它。 如 shutdown.exe 或 Windows PowerShell 的工具无法重新启动它。 远程桌面服务可用于登录并远程关机目标服务器。  
  
## <a name="BKMK_RemovalCredsPage"></a>角色删除凭据  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Credentials.gif)  
  
你在配置降级选项**凭据**页面。 提供以下列表中执行降级所需的凭据：  
  
-   降级额外的域控制器要求域管理员凭据。 选择**强制删除域控制器**将域控制器降级而不删除 Active Directory 对象域控制器元数据。  
  
    > [!IMPORTANT]  
    > 不要选择此选项，除非域控制器无法联系其他域控制器，并且没有*合理无法*来解决该网络的问题。 强制的降级森林中的其余域控制器上保留 Active Directory 孤立元数据。 此外，所有已取消复制的更改在该域控制器，例如密码或新的用户帐户都将丢失永远停止营业。 孤立元数据是很大比例的 Microsoft 客户支持的情况下在广告 DS、 Exchange、 SQL，和其他软件的根本原因。 如果你强行降级域控制器，您*必须*手动立即执行清理元数据。 对于步骤，查看[干净向上服务器元数据](https://technet.microsoft.com/library/cc816907(WS.10).aspx)。  
  
-   降级域中的最后一个域控制器需要企业管理员组成员资格，因为这会删除域本身 （如果这是森林中的最后一个域，这将删除森林）。 服务器管理器会通知你是否当前的域控制器在域中的最后一个域控制器。 选择**域中的最后一个域控制器**以确认域控制器是在域中的最后一个域控制器。  
  
删除广告 DS 的详细信息，请参阅[删除 Active Directory 域服务 (级别 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c)和[降级域控制器和域 & #40;级别 200 & #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_RemovalOptionsPage"></a>广告 DS 删除选项和警告  
如果你需要在审阅选项页面的帮助，请参阅回顾选项  
  
如果域控制器主持其他角色，如 DNS 服务器角色或全球目录服务器上，将显示以下警告页面：  
  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Warnings.gif)  
  
必须单击**继续删除**以确认你可以单击之前，不会再将提供其他角色**下一步**以继续。  
  
如果你将强制域控制器删除，没有复制到其他域控制器在域中的任何 Active Directory 对象更改都将丢失。 此外，如果域控制器主持操作主机角色、 全球目录或 DNS 服务器角色，域和森林中的关键操作可能会受到影响，如下所示。 删除主持域控制器上任何操作的主机角色之前，请尝试转移到另一个域控制器角色。 如果不能转移角色，首先从这台计算机，删除 Active Directory 域服务，然后使用 Ntdsutil.exe 占用角色。 在你打算占用到; 角色域控制器上使用 Ntdsutil如果可能，请用作此域控制器在同一站点最近复制伙伴。 有关传输和占用操作主角色的详细信息，请参阅[文章 255504](https://go.microsoft.com/fwlink/?LinkId=80395) Microsoft 知识库文章中。 如果该向导不确定是否域控制器主机操作主机角色，，运行 netdom.exe 命令以确定是否该域控制器执行任何操作主机角色。  
  
-   全球目录： 用户可能会很到树林中的域的问题日志记录。 全球目录服务器中删除之前，确保足够全球目录服务器此林和站点上，以便用户登录的服务中。 如有必要，指定另一台全球目录服务器和更新的客户端和应用程序使用新的信息。  
  
-   DNS 服务器： DNS 数据存储在 Active Directory 集成的区域中的所有都将丢失。 删除广告 DS 后，该 DNS 服务器不能对 DNS 区域的 Active Directory 集成执行名称分辨率。 因此，我们建议你更新的当前，请参考此 DNS 服务器名称分辨率的 IP 地址，新的 DNS 服务器的 IP 地址的所有计算机 DNS 配置。  
  
-   基础主机： 在域中的客户端可能很难其他域中查找对象。 在继续之前，将不是一个全球目录服务器域控制器传输主基础角色。  
  
-   删除母版： 你可能会有创建新的用户帐户，计算机帐户安全组的问题。 在继续之前，将 RID 主机角色传输到域控制器在该域控制器相同的域中。  
  
-   主域控制器 (PDC) 仿真器： 正在执行的操作 PDC 模拟器，诸如组策略更新，无广告 DS 帐户，重置密码将无法正常工作。 在继续之前，将在相同的域此域控制器域控制器传输 PDC 模拟器主角色。  
  
-   方案母版： 你将不再能够修改这片森林的方案。 在继续之前，传输到域控制器树林中根域中的方案主机角色。  
  
-   域名主机： 你将不再能够域中添加或删除域这片森林。 在继续之前，传输域名主到域控制器在根域森林中的角色。  
  
-   将删除 Active Directory 该域控制器上的所有应用程序 directory 分区。 如果删除操作完成后，域控制器拥有一个或多个应用目录分区的最后一个副本，将不会再存在这些分区。  
  
请注意域后您从上次域控制器在域中卸载 Active Directory 域服务将不会再存在。  
  
如果域控制器委派主持 DNS 区域 DNS 服务器，以下页面将提供选项从 DNS 区域委派删除 DNS 服务器。  
  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_RemovalOptions.gif)  
  
删除广告 DS 的详细信息，请参阅[删除 Active Directory 域服务 (级别 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c)和[降级域控制器和域 & #40;级别 200 & #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_NewAdminPwdPage"></a>新的管理员密码  
**新管理员密码**页面要求你提供密码内置本地计算机管理员帐户，降级完毕后计算机在某个域成员服务器或工作组计算机。  
  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_NewAdminPwd.gif)  
  
删除广告 DS 的详细信息，请参阅[删除 Active Directory 域服务 (级别 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c)和[降级域控制器和域 & #40;级别 200 & #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_ConfirmRoleRemovalPage"></a>查看选项  
**回顾选项**页面提供给 Windows PowerShell 脚本导出降级配置设置，以便你可以自动其他降级有机会。 单击**降级**删除广告 DS。  
  
![广告 DS 安装](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_ReviewOptions.gif)  
  



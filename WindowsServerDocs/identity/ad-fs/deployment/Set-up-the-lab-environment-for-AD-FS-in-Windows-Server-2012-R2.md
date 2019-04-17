---
ms.assetid: 6b38480e-5b1c-49f0-9d46-8cf22f70f0d2
title: "设置适用于 Windows Server 2012 R2 中广告 FS 实验环境"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 049a1a1b0a419b0194edfe56b356a9f1e8b4b058
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="set-up-the-lab-environment-for-ad-fs-in-windows-server-2012-r2"></a>设置适用于 Windows Server 2012 R2 中广告 FS 实验环境

>适用于： Windows Server 2012 R2

本主题概述配置测试环境，可用于完成演练以下演练指南中的步骤：

-   [工作区参与 iOS 设备演练：](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

-   [演练：与 Windows 设备的工作区加入](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)


-   [演练指南：管理具有条件访问控制的风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)

-   [演练指南：管理敏感应用程序的其他多重身份验证的风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

> [!NOTE]
> 不建议你安装的 web 服务器和联盟服务器同一台计算机上。

若要设置此测试环境，完成以下步骤：

1.  [第 1 步： 将域控制器 (DC1) 配置](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_1)

2.  [第 2 步： 与设备注册服务配置联合服务器 (ADFS1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)

3.  [第 3 步： 配置 web 服务器 (WebServ1) 和一个示例基于声明的应用程序](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_5)

4.  [第 4 步： 将配置客户端计算机 (客户端 1)](../../ad-fs/deployment/../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_10)

## <a name="BKMK_1"></a>第 1 步： 将域控制器 (DC1) 配置
出于此测试环境中，你可以进行呼叫你根 Active Directory 域**contoso.com**和指定** pass@word1 **作为管理员密码。

-   安装广告 DS 角色服务并安装 Active Directory 域服务 (广告 DS)，以在 Windows Server 2012 R2 域控制器使你的计算机。 此操作升级你的广告 DS 架构域控制器创建的一部分。 有关详细信息和的分步说明，请参阅[https://technet.microsoft.com/library/hh472162.aspx](https://technet.microsoft.com/library/hh472162.aspx)。

### <a name="BKMK_2"></a>创建测试 Active Directory 帐户
域控制器可正常运行后，可以在此域中创建一个测试组和测试用户帐户和到组帐户添加的用户帐户。 你可以使用这些帐户以完成演练早些时候在本主题中引用演练指南中。

创建以下帐户：

-   用户： **Robert Hatley**具有以下凭据： User name: **RobertH**和密码：**P@ssword**

-   组：**财经**

有关如何创建 Active Directory (AD) 中的用户和组帐户的信息，请参阅[https://technet.microsoft.com/library/cc783323%28v.aspx](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx)。

添加**Robert Hatley**到帐户**财经**组。 如何添加到某个组中的 Active Directory 的用户的信息，请参阅[https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx](https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx)。

### <a name="create-a-gmsa-account"></a>创建 GMSA 帐户
组托管服务帐户 (GMSA) 帐户的 Active Directory 联合身份验证服务 (广告 FS) 安装和配置过程是必需的。

##### <a name="to-create-a-gmsa-account"></a>若要创建 GMSA 帐户

1.  打开 Windows PowerShell 命令窗口中，键入：

    ```
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    New-ADServiceAccount FsGmsa -DNSHostName adfs1.contoso.com -ServicePrincipalNames http/adfs1.contoso.com

    ```

## <a name="BKMK_4"></a>第 2 步： 通过使用设备注册服务配置联合服务器 (ADFS1)
若要设置另一台虚拟机，安装 Windows Server 2012 R2，并将其连接到域**contoso.com**。设置计算机后，你已连接到域，它，然后继续安装和配置广告 FS 角色。

有关视频，请参阅[Active Directory 联合身份验证服务操作视频系列： 安装广告 FS 服务器电场的日落](https://technet.microsoft.com/video/dn469436)。

### <a name="install-a-server-ssl-certificate"></a>安装服务器 SSL 证书
在本地计算机官方商城的 ADFS1 服务器上必须安装服务器安全套接字层 (SSL) 证书。 证书必须具有以下属性：

-   主题名称 (CN): adfs1.contoso.com

-   主题替代名称 (DNS): adfs1.contoso.com

-   主题替代名称 (DNS): enterpriseregistration.contoso.com

有关设置 SSL 证书的详细信息，请参阅[配置 SSL 月 TLS 在域中的网站上的企业 CA](https://social.technet.microsoft.com/wiki/contents/articles/12485.configure-ssltls-on-a-web-site-in-the-domain-with-an-enterprise-ca.aspx)。

[Active Directory 联合身份验证服务视频的操作方法系列： 更新证书](https://technet.microsoft.com/video/adfs-updating-certificates)。

### <a name="install-the-ad-fs-server-role"></a>安装广告 FS 服务器角色

##### <a name="to-install-the-federation-service-role-service"></a>若要安装联合身份验证服务角色服务

1.  登录到使用域管理员帐户服务器administrator@contoso.com。

2.  开始服务器管理器。 若要开始服务器管理器，请单击**服务器管理器**在 Windows**开始**屏幕上，或单击**服务器管理器**桌面上的 Windows 在 Windows 任务栏上。 在**快速启动**的选项卡**欢迎**磁贴上**仪表板**页上，单击**添加角色和功能**。 或者，你可以单击**添加角色和功能**上**管理**菜单。

3.  在**在开始之前**页上，单击**下一步**。

4.  在**选择安装类型**页上，单击**角色基于或功能的安装**，然后单击**下一步**。

5.  在**选择目标服务器**页上，单击**从服务器池选择服务器**，确认目标计算机选择，然后单击**下一步**。

6.  在**选择服务器角色**页上，单击**Active Directory 联合身份验证服务**，然后单击**下一步**。

7.  在**选择功能**页上，单击**下一步**。

8.  在**Active Directory 联合身份验证服务 (广告 FS)**页上，单击**下一步**。

9. 请验证信息，请在后**确认安装选择**页上，选择**必要时自动重新启动目标服务器**复选框，然后依次单击**安装**。

10. 在**安装进度**页面，验证一切正常，安装，然后单击**关闭**。

### <a name="configure-the-federation-server"></a>配置联盟服务器
下一步是配置联合身份验证的服务器。

##### <a name="to-configure-the-federation-server"></a>若要配置联盟服务器

1.  在服务器管理器中**仪表板**页上，单击**通知**标记，，然后单击**配置服务器上的联合身份验证服务**。

    **Active Directory 联合身份验证服务配置向导**打开。

2.  在**欢迎**页上，选择**联合身份验证的服务器场中创建的第一个联盟服务器**，然后单击**下一步**。

3.  在**连接到广告 DS**页面上，用域管理员权限指定帐户**contoso.com**这台计算机加入的 Active Directory 域，然后单击**下一步**。

4.  在**指定的服务属性**页上，执行以下操作，然后依次单击**下一步**:

    -   导入你之前已经购买了 SSL 证书。 此证书已所需的服务身份验证证书。 浏览到 SSL 证书的位置。

    -   若要提供联合身份验证服务的名称，请键入**adfs1.contoso.com**。此值为相同时你登记 SSL 证书 Active Directory 证书服务（广告客户服务）中的提供的值。

    -   若要提供联合身份验证服务的显示名称，请键入**Contoso Corporation**。

5.  在**指定的服务帐户**页上，选择**使用现有的域的用户帐户或组托管服务帐户**，然后指定 GMSA 帐户**fsgmsa**时创建了域控制器创建。

6.  在**指定配置数据库**页上，选择**上使用 Windows 内部数据库此服务器创建数据库**，然后单击**下一步**。

7.  在**回顾选项**页面，验证你的配置选择，然后单击**下一步**。

8.  在**先决条件检查**页上，确保所有先决条件检查成功完成，然后单击**配置**。

9. 在**结果**页上，检查结果，检查是否已成功完成配置，然后单击**所需的完成联合身份验证服务部署后续步骤**。

### <a name="configure-device-registration-service"></a>配置设备注册服务
下一步是配置设备注册服务 ADFS1 服务器上。 有关视频，请参阅[Active Directory 联合身份验证服务操作视频系列： 启用设备注册服务](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service)。

##### <a name="to-configure-device-registration-service-for-windows-server-2012-rtm"></a>若要配置设备注册服务的 Windows Server 2012 RTM

1.  > [!IMPORTANT]
    > **下面的步骤适用于 Windows Server 2012 R2 RTM 的版本中。**

    打开 Windows PowerShell 命令窗口中，键入：

    ```
    Initialize-ADDeviceRegistration
    ```

    在系统提示服务帐户时，键入**contoso\fsgmsa$**。

    现在，Windows PowerShell cmdlet 运行。

    ```
    Enable-AdfsDeviceRegistration
    ```

2.  在 ADFS1 服务器上，在**广告 FS 管理**控制台中，导航到**认证策略**。 选择**编辑全球主要身份验证**。 选择旁边的复选框**启用设备身份验证**，然后单击**确定**。

### <a name="add-host-a-and-alias-cname-resource-records-to-dns"></a>对 DNS 添加主机 (A) 和的资源记录 (CNAME) 别名
在 DC1 上，你必须确保以下域名系统 (DNS) 记录创建用于设备注册服务。

|输入|键入|地址|
|---------|--------|-----------|
|adfs1|主机 (A)|广告 FS 服务器 IP 地址|
|enterpriseregistration|别名 (CNAME)|adfs1.contoso.com|

你可以使用下面的过程中添加到公司 DNS 名称服务器联合身份验证的服务器和设备注册服务主机 (A) 资源记录。

管理员组或等效会员是才能完成此过程的最低要求。 查看有关使用相应的帐户和超链接"https://go.microsoft.com/fwlink/?LinkId=83477"本地和域默认组 (https://go.microsoft.com/fwlink/p/?LinkId=83477) 中的组成员的详细信息。

##### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>对 DNS 添加主机 (A) 和的资源记录 (CNAME) 别名联盟服务器

1.  在 DC1 上，从服务器管理器中，在**工具**菜单上，单击**DNS**以打开 DNS。

2.  控制台树中，依次展开 DC1、**前进查找区域**，右键单击**contoso.com**，然后单击**（A 或 AAAA） 的新主机**。

3.  在**名称、**键入你想要用于广告 FS 场的名称。 此演练中，键入**adfs1**。

4.  在**IP 地址**，键入 IP 地址的 ADFS1 服务器。 单击**添加主机**。

5.  右键单击**contoso.com**，然后单击**新别名 (CNAME)**。

6.  在**新资源记录**对话框中，键入**enterpriseregistration**中**别名**框。

7.  在完全合格 (的域名 FQDN) 目标主机框中，键入**adfs1.contoso.com**，然后单击**确定**。

    > [!IMPORTANT]
    > 在现实世界部署，如果你的公司所拥有多个用户主要名称 (UPN) 后缀，必须创建多个 CNAME 记录的这些 UPN 后缀 DNS 在每个一个。

## <a name="BKMK_5"></a>第 3 步： 配置 web 服务器 (WebServ1) 和一个示例基于声明的应用程序
安装 Windows Server 2012 R2 操作系统设置虚拟机 (WebServ1) 并将其连接到域**contoso.com**。已加入域后，你可以继续进行安装和配置的 Web 服务器角色。

若要完成之前本主题中所引用的演练，你必须受联盟服务器 (ADFS1) 的示例应用。

你可以下载 Windows 身份基础 SDK ([https://www.microsoft.com/download/details.aspx?id=4451](https://www.microsoft.com/download/details.aspx?id=4451)，其中包括一个示例基于声明的应用程序。

你必须完成以下步骤来设置具有此示例基于声明的应用程序的 web 服务器。

> [!NOTE]
> 已通过以下步骤测试运行 Windows Server 2012 R2 的操作系统的 web 服务器上。

1.  [安装的 Web 服务器角色和 Windows 身份基础](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_15)

2.  [安装 Windows SDK 的身份基础](../../ad-fs/deployment/../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_13)

3.  [在 IIS 配置简单的索赔应用](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_9)

4.  [创建您联合身份验证的服务器上信赖的方信任](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_11)

### <a name="BKMK_15"></a>安装的 Web 服务器的角色和 Windows 身份基础

1.  > [!NOTE]
    > 你必须具有对 Windows Server 2012 R2 的安装媒体的访问权限。

    登录 WebServ1 使用** administrator@contoso.com **和密码** pass@word1 **。

2.  从服务器管理器中，在**快速启动**的选项卡**欢迎**磁贴上**仪表板**页上，单击**添加角色和功能**。 或者，你可以单击**添加角色和功能**上**管理**菜单。

3.  在**在开始之前**页上，单击**下一步**。

4.  在**选择安装类型**页上，单击**角色基于或功能的安装**，然后单击**下一步**。

5.  在**选择目标服务器**页上，单击**从服务器池选择服务器**，确认目标计算机选择，然后单击**下一步**。

6.  在**选择服务器角色**页上，选择旁边的复选框**Web 服务器 (IIS)**，单击**添加功能**，然后单击**下一步**。

7.  在**选择功能**页上，选择**Windows 身份基础 3.5**，然后单击**下一步**。

8.  在**Web 服务器角色 (IIS)**页上，单击**下一步**。

9. 在**选择角色服务**页上，选择，展开**应用程序开发**。 选择**ASP.NET 3.5**，单击**添加功能**，然后单击**下一步**。

10. 在**确认安装选择**页上，单击**指定备用源代码路径**。 Windows Server 2012 R2 的安装媒体位于 Sxs 目录输入的路径。 有关示例 D:\Sources\Sxs。 单击**确定**，然后单击**安装**。

### <a name="BKMK_13"></a>安装 Windows SDK 的身份基础

1.  运行 WindowsIdentityFoundation-SDK-3.5.msi 安装 Windows 的身份基础 SDK 3.5 (https://www.microsoft.com/download/details.aspx?id=4451)。 选择所有默认选项。

### <a name="BKMK_9"></a>在 IIS 配置简单的索赔应用

1.  在计算机证书应用商店中安装有效 SSL 证书。 证书应包含您的 web 服务器的名称**webserv1.contoso.com**。

2.  复制到 C:\Inetpub\Claimapp C:\Program 文件 (x86) \Windows Identity Foundation SDK\v3.5\Samples\Quick Start\Web Application\PassiveRedirectBasedClaimsAwareWebApp 的内容。

3.  编辑**Default.aspx.cs**文件，以便没有索赔筛选发生。 执行此步骤以确保示例应用程序，显示由联盟服务器发出的所有声明。 请执行以下操作：

    1.  打开**Default.aspx.cs**文本编辑器中。

    2.  第二个文件中搜索`ExpectedClaims`。

    3.  查看整个注释`IF`声明和其大括号。 通过键入指示评论"月月"（不带引号） 在一行的起始处。

    4.  你`FOREACH`声明现在应该类似于此代码示例。

        ```
        Foreach (claim claim in claimsIdentity.Claims)
        {
           //Before showing the claims validate that this is an expected claim
           //If it is not in the expected claims list then don't show it
           //if (ExpectedClaims.Contains( claim.ClaimType ) )
           // {
              writeClaim( claim, table );
           //}
        }

        ```

    5.  保存并关闭**Default.aspx.cs**。

    6.  打开**web.config**文本编辑器中。

    7.  删除整个`<microsoft.identityModel>`部分。 删除所有内容从开始`including <microsoft.identityModel>`达和包括`</microsoft.identityModel>`。

    8.  保存并关闭**web.config**。

4.  **配置 IIS 管理器**

    1.  打开**Internet 信息服务 (IIS) 管理器**。

    2.  转到**应用程序池**，右键单击**池中**选择**高级设置**。 设置**加载的用户配置文件**到**如此**，然后单击**确定**。

    3.  右键单击**池中**选择**基本设置**。 更改**.NET CLR 版本**到**.NET CLR 版本 v2.0.50727**。

    4.  右键单击**默认网站**选择**编辑绑定**。

    5.  添加**HTTPS**绑定到端口**443**已安装 SSL 证书。

    6.  右键单击**默认网站**选择**添加的应用程序**。

    7.  设置为别名**claimapp**和物理路径**c:\inetpub\claimapp**。

5.  若要配置**claimapp**处理您联合身份验证的服务器，请执行以下：

    1.  运行 FedUtil.exe，位于**C:\Program 文件 (x86) \Windows Identity Foundation SDK\v3.5**。

    2.  将应用程序的配置位置设置为**C:\inetput\claimapp\web.config**和设置为你的站点的 URL 的应用程序 URI **https://webserv1.contoso.com /claimapp/**。 单击**下一步**。

    3.  选择**使用现有的 STS**并浏览到你的广告 FS 服务器元数据 URL **https://adfs1.contoso.com/federationmetadata/2007-06/federationmetadata.xml**。 单击**下一步**。

    4.  选择**禁用证书链验证**，然后单击**下一步**。

    5.  选择**不加密**，然后单击**下一步**。 在**提供索赔**页上，单击**下一步**。

    6.  选择旁边的复选框**计划任务执行日常 WS 联盟元数据更新**。 单击**完成**。

    7.  现已配置应用程序的示例。 如果测试应用程序 URL **https://webserv1.contoso.com/claimapp**，它应该将你重定向到您联合身份验证的服务器。 联合身份验证的服务器应显示错误页面，这是因为尚未配置信赖的方信任。 换言之，你拥有不安全广告 FS 此测试应用程序。

现在，你必须安全示例应用程序的 web 服务器对广告 FS 上运行。 你可以添加信赖的方信任上联盟服务器 (ADFS1) 来执行此操作。 有关视频，请参阅[Active Directory 联合身份验证服务操作视频系列： 添加依赖方信任](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust)。

### <a name="BKMK_11"></a>创建您联合身份验证的服务器上信赖的方信任

1.  你联盟服务器上 (ADFS1)，在**广告 FS 管理控制台**，导航到**信赖方信任**，然后单击**添加依赖方信任**。

2.  上**选择数据源**页上，选择**导入信赖方有关的数据发布在线或本地网络上**，输入的元数据 URL **claimapp**，然后单击**下一步**。 运行 FedUtil.exe 创建元数据.xml 文件。 它位于**https://webserv1.contoso.com/claimapp/federationmetadata/2007-06/federationmetadata.xml**。

3.  上**指定显示名称**页面上，指定**显示名称**你依赖的方信任**claimapp**，然后单击**下一步**。

4.  在**配置现在多重身份验证？**页上，选择**不想指定此信赖的方信任多重身份验证设置，这次**，然后单击**下一步**。

5.  在**选择颁发授权规则**页上，选择**允许所有用户访问此依赖方**，然后单击**下一步**。

6.  在**添加信任准备已就绪**页上，单击**下一步**。

7.  在**编辑索赔规则**对话框中，单击**添加规则**。

8.  在**选择规则类型**页上，选择**发送索赔使用自定义规则**，然后单击**下一步**。

9. 在**配置索赔规则**页上，在**声明规则名称**框中，键入**所有索赔**。 在**自定义规则**框中，键入以下索赔规则。

    ```
    c:[ ]
    => issue(claim = c);

    ```

10. 单击**完成**，然后单击**确定**。

## <a name="BKMK_10"></a>第 4 步： 将配置客户端计算机 (客户端 1)
设置其他虚拟机和安装 Windows 8.1。 此虚拟机必须是同一个虚拟网络其他计算机上。 不应已到 Contoso 域加入此计算机。

客户必须信任用于设置中的联合服务器 (ADFS1)，SSL 证书[第 2 步： 与设备注册服务配置联合服务器 (ADFS1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)。 它还必须无法验证的证书证书吊销信息。

您还必须设置并使用 Microsoft 帐户登录到客户端 1。

## <a name="see-also"></a>请参阅


- [Active Directory 联合身份验证服务操作视频系列： 安装广告 FS 服务器电场的日落](https://technet.microsoft.com/video/dn469436)
- [Active Directory 联合身份验证服务视频的操作方法系列： 更新证书](https://technet.microsoft.com/video/adfs-updating-certificates)
- [Active Directory 联合身份验证服务操作视频系列： 添加信赖的方信任](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust)
- [Active Directory 联合身份验证服务视频的操作方法系列： 启用设备注册服务](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service)
- [Active Directory 联合身份验证服务视频的操作方法系列： 安装 Web 应用程序代理服务器](https://technet.microsoft.com/video/dn469438)




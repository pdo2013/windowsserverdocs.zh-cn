---
ms.assetid: 6b38480e-5b1c-49f0-9d46-8cf22f70f0d2
title: 为 Windows Server 2012 R2 中的 AD FS 设置实验室环境
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 52199ab8ca6f82443e78e72c6980746fa561363a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408309"
---
# <a name="set-up-the-lab-environment-for-ad-fs-in-windows-server-2012-r2"></a>为 Windows Server 2012 R2 中的 AD FS 设置实验室环境


本主题概述了配置测试环境的步骤，该测试环境可用于在以下操作实例指南中完成演练：

-   [演练：使用 iOS 设备加入工作区](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

-   [演练：使用 Windows 设备加入工作区](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)


-   [操作实例指南：使用条件访问控制管理风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)

-   [操作实例指南：利用适用于敏感应用程序的附加多重身份验证管理风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

> [!NOTE]
> 我们不建议你将 Web 服务器和联合服务器安装在同一计算机上。

若要设置此测试环境，完成以下步骤：

1.  [步骤 1：配置域控制器（DC1）](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_1)

2.  [步骤 2：用设备注册服务配置联合服务器（ADFS1）](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)

3.  [步骤 3：配置 web 服务器（WebServ1）和基于声明的示例应用程序](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_5)

4.  [步骤 4：配置客户端计算机（Client1）](../../ad-fs/deployment/../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_10)

## <a name="BKMK_1"></a>步骤1：配置域控制器 (DC1)
在此测试环境中，可以调用根 Active Directory 域**contoso.com** ，并指定<strong>pass@word1</strong>作为管理员密码。

-   安装 AD DS 角色服务并安装 Active Directory 域服务（AD DS）以使你的计算机成为 Windows Server 2012 R2 中的域控制器。 此操作会在创建域控制器的过程中升级你的 AD DS 架构。 有关详细信息和分步说明，请参阅[https://technet.microsoft.com/library/hh472162.aspx](https://technet.microsoft.com/library/hh472162.aspx)。

### <a name="BKMK_2"></a>创建测试 Active Directory 帐户
在你的域控制器起作用后，可以在此域中创建一个测试组和测试用户帐户，并将该用户帐户添加到组帐户。 你可以使用这些帐户来完成本主题前面部分中引用的操作实例指南中的演练。

创建以下帐户：

- 用户：**Robert Hatley** 具有以下凭据：用户名：**RobertH**和密码：<strong>P@ssword</strong>

- 组：**额**

有关如何在 Active Directory （AD）中创建用户帐户和组帐户的信息， [https://technet.microsoft.com/library/cc783323%28v.aspx](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx)请参阅。

将 **Robert Hatley** 帐户添加到 **Finance** 组。 有关如何向 Active Directory 中的组添加用户的信息，请参阅[https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx](https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx)。

### <a name="create-a-gmsa-account"></a>创建 GMSA 帐户
在 Active Directory 联合身份验证服务（AD FS）安装和配置过程中需要组托管服务帐户（GMSA）帐户。

##### <a name="to-create-a-gmsa-account"></a>创建 GMSA 帐户

1.  打开 Windows PowerShell 命令窗口并键入：

    ```
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    New-ADServiceAccount FsGmsa -DNSHostName adfs1.contoso.com -ServicePrincipalNames http/adfs1.contoso.com

    ```

## <a name="BKMK_4"></a>步骤2：通过使用设备注册服务配置联合服务器 (ADFS1)
若要设置另一台虚拟机，请安装 Windows Server 2012 R2 并将其连接到域**contoso.com**。 在将计算机加入域后设置计算机，然后继续安装和配置 AD FS 角色。

有关视频，请参阅[Active Directory 联合身份验证服务操作方法视频系列：安装 AD FS 服务器场](https://technet.microsoft.com/video/dn469436)。

### <a name="install-a-server-ssl-certificate"></a>安装服务器 SSL 证书
必须在本地计算机存储中的 ADFS1 服务器上安装服务器安全套接字层 (SSL) 证书。 该证书必须具有以下属性：

-   使用者名称 (CN)：adfs1.contoso.com

-   使用者备用名称 (DNS)：adfs1.contoso.com

-   使用者备用名称 (DNS)：enterpriseregistration.contoso.com

有关设置 SSL 证书的详细信息，请参阅 [使用企业 CA 在域中的网站上配置 SSL/TLS](https://social.technet.microsoft.com/wiki/contents/articles/12485.configure-ssltls-on-a-web-site-in-the-domain-with-an-enterprise-ca.aspx)。

[Active Directory 联合身份验证服务操作方法视频系列：更新证书](https://technet.microsoft.com/video/adfs-updating-certificates)。

### <a name="install-the-ad-fs-server-role"></a>安装 AD FS 服务器角色

##### <a name="to-install-the-federation-service-role-service"></a>安装联合身份验证服务角色服务

1. 使用域管理员帐户administrator@contoso.com登录到服务器。

2. 启动“服务器管理器”。 若要启动“服务器管理器”，请在 Windows“开始” 屏幕上单击“服务器管理器” ，或在 Windows 桌面上的 Windows 任务栏中单击“服务器管理器” 。 在“仪表板” 页面上的“欢迎” 磁贴的“快速启动” 选项卡中，单击“添加角色和功能”。 或者，也可以在“管理”菜单中单击“添加角色和功能”。

3. 在“开始之前” 页上，单击“下一步”。

4. 在 **“选择安装类型”** 页面上，单击 **“基于角色或基于功能的安装”** ，然后单击 **“下一步”** 。

5. 在“选择目标服务器”页面上，单击“从服务器池中选择一个服务器”，验证目标计算机是否已选中，然后单击“下一步”。

6. 在“选择服务器角色” 页面上，单击“Active Directory 联合身份验证服务”，然后单击“下一步”。

7. 在“选择功能”页上，单击“下一步”。

8. 在“Active Directory 联合身份验证服务 (AD FS)” 页面上，单击“下一步”。

9. 在验证“确认安装选择” 页面上的信息后，选中“必要时自动重新启动目标服务器” 复选框，然后单击“安装”。

10. 在“安装进度”页面上，验证所有内容是否正确安装，然后单击“关闭”。

### <a name="configure-the-federation-server"></a>配置联合服务器
下一步是配置联合服务器。

##### <a name="to-configure-the-federation-server"></a>配置联合服务器

1.  在服务器管理器“仪表板”页面上，单击“通知”标志，然后单击“在服务器上配置联合身份验证服务”。

    打开“Active Directory 联合身份验证服务配置向导” 。

2.  在“欢迎使用”页面上，选择“在联合服务器场中创建第一个联合服务器”，然后单击“下一步”。

3.  在“连接到 AD DS”页面上，为此计算机加入的 **contoso.com** Active Directory 域指定具有域管理员权限的帐户，然后单击“下一步”。

4.  在“指定服务属性” 页面上，执行以下操作，然后再单击“下一步”：

    -   导入之前获得的 SSL 证书。 此证书是所需的服务身份验证证书。 浏览到你的 SSL 证书的位置。

    -   若要为你的联合身份验证服务提供名称，请键入 **adfs1.contoso.com**。 此值是注册 SSL 证书时在 Active Directory 证书服务 (AD CS) 中提供的相同的值。

    -   若要为你的联合身份验证服务提供显示名称，请键入 **Contoso Corporation**。

5.  在“指定服务帐户” 页面上，选择“使用现有的域用户帐户或组托管服务帐户”，然后指定在你创建域控制器后创建的 GMSA 帐户 **fsgmsa** 。

6.  在“指定配置数据库” 页面上，选择“使用 Windows 内部数据库在此服务器上创建数据库”，然后单击“下一步”。

7.  在“查看选项”页面上，验证你的配置选择，然后单击“下一步”。

8.  在“先决条件检查” 页面上，验证所有先决条件检查已成功完成，然后单击“配置”。

9. 在“结果” 页面上、查看结果、检查是否已成功完成配置，然后单击“完成联合身份验证服务部署所需的后续步骤”。

### <a name="configure-device-registration-service"></a>配置设备注册服务
下一步是配置 ADFS1 服务器上的设备注册服务。 有关视频，请参阅[Active Directory 联合身份验证服务操作方法视频系列：正在启用设备注册服务](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service)。

##### <a name="to-configure-device-registration-service-for-windows-server-2012-rtm"></a>配置 Windows Server 2012 RTM 的设备注册服务

1.  > [!IMPORTANT]
    > **以下步骤适用于 Windows Server 2012 R2 RTM 版本。**

    打开 Windows PowerShell 命令窗口并键入：

    ```
    Initialize-ADDeviceRegistration
    ```

    当系统提示你输入服务帐户时，键入 **contoso\fsgmsa$** 。

    现在，运行 Windows PowerShell cmdlet。

    ```
    Enable-AdfsDeviceRegistration
    ```

2.  在 ADFS1 服务器上，在“AD FS 管理”控制台中，导航到“身份验证策略”。 选择“编辑全局主要身份验证”。 选中“启用设备身份验证”旁边的复选框，然后单击“确定”。

### <a name="add-host-a-and-alias-cname-resource-records-to-dns"></a>将主机 (A) 和别名 (CNAME) 资源记录添加到 DNS
在 DC1 上，你必须确保为设备注册服务创建以下域名系统 (DNS)。

|条目|type|地址|
|---------|--------|-----------|
|adfs1|主机 (A)|AD FS 服务器的 IP 地址|
|enterpriseregistration|别名 (CNAME)|adfs1.contoso.com|

你可以使用以下过程为联合服务器和设备注册服务将主机 (A) 资源记录添加到公司 DNS 名称服务器。

管理员组中的成员身份或等效身份是完成此过程所需的最低要求。 查看有关如何使用相应帐户和超链接 "<https://go.microsoft.com/fwlink/?LinkId=83477>本地和域默认组（<https://go.microsoft.com/fwlink/p/?LinkId=83477>）中的组成员身份的详细信息。

##### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>为联合服务器将主机 (A) 和别名 (CNAME) 资源记录添加到 DNS

1.  在 DC1 上，从“服务器管理器”中，在“工具” 菜单上，单击“DNS” 以打开 DNS 管理单元。

2.  在控制台树中，依次展开 DC1、“正向查找区域”，右键单击“contoso.com”，然后单击“新建主机 （A 或 AAAA）”。

3.  在“名称” 中，键入你希望用于 AD FS 场的名称。 对于此操作实例，则键入 **adfs1**。

4.  在“IP 地址”中，键入 ADFS1 服务器的 IP 地址。 单击“添加主机”。

5.  右键单击“contoso.com”，然后单击“新别名 (CNAME)”。

6.  在“新资源记录”对话框中，在“别名”框内键入 **enterpriseregistration**。

7.  在目标主机框的完全限定域名 (FQDN) 中，键入“adfs1.contoso.com”，然后单击“确定”。

    > [!IMPORTANT]
    > 在现实世界部署中，如果你的公司有多个用户主体名称 (UPN) 后缀，则必须创建多个 CNAME 记录，每个记录可用于那些在 DNS 中的 UPN 后缀。

## <a name="BKMK_5"></a>步骤3：配置 Web 服务器 (WebServ1） 和一个基于声明的应用程序示例
通过安装 Windows Server 2012 R2 操作系统并将其连接到域**contoso.com**来设置虚拟机（WebServ1）。 在加入域后，你可以继续安装和配置 Web 服务器角色。

若要完成本主题前面部分中所引用的实例操作，你必须具备由联合服务器 (ADFS1) 保护的示例应用程序。

您可以下载 Windows Identity Foundation SDK （[https://www.microsoft.com/download/details.aspx?id=4451](https://www.microsoft.com/download/details.aspx?id=4451)，其中包含基于声明的示例应用程序。

你必须完成以下步骤以使用该基于声明的应用程序示例设置 Web 服务器。

> [!NOTE]
> 这些步骤已在运行 Windows Server 2012 R2 操作系统的 web 服务器上进行了测试。

1.  [安装 Web 服务器角色和 Windows Identity Foundation](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_15)

2.  [安装 Windows Identity Foundation SDK](../../ad-fs/deployment/../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_13)

3.  [在 IIS 中配置简单声明应用](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_9)

4.  [在联合服务器上创建信赖方信任](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_11)

### <a name="BKMK_15"></a>安装 Web 服务器角色和 Windows Identity Foundation

1. > [!NOTE]
   > 你必须具有对 Windows Server 2012 R2 安装媒体的访问权限。

   使用<strong>administrator@contoso.com</strong>和密码<strong>pass@word1</strong>登录到 WebServ1。

2. 从服务器管理器中，在“仪表板”页面上的“欢迎”磁贴的“快速启动”选项卡上，单击“添加角色和功能”。 或者，也可以在“管理”菜单中单击“添加角色和功能”。

3. 在“开始之前” 页上，单击“下一步”。

4. 在 **“选择安装类型”** 页面上，单击 **“基于角色或基于功能的安装”** ，然后单击 **“下一步”** 。

5. 在“选择目标服务器”页面上，单击“从服务器池中选择一个服务器”，验证目标计算机是否已选中，然后单击“下一步”。

6. 在“选择服务器角色” 页面上，选中“Web 服务器 (IIS)”旁边的复选框，单击“添加功能”，然后单击“下一步”。

7. 在“选择功能”页面上，选中“Windows Identity Foundation 3.5”，然后单击“下一步”。

8. 在“Web 服务器角色 (IIS)” 页面上，单击“下一步”。

9. 在“选择角色服务”页面上，选中并展开“应用程序开发”。 选中“ASP.NET 3.5”，单击“添加功能”，然后单击“下一步”。

10. 在“确认安装选择” 页面上，单击“指定备用源路径”。 输入位于 Windows Server 2012 R2 安装媒体中的 Sxs 目录的路径。 例如，D:\Sources\Sxs。 单击“确定”，再单击“安装”。

### <a name="BKMK_13"></a>安装 Windows Identity Foundation SDK

1.  运行 Windowsidentityfoundation-sdk-3.5.msi 以安装 Windows Identity Foundation SDK 3.5 （ https://www.microsoft.com/download/details.aspx?id=4451) 。 选择所有默认选项。

### <a name="BKMK_9"></a>在 IIS 中配置简单声明应用

1.  在计算机证书存储中安装有效的 SSL 证书。 该证书应包含你的 Web 服务器的名称 **webserv1.contoso.com**。

2.  将 C:\Program Files (x86) \Windows Identity Foundation SDK\v3.5\Samples\Quick Start\Web Application\PassiveRedirectBasedClaimsAwareWebApp 的内容复制到 C:\Inetpub\Claimapp。

3.  编辑 **Default.aspx.cs** 文件，以便不发生声明过滤。 执行此步骤以确保示例应用程序显示由联合服务器颁发的所有声明。 请执行以下操作：

    1.  在文本编辑器中打开 **Default.aspx.cs** 。

    2.  在 `ExpectedClaims`的第二个实例中搜索文件。

    3.  注释掉整个 `IF` 语句及其左大括号。 通过在行的开头键入 "//" （不包含引号）来指示注释。

    4.  你的 `FOREACH` 语句现在看起来应像此代码示例所示。

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

    5.  保存并关闭 **Default.aspx.cs**。

    6.  在文本编辑器中打开“web.config” 。

    7.  删除整个`<microsoft.identityModel>`部分。 从 `including <microsoft.identityModel>` 开始删除包括 `</microsoft.identityModel>`在内的所有内容。

    8.  保存并关闭“web.config”。

4.  **配置 IIS 管理器**

    1.  打开“Internet 信息服务 (IIS) 管理器”。

    2.  转到“应用程序池”，右键单击“DefaultAppPool” 以选中“高级设置”。 将“加载用户配置文件”设置为“True”，然后单击“确定”。

    3.  右键单击“DefaultAppPool” 以选中“基本设置”。 将“.NET CLR 版本”更改到 “.NET CLR 版本 v2.0.50727”。

    4.  右键单击“默认网站” 以选中“编辑绑定”。

    5.  使用你已安装的 SSL 证书将“HTTPS” 绑定添加到端口“443” 。

    6.  右键单击“默认网站”以选中“添加应用程序”。

    7.  将别名设置为 **claimapp** 并将物理路径设置为 **c:\inetpub\claimapp**。

5.  若要将 **claimapp** 配置为与你的联合服务器一起使用，请执行以下内容：

    1.  运行 FedUtil.exe（它位于 **C:\Program Files (x86)\Windows Identity Foundation SDK\v3.5**）。

    2.  将应用程序配置位置设置为**C:\inetput\claimapp\web.config** ，并将 "应用程序 URI" 设置为站点的 URL， **https://webserv1.contoso.com /claimapp/** 。 单击“下一步”。

    3.  选择 "**使用现有 STS** " 并浏览到 AD FS 服务器的元数据 URL **https://adfs1.contoso.com/federationmetadata/2007-06/federationmetadata.xml** 。 单击“下一步”。

    4.  选中“禁用证书链验证”，然后单击“下一步”。

    5.  选中“不加密”，然后单击“下一步”。 在“提供的声明” 页面上，单击“下一步”。

    6.  选中“计划任务以执行每日 WS 联合身份验证元数据更新”旁边的复选框。 单击 **“完成”** 。

    7.  现在配置你的示例应用程序。 如果测试应用程序 URL **https://webserv1.contoso.com/claimapp** ，它应将你重定向到联合服务器。 联合服务器应显示一个错误页面，因为你尚未配置信赖方信任。 换句话说，你没有 AD FS 保护此测试应用程序。

你现在必须通过 AD FS 保护在 web 服务器上运行的示例应用程序。 可以通过在你的联合服务器 (ADFS1) 上添加信赖方信任执行此操作。 有关视频，请参阅[Active Directory 联合身份验证服务操作方法视频系列：添加信赖方信任](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust)。

### <a name="BKMK_11"></a>在联合服务器上创建信赖方信任

1.  在你的联合服务器 (ADFS1) 上，在“AD FS 管理控制台”中，导航到“信赖方信任”，然后单击“添加信赖方信任”。

2.  在“选择数据源” 页面上，选中“导入有关信赖方联机或在本地网络上发布的数据”，为 **claimapp**输入元数据 URL，然后单击“下一步”。 运行 FedUtil.exe 创建了元数据 .xml 文件。 它位于 **https://webserv1.contoso.com/claimapp/federationmetadata/2007-06/federationmetadata.xml** 。

3.  在“指定显示名称” 页面上，为你的信赖方信任指定“显示名称” ， **claimapp**，然后单击“下一步”。

4.  在“现在配置多重身份验证吗？” 页面上，选中“此时我不想为此信赖方信任指定多重身份验证设置”，然后单击“下一步”。

5.  在“选择颁发授权规则”页面上，选中“允许所有用户访问此信赖方”，然后单击“下一步”。

6.  在“准备好添加信任”页面上，单击“下一步”。

7.  在“编辑声明规则”对话框上，单击“添加规则”。

8.  在“选择规则类型” 页面上，选中“使用自定义规则发送声明”，然后单击“下一步”。

9. 在“配置声明规则”页面上，在“声明规则名称”框中，键入 **All Claims**。 在“自定义规则”框中，键入以下声明规则。

    ```
    c:[ ]
    => issue(claim = c);

    ```

10. 单击“完成”，然后单击“确定”。

## <a name="BKMK_10"></a>步骤4：配置客户端计算机 (Client1)
设置另一台虚拟机并安装 Windows 8.1。 此虚拟机必须与其他虚拟机在相同的虚拟网络上。 此虚拟机不应加入 Contoso 域。

客户端必须信任用于联合服务器（ADFS1）的 SSL 证书（你在[步骤2：用设备注册服务](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)配置联合服务器（ADFS1）。 它还必须能够验证该证书的吊销信息。

此外，你还必须设置和使用 Microsoft 帐户以登录到 Client1。

## <a name="see-also"></a>请参阅


- [Active Directory 联合身份验证服务操作方法视频系列：安装 AD FS 服务器场](https://technet.microsoft.com/video/dn469436)
- [Active Directory 联合身份验证服务操作方法视频系列：更新证书](https://technet.microsoft.com/video/adfs-updating-certificates)
- [Active Directory 联合身份验证服务操作方法视频系列：添加信赖方信任](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust)
- [Active Directory 联合身份验证服务操作方法视频系列：启用设备注册服务](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service)
- [Active Directory 联合身份验证服务操作方法视频系列：安装 Web 应用程序代理](https://technet.microsoft.com/video/dn469438)




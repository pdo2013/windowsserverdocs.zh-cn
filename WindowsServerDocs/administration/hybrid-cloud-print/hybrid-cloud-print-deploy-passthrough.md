---
title: 部署 Windows Server 混合云打印-直通身份验证
description: 如何使用传递身份验证设置 Microsoft 混合云打印
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: Windows Server 2016
ms.tgt_pltfrm: na
ms.topic: ''
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247c5e9
author: msjimwu
ms.author: coreyp
manager: dongill
ms.date: 3/15/2018
ms.openlocfilehash: f372c81cad480ad7d5595892985ae7a9a52ac093
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887108"
---
# <a name="deploy-windows-server-hybrid-cloud-print-with-passthrough-authentication"></a>部署 Windows Server 混合云打印使用直通身份验证

>适用于：Windows Server 2016

本主题中，为 IT 管理员介绍了 Microsoft 混合云打印解决方案的端到端部署。 基于现有 Windows 服务器作为打印服务器，并启用 Azure Active Directory 加入和 MDM 托管运行此解决方案层，设备能够发现并打印到组织管理打印机。

## <a name="pre-requisites"></a>先决条件

有大量的订阅、 服务和你将需要在开始此安装之前获取的计算机。 它们是：

-   Azure AD 高级版订阅
    
    请参阅[开始使用 Azure 订阅](https://azure.microsoft.com/trial/get-started-active-directory/)，到 Azure 试用版订阅。 

-   MDM 服务，例如 Intune
    
    请参阅[Microsoft Intune](https://www.microsoft.com/en-us/cloud-platform/microsoft-intune)，到 Intune 试用版订阅。

-   Windows Server Active Directory 作为运行

    请参阅[分步：设置 Windows Server 2016 中的 Active Directory](https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/)，有关 Active Directory 设置的帮助。

-   已加入域作为打印服务器运行 Windows Server 2016
    
    请参阅[通过使用添加角色和功能向导安装角色、 角色服务和功能](https://docs.microsoft.com/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features#BKMK_installarfw)，有关如何在 Windows Server 上安装角色和角色服务的信息。

-   Azure AD Connect
    
    请参阅[Azure AD Connect 的自定义安装](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom)，有关设置 Azure AD Connect 的帮助。

-   Azure 应用程序代理连接器在单独的域加入的 Windows Server 计算机
    
    请参阅[开始使用应用程序代理并安装连接器](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable)，有关设置 Azure 应用程序代理连接器的帮助。

-   公开的域的名称
    
    可以使用由 Azure 中，为您创建的域名，也可以购买你自己的域名。

## <a name="deployment-steps"></a>部署步骤

本指南概述了五 （5） 安装步骤：

- 第 1 步：安装 Azure AD Connect 将 Azure AD 之间同步和在本地 AD
- 步骤 2：打印服务器上安装混合云打印包
- 步骤 3:使用直通身份验证安装 Azure 应用程序代理 (AAP)
- 步骤 4：配置所需的 MDM 策略
- 步骤 5：发布的共享的打印机

### <a name="step-1---install-azure-ad-connect-to-sync-between-azure-ad-and-on-premises-ad"></a>步骤 1-安装 Azure AD Connect 将 Azure AD 之间同步和在本地 AD
1. Windows Server Active Directory 计算机上，下载 Azure AD Connect 软件
2. 启动 Azure AD Connect 安装包，然后选择**使用快速设置**
3. 输入在安装过程中请求的信息，然后单击**安装**

### <a name="step-2---install-hybrid-cloud-print-package-on-the-print-server"></a>步骤 2-打印服务器上安装混合云打印包

1. 安装混合云打印 PowerShell 模块
    -  从提升的 PowerShell 命令提示符运行以下命令
        - `find-module -Name "PublishCloudPrinter"` 若要确认计算机可以访问 PowerShell 库 (PSGallery)
        - `install-module -Name "PublishCloudPrinter"`

    > 注意：你可能会看到消息指出 PSGallery 一个不受信任的存储库。  输入 y 以继续进行安装。

2. 安装混合云打印解决方案
    - 在同一个提升 PowerShell 命令提示符，将目录更改为 `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`
    - 运行 <br>
        `CloudPrintDeploy.ps1 -AzureTenant <Domain name used by Azure AD Connect> -AzureTenantGuid <Azure AD Directory ID>`
3.  配置 2 个 IIS 终结点以支持 SSL
    -   SSL 证书可以是自签名的证书或某些受信任证书颁发机构 (CA) 颁发一个
    -  如果使用自签名的证书，请确保将证书导入到客户端计算机
4.  安装 SQLite 包
    - 打开提升的 PowerShell 命令提示符
    - 运行以下命令以下载 System.Data.SQLite 的 nuget 包 <br>
        `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`
    - 运行以下命令以安装包<br>
    `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

    > 注意：我们建议下载并安装最新版本通过省略"-requiredversion"选项。

5.  将 SQLite dll 复制到 MopriaCloudService Webapp \<bin\>文件夹 (**c:\\inetpub\\wwwroot\\MopriaCloudService\\bin**): <br>
    - SQLite 二进制文件应位于"\\Program Files\\PackageManagement\\NuGet\\包"

            \\System.Data.SQLite.**Core**.x.x.x.x\\lib\\net46\\System.Data.SQLite.dll
            --\> \<bin\>\\System.Data.SQLite.dll  
            \\System.Data.SQLite.**Core**.x.x.x.x\\build\\net46\\x86\\SQLite.Interop.dll
            --\> \<bin\>\\**x86**\\SQLite.Interop.dll  
            \\System.Data.SQLite.**Core**.x.x.x.x\\build\\net46\\x64\\SQLite.Interop.dll
            --\> \<bin\>\\**x64**\\SQLite.Interop.dll
            \\System.Data.SQLite.**Linq**.x.x.x.x\\lib\\net46\\System.Data.SQLite.Linq.dll
            --\> \<bin\>\\System.Data.SQLite.Linq.dll  
            \\System.Data.SQLite.**EF6**.x.x.x.x\\lib\\net46\\System.Data.SQLite.EF6.dll
            --\> \<bin\>\\System.Data.SQLite.EF6.dll

    > 注意： x.x.x.x 是安装上面的 SQLite 版本。

6.  更新`c:\inetpub\wwwroot\MopriaCloudService\web.config`文件以在下面的示例包括 SQLite 版本 x.x.x.x\<运行时\>/\<assemblyBinding\>部分：

        <dependentAssembly>
        assemblyIdentity name="System.Data.SQLite" culture="neutral" publicKeyToken="db937bc2d44ff139" /
        <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
        </dependentAssembly>
        <dependentAssembly>
        <assemblyIdentity name="System.Data.SQLite.Core" culture="neutral" publicKeyToken="db937bc2d44ff139" />
        <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
        </dependentAssembly>
        <dependentAssembly>
        <assemblyIdentity name="System.Data.SQLite.EF6" culture="neutral" publicKeyToken="db937bc2d44ff139" />
        <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
        </dependentAssembly>
        <dependentAssembly>
        <assemblyIdentity name="System.Data.SQLite.Linq" culture="neutral" publicKeyToken="db937bc2d44ff139" />
        <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
        </dependentAssembly>

7. 创建的 SQLite 数据库：
    -  下载并安装 SQLite 工具二进制文件 <https://www.sqlite.org/>
    -  转到**c:\\inetpub\\wwwroot\\MopriaCloudService\\数据库**目录
    -  执行以下命令以在此目录中创建数据库：  `sqlite3.exe MopriaDeviceDb.db ".read MopriaSQLiteDb.sql"`
    -  从文件资源管理器，打开 MopriaDeviceDb.db 文件属性，以添加用户/组允许将发布到安全选项卡中的 Mopria 数据库
        - 建议仅将添加所需的管理员用户组。
8. 2 个 web 应用注册 Azure AD，以支持 OAuth2 身份验证
    -   以全局管理员身份访问 Azure AD 租户管理门户的登录名
        1. 转到"应用注册"选项卡，添加打印终结点
            -   添加应用程序中，选择"新建应用程序注册"
            -   提供一个适当的名称，并选择"Web 应用 / API"
            -   登录 URL ="http://MicrosoftEnterpriseCloudPrint/CloudPrint"
        2.  重复的发现终结点
            -   登录 URL ="http://MopriaDiscoveryService/CloudPrint"
        3.  重复的本机客户端应用程序
            -   提供应用名称，请确保选择"本机客户端应用程序"作为"应用程序类型"
            -   Redirect URI = "https://\<services-machine-endpoint\>/RedirectUrl"
        4.  转到本机客户端应用"设置"
            -   复制"应用程序 ID"值以用于更高版本的安装步骤
            -   选择"所需的权限"
                1.  单击"添加"
                2.  单击"选择 API"
                3.  通过定义时创建的应用程序终结点的名称搜索打印终结点和发现终结点
                4.  添加 2 个终结点
                5.  请确保启用了每个应用程序终结点的"委派权限"选项
                6.  请确保单击底部的"完成"按钮
                7.  添加两个终结点后，请单击"授予权限"。  选择"是"当系统提示你批准请求。
            -   转到"重定向 URI"，并向列表中添加以下重定向 Uri: `ms-appx-web://Microsoft.AAD.BrokerPlugin/\<NativeClientAppID\>`
                `ms-appx-web://Microsoft.AAD.BrokerPlugin/S-1-15-2-3784861210-599250757-1266852909-3189164077-45880155-1246692841-283550366`

### <a name="step-3---install-azure-application-proxy-aap-with-passthrough-authentication"></a>第 3 步-使用直通身份验证安装 Azure 应用程序代理 (AAP)
1. 登录到你的 Azure AD (AAD) 租户管理门户
    - 在 AAD 菜单列表中，选择"应用程序代理"
    - 单击屏幕顶部的"启用应用程序代理"
    - 下载到已加入域的 Windows Server 计算机起作用，作为 Web 应用程序代理 (WAP) 的"应用程序代理连接器"。
2. WAP 计算机，以管理员身份安装"应用程序代理连接器"的登录名
    - 在安装期间，应用程序代理连接器向提供的凭据在你想要启用 AAP 上的 Azure 原则
    - 请确保 WAP 计算机已加入到你的本地 Active Directory 域
3. 返回到 AAD 租户管理门户并添加应用程序代理
    - 转到**企业应用程序**选项卡
    - 单击**新的应用程序**
    - 选择**的本地应用程序**并填写字段
        - 名称：您希望的任何名称
        - 内部 URL:这是到 Mopria 发现云服务在 WAP 计算机可以访问的内部 URL
        - 外部 URL:根据你的组织需要配置
        - 预身份验证方法：传递

    >   注意：如果找不到更高版本的所有设置，则添加可用的设置与代理和选择刚创建的应用程序代理并转到**应用程序代理**选项卡上，并添加所有上述信息。

4. 企业云打印服务的更高版本，重复 3，并提供你的企业云打印服务的内部 URL

    >   注意：Https://&lt;services 机终结点 &gt; /mcs 下面所述的 URL 应是为 Mopria 云服务和/或企业云打印服务设置的外部 URL。


### <a name="step-4---configure-the-required-mdm-policies"></a>步骤 4-配置所需的 MDM 策略
- 登录到你的 MDM 提供程序
- 查找企业云打印策略组和配置策略遵循以下指导原则：
    - CloudPrintOAuthAuthority = https://login.microsoftonline.com/\<Azure AD Directory ID\>
    - CloudPrintOAuthClientId = 本机 Azure AD 管理门户中注册的 Web 应用的"应用程序 ID"值
    - CloudPrinterDiscoveryEndPoint = Mopria 发现服务 Azure 应用程序代理的步骤 3.3 中创建的外部 URL (必须是完全相同但不是带尾部 /)
    - MopriaDiscoveryResourceId ="应用 ID URI"的 Web 应用 / 步骤 2.8 中已注册的发现终结点的 API。  您可以在设置下找到此-> 应用属性
    - CloudPrintResourceId ="应用 ID URI"的 Web 应用 / 打印终结点的 API 已在步骤 2.8 中注册。 您可以在设置下找到此-> 应用属性
    - DiscoveryMaxPrinterLimit = \<a positive integer\>

>   注意：如果使用 Microsoft Intune 服务，可以找到"云打印机"类别下的这些设置。

|Intune 显示名称                     |策略                         |
|----------------------------------------|-------------------------------|
|打印机发现 URL                   |CloudPrinterDiscoveryEndpoint  |
|打印机访问授权机构 URL            |CloudPrintOAuthAuthority       |
|Azure 本机客户端应用 GUID            |CloudPrintOAuthClientId        |
|打印服务资源 URI              |CloudPrintResourceId           |
|查询 （仅移动版） 的打印机最大数  |DiscoveryMaxPrinterLimit       |
|打印机发现服务资源 URI  |MopriaDiscoveryResourceId      |


>   注意：如果云打印策略组不可用，但 MDM 提供程序支持的 OMA URI 设置，则可以设置相同的策略。  请参阅此<a href="https://docs.microsoft.com/windows/client-management/mdm/policy-csp-enterprisecloudprint#enterprisecloudprint-cloudprintoauthauthority">一文</a>有关的其他信息。

- OMA-URI
    - `CloudPrintOAuthAuthority = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthAuthority`
        - 值 = `https://login.microsoftonline.com/` \<Azure AD 目录 ID\>
    - `CloudPrintOAuthClientId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthClientId`
        - 值 = \<Azure AD 本机应用的应用程序 ID\>
    - `CloudPrinterDiscoveryEndPoint = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrinterDiscoveryEndPoint`
        - 值 = Mopria 发现服务 Azure 应用程序代理的步骤 3.3 中创建的外部 URL (必须是完全相同但不是带尾部 /)
    - `MopriaDiscoveryResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/MopriaDiscoveryResourceId`
        - 值 ="应用 ID URI"的 Web 应用 / 步骤 2.8 中已注册的发现终结点的 API。  您可以在设置下找到此-> 应用属性
    - `CloudPrintResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId`
        - 值 ="应用 ID URI"的 Web 应用 / 打印终结点的 API 已在步骤 2.8 中注册。 您可以在设置下找到此-> 应用属性
    - `DiscoveryMaxPrinterLimit = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/DiscoveryMaxPrinterLimit`
        - 值 =\<正整数\>
  
### <a name="step-5---publish-desired-shared-printers"></a>步骤 5-发布所需的共享的打印机
1. 在打印服务器上安装所需的打印机
2. 共享打印机通过打印机属性用户界面
3. 选择所需的用户授予访问权限集
4. 保存所做的更改，关闭打印机属性窗口
5. 在 Windows 10 Fall Creator Update 计算机中，打开提升的 Windows PowerShell 命令提示符
    1. 运行以下命令
        - `find-module -Name "PublishCloudPrinter"` 若要确认计算机可以访问 PowerShell 库 (PSGallery)
        - `install-module -Name "PublishCloudPrinter"`

        >   注意：你可能会看到消息指出 PSGallery 一个不受信任的存储库。  输入 y 以继续进行安装。

        - Publish-CloudPrinter
            - 打印机 = 已定义的共享的打印机名称
            - 制造商 = 打印机制造商
            - 模型 = 打印机型号
            - OrgLocation = JSON 字符串，它指定打印机的位置，例如：   `{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"Microsoft", "depth":1}, {"category":"site", "vs":"Redmond, WA", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}`
            - Sddl = SDDL 字符串，表示打印机的权限。 这可以获得通过适当地修改打印机属性的安全性选项卡，然后在命令提示符处运行以下命令： `(Get-Printer PrinterName -full).PermissionSDDL`
                例如"G:DUD:(A;OICI;FA;;;WD)"

            > 注意：将需要添加**`O:BA`** 作为前缀的结果从上面的命令提示符命令之前将值设置为 SDDL 设置。  例如：SDDL = `O:BAG:DUD:(A;OICI;FA;;;WD)`

            - DiscoveryEndpoint = https://&lt;services-machine-endpoint&gt;/mcs
            - PrintServerEndpoint = https://&lt;services-machine-endpoint&gt;/ecp
            - AzureClientId = 上面提供的已注册的本机 Web 应用程序值的应用程序 ID
            - AzureTenantGuid = Azure AD 租户的目录 ID
            - DiscoveryResourceId = 代理 Mopria 发现云服务的 [可选] 应用程序 ID

        > 注意：可以在的命令行中输入所有必需的参数值。<br>
**发布 CloudPrinter** PowerShell 命令语法： <br>
发布 CloudPrinter-打印机\<字符串\>-制造商\<字符串\>-模型\<字符串\>-OrgLocation\<字符串\>-Sddl \<字符串\>-DiscoveryEndpoint\<字符串\>-PrintServerEndpoint\<字符串\>-AzureClientId\<字符串\>-AzureTenantGuid \<字符串\>[-DiscoveryResourceId\<字符串\>] <br>
示例命令： `publish-cloudprinter -Printer EcpPrintTest -Manufacturer Microsoft -Model FilePrinterEcp -OrgLocation '{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"MyCompany", "depth":1}, {"category":"site", "vs":"MyCity, State", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}' -Sddl "O:BAG:DUD:(A;OICI;FA;;;WD)" -DiscoveryEndpoint https://<services-machine-endpoint>/mcs -PrintServerEndpoint https://<services-machine-endpoint>/ecp -AzureClientId <Native Web App ID> -AzureTenantGuid <Azure AD Directory ID> -DiscoveryResourceId <Proxied Mopria Discovery Cloud Service App ID>`


## <a name="verifing-the-deployment"></a>验证部署
在 Azure AD 加入设备的 MDM 策略配置：
- 打开 web 浏览器并转到 https://&lt;services 机终结点 &gt; /mcs/服务
- 您应看到描述此终结点的功能集的 JSON 文本
- 转到"OS 设置-\>设备-\>打印机和扫描仪"
    - 你应看到"搜索云打印机"链接
    - 单击的链接
    - 单击"请选择搜索位置"链接
        - 你应看到设备位置层次结构
    - 选择一个位置，并单击**确定**，然后单击**搜索**按钮以查找打印机
    - 选择打印机，然后单击**添加设备**按钮
    - 安装完成后成功的打印机，打印到打印机从你最喜欢的应用程序

>   注意：如果使用"EcpPrintTest"打印机，您可以在下的打印服务器计算机中找到输出文件"c:\\ECPTestOutput\\EcpTestPrint.xps"位置。
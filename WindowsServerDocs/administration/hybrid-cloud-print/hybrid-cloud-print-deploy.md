---
title: 部署 Windows Server 混合云打印
description: 如何设置 Microsoft 混合云打印
ms.prod: windows-server
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
ms.openlocfilehash: af5cd5f83633df7e704f4b768baf8dc6d78546aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370439"
---
# <a name="deploy-windows-server-hybrid-cloud-print-with-pre-authentication"></a>部署 Windows Server 混合云打印与预身份验证

>适用于：Windows Server 2016

本主题为 IT 管理员介绍了 Microsoft 混合云打印解决方案的端到端部署。 此解决方案层位于作为打印服务器运行的现有 Windows Server 之上，并允许 Azure Active Directory 联接和 MDM 管理的设备发现和打印到组织托管的打印机。

## <a name="pre-requisites"></a>先决条件

开始此安装之前，需要获取多个订阅、服务和计算机。 它们是：

-   Azure AD 高级订阅
    
    若要获取 Azure 试用订阅，请参阅[azure 订阅入门](https://azure.microsoft.com/trial/get-started-active-directory/)。 

-   MDM 服务，例如 Intune
    
    请参阅[Microsoft Intune](https://www.microsoft.com/en-us/cloud-platform/microsoft-intune)，了解 Intune 的试用订阅。

-   作为 Active Directory 运行的 Windows Server

    请[参阅分步：若要设置 Active Directory，请在](https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/)Windows Server 2016 中设置 Active Directory。

-   已加入域的 Windows Server 2016 作为打印服务器运行
    
    有关如何在 Windows Server 上安装角色和角色服务的信息，请参阅[使用 "添加角色和功能" 向导安装角色、角色服务和功能](https://docs.microsoft.com/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features#BKMK_installarfw)。

-   Azure AD Connect
    
    有关设置 Azure AD Connect 的帮助，请参阅[Azure AD Connect 的自定义安装](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom)。

-   在已加入域的单独 Windows Server 计算机上 Azure 应用程序代理连接器
    
    有关设置 Azure 应用程序代理连接器的帮助，请参阅[应用程序代理入门和安装连接器](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable)。

-   面向公众的域名
    
    你可以使用 Azure 为你创建的域名，或购买你自己的域名。

## <a name="deployment-steps"></a>部署步骤

本指南概述了五（5）个安装步骤：

- 第 1 步：安装 Azure AD Connect 以便在 Azure AD 和本地 AD 之间同步
- 步骤 2：在打印服务器上安装混合云打印包
- 步骤 3:安装 Azure 应用程序 Proxy （AAP-SCHEME）与 Kerberos 约束委派（KCD）
- 步骤 4：配置所需的 MDM 策略
- 步骤 5：发布共享打印机

### <a name="step-1---install-azure-ad-connect-to-sync-between-azure-ad-and-on-premises-ad"></a>步骤 1-安装 Azure AD Connect 以便在 Azure AD 和本地 AD 之间同步
1. 在 Windows Server Active Directory 计算机上，下载 Azure AD Connect 软件
2. 启动 Azure AD Connect 安装包并选择 "**使用快速设置**"
3. 输入安装过程中所需的信息，然后单击 "**安装**"

### <a name="step-2---install-hybrid-cloud-print-package-on-the-print-server"></a>步骤 2-在打印服务器上安装混合云打印包

1. 安装混合云打印 PowerShell 模块
   - 在提升权限的 PowerShell 命令提示符下运行以下命令
      - `find-module -Name "PublishCloudPrinter"`确认计算机可以访问 PowerShell 库（PSGallery）
      - `install-module -Name "PublishCloudPrinter"`

     > 注意：你可能会看到一条消息，指出 "PSGallery" 是不受信任的存储库。  输入 "y" 以继续安装。

2. 安装混合云打印解决方案
    - 在已提升权限的 PowerShell 命令提示符下，将目录更改为`C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`
    - 运行 <br>
        `CloudPrintDeploy.ps1 -AzureTenant <Domain name used by Azure AD Connect> -AzureTenantGuid <Azure AD Directory ID>`
3. 配置2个 IIS 终结点以支持 SSL
   -   SSL 证书可以是自签名证书，也可以是从某些受信任的证书颁发机构（CA）颁发的证书
   -  如果使用自签名证书，请确保将证书导入到客户端计算机
4. 安装 SQLite 包
   - 打开提升权限的 PowerShell 命令提示符
   - 运行以下命令以下载 system.exception nuget 包 <br>
       `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`
   - 运行以下命令以安装包<br>
   `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

   > 注意：建议通过保留 "-requiredversion" 选项来下载并安装最新版本。

5. 将 SQLite dll 复制到\<MopriaCloudService Webapp bin\>文件夹（**C：\\inetpub\\\\wwwroot\\MopriaCloudService bin**）： <br>
   - SQLite 二进制文件\\应为 "Program Files\\PackageManagement\\NuGet\\包"

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

   > 注意： x.x.x.x 是上面安装的 SQLite 版本。

6. 在以下\<运行时\>assemblyBinding\>部分更新文件，使其包含 SQLite 版本 1.x. x. x. x. x. x. x. x. x： `c:\inetpub\wwwroot\MopriaCloudService\web.config` /\<

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

7. 创建 SQLite 数据库：
    -  从下载并安装 SQLite 工具二进制文件<https://www.sqlite.org/>
    -  中转到**c：\\inetpub\\wwwroot\\MopriaCloudService\\数据库**目录
    -  执行以下命令以在此目录中创建数据库：`sqlite3.exe MopriaDeviceDb.db ".read MopriaSQLiteDb.sql"`
    -  在文件资源管理器中，打开 MopriaDeviceDb 文件属性以添加允许在 "安全" 选项卡中发布到 Mopria 数据库的用户/组
        - 建议仅添加所需的管理用户组。
8. 将2个 web 应用注册到 Azure AD 以支持 OAuth2 authentication
   - 以全局管理员身份登录到 Azure AD 租户管理门户
     1. 前往 "应用注册" 选项卡以添加打印终结点
        - 添加应用程序，选择 "新应用程序注册"
        - 提供适当的名称，然后选择 "Web 应用/API"
        - 登录 URL = "<http://MicrosoftEnterpriseCloudPrint/CloudPrint>"
     2. 针对发现终结点重复此操作
        - 登录 URL = "<http://MopriaDiscoveryService/CloudPrint>"
     3. 为 native client 应用程序重复
        -   提供应用名称时，请确保选择 "本机客户端应用程序" 作为 "应用程序类型"
        -   重定向 URI = "\<https://\>/RedirectUrl"
     4. 进入本机客户端应用 "设置"
        -   复制 "应用程序 ID" 值以用于后面的安装步骤
        -   选择 "所需权限"
            1.  单击 "添加"
            2.  单击 "选择 API"
            3.  按照创建应用终结点时定义的名称搜索打印终结点和发现终结点
            4.  添加2个终结点
            5.  请确保已启用每个应用终结点的 "委派的权限" 选项
            6.  请确保单击底部的 "完成" 按钮
            7.  添加这两个终结点后，单击 "授予权限"。  出现批准请求时，请选择 "是"。
        -   转到 "重定向 URI"，并将以下重定向 Uri 添加到列表中：`ms-appx-web://Microsoft.AAD.BrokerPlugin/\<NativeClientAppID\>`
            `ms-appx-web://Microsoft.AAD.BrokerPlugin/S-1-15-2-3784861210-599250757-1266852909-3189164077-45880155-1246692841-283550366`

### <a name="step-3---install-azure-application-proxy-aap-with-kerberos-constrained-delegation-kcd"></a>步骤 3-安装 Azure 应用程序 Proxy （AAP-SCHEME）与 Kerberos 约束委派（KCD）
1. 登录到 Azure AD （AAD）租户管理门户
    - 在 AAD 菜单列表中，选择 "应用程序代理"
    - 单击屏幕顶部的 "启用应用程序代理"
    - 将 "应用程序代理连接器" 下载到将充当 Web 应用程序代理（WAP）的已加入域的 Windows Server 计算机。
2. 在 WAP 计算机上，以管理员身份登录并安装 "应用程序代理连接器"
    - 在安装过程中，为应用程序代理连接器提供要在其上启用 AAP-SCHEME 的 Azure 原则的凭据
    - 请确保 WAP 计算机已加入本地 Active Directory
3. 在 Active Directory 计算机上，请参阅 "**工具"-"> 用户和计算机**"
    - 选择运行应用程序代理连接器的计算机
    - 右键单击并选择 "**属性-> 委派**" 选项卡
    - 选择 "**仅信任此计算机来委派指定的服务"。**
    - 选择 "**使用任何身份验证协议"。**
    - 在**此帐户可以向其提供委派凭据的服务**下
        - 添加运行服务的计算机的 SPN 标识值（MopriaDiscoveryService 和 MicrosoftEnterpriseCloudPrint 服务）
            - 对于 SPN，输入计算机本身的 SPN，即主机/\<计算机名\>。\<域\>"<br>
                `HOST/appServer.Contoso.com`
4. 返回到 AAD 租户管理门户并添加应用程序代理
   - 中转到 "**企业应用程序**" 选项卡
   - 单击 "**新建应用程序**"
   - 选择 **"本地应用程序**" 并填写字段
       - 名称：所需的任何名称
       - 内部 URL：这是你的 WAP 计算机可以访问的 Mopria 发现云服务的内部 URL
       - 外部 URL：根据组织的需要配置
       - 预身份验证方法：Azure Active Directory

     >   注意:如果你找不到上述所有设置，请添加具有可用设置的代理，然后选择你刚刚创建的应用程序代理，然后单击 "**应用程序代理**" 选项卡并添加上述所有信息。

   - 创建后，返回到 "**企业应用程序** -> " "**所有应用**程序"，选择刚创建的新应用程序
   - 请参阅 "**单一登录**"，确保 "单一登录模式" 设置为 "集成 Windows 身份验证"
   - 将 "内部应用程序 SPN" 设置为你在步骤3.3 中指定的 SPN
   - 请确保 "委派的登录标识" 设置为 "用户主体名称"

5. 对于 Enterprise Cloud 打印服务，请重复上述4，并提供企业云打印服务的内部 URL
6. 返回 Azure AD 租户管理门户，并选择 "本机客户端应用"-> "设置"**应用注册**
    - 选择**所需权限**
        - 添加刚刚创建的两个新的代理应用程序
        - 授予这两个应用程序的委派权限
        - 添加这两个代理应用程序后，单击 "授予权限"。  出现批准请求时，请选择 "是"。

7. 在 IIS 中为 Mopria 云服务和企业云打印服务计算机启用 Windows 身份验证
    - 请确保已安装 Windows 身份验证功能：
        1. 打开服务器管理器
        2. 单击 "**管理**"
        3. 单击 "**添加角色和功能**"
        4. 选择**基于角色或基于功能的安装**
        5. 选择服务器
        6. 在 "Web 服务器（IIS）" 下 > Web 服务器-> 安全性 "，选择" **Windows 身份验证**"
        7. 单击 "下一步"，直到完成安装
    - 在 IIS 中启用 Windows 身份验证：
        1. 打开 Internet Information Services （IIS）管理器
        2. 对于每个服务/站点：
            1.  选择服务/站点
            2.  双击**身份验证**
            3.  单击 " **Windows 身份验证**"，然后单击 "**操作**" 下的**启用**

### <a name="step-4---configure-the-required-mdm-policies"></a>步骤 4-配置所需的 MDM 策略
- 登录到 MDM 提供程序
- 查找企业云打印策略组并按照以下准则配置策略：
  - CloudPrintOAuthAuthority = https://login.microsoftonline.com/\<Azure AD 目录 ID\>
  - CloudPrintOAuthClientId = 在 Azure AD 管理门户中注册的本机 Web 应用的 "应用程序 ID" 值
  - CloudPrinterDiscoveryEndPoint = 在步骤3.3 中创建的 Mopria Discovery 服务 Azure 应用程序代理的外部 URL （必须完全相同，但不包含尾随/）
  - MopriaDiscoveryResourceId = 在步骤3.4 中创建的 Mopria Discovery 服务 Azure 应用程序代理的外部 URL （必须完全相同，包括尾随/）
  - CloudPrintResourceId = 在步骤3.5 中创建的企业云打印服务 Azure 应用程序代理的外部 URL （必须完全相同（包括尾随/）
  - DiscoveryMaxPrinterLimit = \<正整数\>

>   注意:如果使用 Microsoft Intune 服务，则可以在 "云打印机" 类别下找到这些设置。

|Intune 显示名称                     |策略                         |
|----------------------------------------|-------------------------------|
|打印机发现 URL                   |CloudPrinterDiscoveryEndpoint  |
|打印机访问授权 URL            |CloudPrintOAuthAuthority       |
|Azure native client 应用 GUID            |CloudPrintOAuthClientId        |
|打印服务资源 URI              |CloudPrintResourceId           |
|要查询的最大打印机数（仅限移动设备）  |DiscoveryMaxPrinterLimit       |
|打印机发现服务资源 URI  |MopriaDiscoveryResourceId      |

>   注意:如果云打印策略组不可用，但 MDM 提供程序支持 OMA URI 设置，则可以设置相同的策略。  有关其他信息，请参阅此<a href="https://docs.microsoft.com/windows/client-management/mdm/policy-csp-enterprisecloudprint#enterprisecloudprint-cloudprintoauthauthority">文章</a>。

- OMA-URI
    - `CloudPrintOAuthAuthority = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthAuthority`
        - 值 = `https://login.microsoftonline.com/` \<Azure AD 目录 ID\>
    - `CloudPrintOAuthClientId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthClientId`
        - 值 = \<Azure AD 本机应用的应用程序 ID\>
    - `CloudPrinterDiscoveryEndPoint = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrinterDiscoveryEndPoint`
        - 值 = 在步骤3.3 中创建的 Mopria 发现服务 Azure 应用程序代理的外部 URL （必须完全相同，但不包含尾随/）
    - `MopriaDiscoveryResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/MopriaDiscoveryResourceId`
        - 值 = 在步骤3.4 中创建的 Mopria 发现服务 Azure 应用程序代理的外部 URL （必须完全相同（包括尾随/）
    - `CloudPrintResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId`
        - 值 = 企业云打印服务 Azure 应用程序代理在步骤3.5 中创建的外部 URL （必须完全相同，包括尾随/）
    - `DiscoveryMaxPrinterLimit = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/DiscoveryMaxPrinterLimit`
        - 值 = \<正整数\>

### <a name="step-5---publish-desired-shared-printers"></a>步骤 5-发布所需的共享打印机
1. 在打印服务器上安装所需的打印机
2. 通过 "打印机属性" UI 共享打印机
3. 选择要授予访问权限的所需用户组
4. 保存更改并关闭 "打印机属性" 窗口
5. 在 Windows 10 秋季创建者更新计算机上，打开提升的 Windows PowerShell 命令提示符
   1. 运行以下命令
      - `find-module -Name "PublishCloudPrinter"`确认计算机可以访问 PowerShell 库（PSGallery）
      - `install-module -Name "PublishCloudPrinter"`

        >   注意：你可能会看到一条消息，指出 "PSGallery" 是不受信任的存储库。  输入 "y" 以继续安装。

      - 发布-CloudPrinter
        - Printer = 定义的共享打印机名称
        - 制造商 = 打印机制造商
        - 型号 = 打印机型号
        - OrgLocation = 指定打印机位置的 JSON 字符串，例如：`{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"Microsoft", "depth":1}, {"category":"site", "vs":"Redmond, WA", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}`
        - Sddl = 用于表示打印机权限的 SDDL 字符串。 此操作可通过相应地修改打印机属性的 "安全" 选项卡来获取，然后在命令提示符下运行以下命令：`(Get-Printer PrinterName -full).PermissionSDDL`
            例如"G:DUD：（A; OICI; FA;;;WD） "

          > 注意：在将值设置为 **`O:BA`** "SDDL" 设置之前，你将需要在命令提示符命令的结果中添加作为前缀。  例如：SDDL =`O:BAG:DUD:(A;OICI;FA;;;WD)`

        - DiscoveryEndpoint = 在步骤3.4 中创建的 Mopria Discovery 服务 Azure 应用程序代理的外部 URL
        - PrintServerEndpoint = 在步骤3.5 中创建的企业云打印服务 Azure 应用程序代理的外部 URL
        - AzureClientId = 以上注册的本机 Web 应用值的应用程序 ID
        - AzureTenantGuid = Azure AD 租户的目录 ID
        - DiscoveryResourceId = [Optional] 代理的 Mopria Discovery 云服务的应用程序 ID

        > 注意：也可以在命令行中输入所有必需的参数值。<br>
        **发布-CloudPrinter**PowerShell 命令语法： <br>
        CloudPrinter-Printer \<string @ no__t-1-制造商 \<string @ no__t-3-Model \<string @ no__t-5-OrgLocation \<string @ no__t-8String @no__t-no__t @ DiscoveryEndpoint-11-PrintServerEndpoint \>2string @ no__t-13-AzureClientId 4string @ no__t-15-AzureTenantGuid 6string @ no__t-17 [-DiscoveryResourceId 8string @ no__t-19] <br>
        示例命令：`publish-cloudprinter -Printer EcpPrintTest -Manufacturer Microsoft -Model FilePrinterEcp -OrgLocation '{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"MyCompany", "depth":1}, {"category":"site", "vs":"MyCity, State", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}' -Sddl "O:BAG:DUD:(A;OICI;FA;;;WD)" -DiscoveryEndpoint https://<services-machine-endpoint>/mcs -PrintServerEndpoint https://<services-machine-endpoint>/ecp -AzureClientId <Native Web App ID> -AzureTenantGuid <Azure AD Directory ID> -DiscoveryResourceId <Proxied Mopria Discovery Cloud Service App ID>`


## <a name="verifying-the-deployment"></a>验证部署
在配置了 MDM 策略的 Azure AD 联接设备上：
- 打开 web 浏览器并中转到 https://&lt;服务-计算机终结点&gt;/mcs/services （发现终结点的外部 URL）
- 应该会看到描述此终结点功能集的 JSON 文本
- 中转到 "OS 设置-\>设备-\>打印机 & 扫描仪"
    - 应该会看到 "搜索云打印机" 链接
    - 单击链接
    - 单击 "请选择搜索位置" 链接
        - 应会看到设备位置层次结构
    - 选择一个位置，然后单击 **"确定"** ，然后单击 "**搜索**" 按钮查找打印机
    - 选择 "打印机"，然后单击 "**添加设备**" 按钮
    - 安装成功后，从你喜欢的应用打印到打印机

>   注意:如果使用 "EcpPrintTest" 打印机，则可以在 "C：\\ECPTestOutput\\EcpTestPrint" 位置下的打印服务器计算机中找到输出文件。

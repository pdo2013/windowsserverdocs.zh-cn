---
title: "迁移广告 FS 2.0 联盟服务器"
description: "在迁移到 Windows Server 2012 R2 的广告 FS 服务器上提供的信息。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bef24f79cfa92dfeca1846501f14ebf6d8231f0d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>将广告 FS 2.0 联盟 server 迁移到在 Windows Server 2012 R2 上广告 FS

若要迁移属于单节点广告 FS 电场的日落，WIF 电场的日落或对 Windows Server 2012 R2 的 SQL Server 场广告 FS 联盟服务器，必须执行以下任务：  
  
1.  [导出和备份广告 FS 配置数据](migrate-ad-fs-fed-server-r2.md#export-and-backup-the-ad-fs-configuration-data)  
  
2.  [创建 Windows Server 2012 R2 联盟服务器电场的日落](migrate-ad-fs-fed-server-r2.md#create-a-windows-server-2012-r2-federation-server-farm)  
  
3.  [将配置数据原始导入到 Windows Server 2012 R2 广告 FS 电场的日落](migrate-ad-fs-fed-server-r2.md#import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm)  
  
##  <a name="export-and-backup-the-ad-fs-configuration-data"></a>导出和备份广告 FS 配置数据  
 若要导出的广告 FS 配置设置，请执行以下步骤：  
  
###  <a name="to-export-service-settings"></a>若要导出服务设置  
  
1.  确保你具有访问以下证书和他们专用键.pfx 文件中：  
  
    -   由你想要迁移联合服务器场 SSL 证书  
  
    -   （如果它是 SSL 证书）的服务通信证书由你想要迁移联合 server 场  
  
    -   所有第三方合作伙伴令牌签名或/解密加密令牌证书可由你想要迁移联合 server 场  
  
若要查找 SSL 证书，请打开 Internet 信息服务 (IIS) 管理控制台中，选择**默认网站**在左侧窗格中，单击**绑定…** 在**操作**窗格中，查找并选择 https 绑定、单击**编辑**，然后单击**视图**。  
  
你必须导出 SSL 证书联合身份验证服务和.pfx 文件其专用关键使用。 有关详细信息，请参阅[导出服务器身份验证证书专用键一部分](export-the-private-key-portion-of-a-server-authentication-certificate.md)。  
  
> [!NOTE]
>  如果你计划部署 Windows Server 2012 R2 中运行广告 FS 作为设备注册服务，您必须获取新的 SSL 证书。有关详细信息，请参阅[注册广告 FS SSL 证书](enroll-an-ssl-certificate-for-ad-fs.md)和[与设备注册服务配置联合服务器](configure-a-federation-server-with-device-registration-service.md)。  
  
若要查看令牌签名、令牌解密和服务使用的通信证书，运行以下 Windows PowerShell 命令创建文件中使用的所有证书列表：  
  
``` powershell
Get-ADFSCertificate | Out-File “.\certificates.txt”  
 ```  
  
2.  将广告 FS 联合身份验证服务属性，如联合身份验证服务名称、联合身份验证服务的显示名称、和联合身份验证的服务器标识符导出到一个文件。  
  
若要导出联合身份验证服务属性，请打开 Windows PowerShell 和运行以下命令： 

``` powershell
Get-ADFSProperties | Out-File “.\properties.txt”`.  
``` 

输出文件包含以下重要配置值：  
 
|**联合身份验证服务属性名称 Get-ADFSProperties 由报告**|**广告 FS 管理控制台在联合身份验证服务属性名称**|
|-----|-----|  
|主机|联合身份验证服务名称|  
|标识符|联合身份验证服务的标识符|  
|显示名称|联合身份验证服务显示名称|  
  
3.  备份应用程序的配置文件。 其他设置，此文件包含策略数据库连接字符串。  
  
若要备份应用程序的配置文件，则必须手动复制`%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`为备份的服务器上的安全位置的文件。  
  
> [!NOTE]
>  在此文件，请记下的数据库连接字符串后立即位于"policystore 连接 ="。 如果连接字符串指定 SQL Server 数据库，在还原原始联合身份验证的服务器上的广告 FS 配置时需要值。  
>   
>  以下是 WID 连接字符串的一个示例：`“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`。 下面是它的 SQL Server 连接字符串一个示例：`"Data Source=databasehostname;Integrated Security=True"`。  
  
4.  录制广告 FS 联合身份验证服务帐户的身份，并且此帐户的密码。  
  
若要查找的身份值，检查**登录为**列**广告 FS 2.0 Windows 服务**中**服务**控制台和手动录制此值。  
  
> [!NOTE]
>  独立联合身份验证服务、使用内置的网络服务帐户。  在此情况下，你不需要有一个密码。  
  
5.  导出到一个文件的列表中启用广告 FS 端点。  
  
若要执行此操作，请打开 Windows PowerShell 和运行以下命令： 

``` powershell
Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
``` 

6.  将任何自定义声明说明导出到一个文件。  
  
若要执行此操作，请打开 Windows PowerShell 和运行以下命令： 

``` powershell
Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
 ```

7.  如果你有如 useRelayStateForIdpInitiatedSignOn 配置 web.config 文件中的自定义设置，请确保备份 web.config 文件以供参考。 你可以将文件复制从映射到虚拟路径目录**"月 adfs 月 1！"** IIS 中。 默认情况下，它处于**%systemdrive%\inetpub\adfs\ls**目录。  
  
###  <a name="to-export-claims-provider-trusts-and-relying-party-trusts"></a>若要导出索赔提供商信任和信赖的方信任  
  
1.  导出广告 FS 索赔提供商的信任和依赖方信任，你必须以管理员身份登录 (但是，不域管理员作为) 你联盟到服务器，并运行以下 Windows PowerShell 脚本，它是位于**媒体/server_support/adfs** Windows Server 2012 R2 安装光盘的文件夹：`export-federationconfiguration.ps1`。  
  
> [!IMPORTANT]
>  导出脚本参数如下：  
>   
>  -   Export-FederationConfiguration.ps1-路径 < string\ > [-ComputerName < string\ >] [-凭据 < pscredential\ >] [-强制] [-CertificatePassword < securestring\ >]  
> -   Export-FederationConfiguration.ps1-路径 < string\ > [-ComputerName < string\ >] [-凭据 < pscredential\ >] [-强制] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustIdentifier < 字符串 [>] [-ClaimsProviderTrustIdentifier < 字符串 [>]  
> -   Export-FederationConfiguration.ps1-路径 < string\ > [-ComputerName < string\ >] [-凭据 < pscredential\ >] [-强制] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustName < 字符串 [>] [-ClaimsProviderTrustName < 字符串 [>]  
>   
>  **-RelyingPartyTrustIdentifier < 字符串 [>** -cmdlet 仅导出依赖方信任字符串深刻中指定的标识符。 默认设置为导出无信赖的方信任。 如果没有 RelyingPartyTrustIdentifier、ClaimsProviderTrustIdentifier、RelyingPartyTrustName，以及 ClaimsProviderTrustName 指定，脚本将导出所有依赖方信任和索赔信任提供商。  
>   
>  **-ClaimsProviderTrustIdentifier < 字符串 [>** -cmdlet 仅导出索赔提供商信任字符串深刻中指定的标识符。 默认设置为导出无索赔提供商信任。  
>   
>  **-RelyingPartyTrustName < 字符串 [>** -cmdlet 仅导出依赖方信任字符串深刻中指定的名称。 默认设置为导出无信赖的方信任。  
>   
>  **-ClaimsProviderTrustName < 字符串 [>** -cmdlet 仅导出索赔提供商信任字符串深刻中指定的名称。 默认设置为导出无索赔提供商信任。  
>   
>  **-路径 < string\ >** -通往将包含的已导出的文件的文件夹。  
>   
>  **-ComputerName < string\ >** -指定 STS 服务器主机名称。 默认设置为在本地计算机。 如果要在 Windows Server 2012 R2 的广告 FS 到迁移广告 FS 2.0 或在 Windows Server 2012 的广告 F，这是主机旧版广告 FS 服务器的名称。  
>   
>  **-凭据 < PSCredential\ >** -指定具有执行此操作的权限的用户帐户。 默认值为当前用户。  
>   
>  **-强制**– 指定到无法提示用户进行确认。  
>   
>  **-CertificatePassword < SecureString\ >** -指定导出广告 FS 证书专用键的密码。 如果未指定，脚本会提示输入密码，如果需要导出专用密钥与广告 FS 证书。  
>   
>  **输入**： 无  
>   
>  **输出**： 字符串-此 cmdlet 返回导出文件夹路径。 你可以管道返回到导入 FederationConfiguration 的对象。  
  
###  <a name="to-back-up-custom-attribute-stores"></a>若要备份自定义特性官方商城  
  
1.  你必须手动导出你想要保留在你在 Windows Server 2012 R2 的新广告 FS 农场里的所有自定义特性官方商城。  
  
> [!NOTE]
>  在 Windows Server 2012 R2，广告 FS 要求基于.NET Framework 4.0 或上方的自定义特性官方商城。 按照说明进行操作，并在[Microsoft.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)自行安装和设置.Net Framework 4.5。  
  
通过运行以下 Windows PowerShell 命令，可以被广告 FS 使用找到有关自定义特性官方商城的信息： 

``` powershell
Get-ADFSAttributeStore
```  

升级或迁移自定义特性官方商城的步骤会有所不同。  
  
2.  您必须手动导出你想要保留在你在 Windows Server 2012 R2 的新广告 FS 农场里的自定义特性存储的所有.dll 文件。 升级或迁移.dll 文件的自定义特性官方商城的步骤会有所不同。  
  
##  <a name="create-a-windows-server-2012-r2-federation-server-farm"></a>创建 Windows Server 2012 R2 联盟服务器电场的日落  
  
1.  在你想要用作联合身份验证的服务器，然后添加广告 FS server 角色计算机上安装 Windows Server 2012 R2 的操作系统。 有关详细信息，请参阅[安装广告 FS 角色服务](install-the-ad-fs-role-service.md)。 然后配置了 Active Directory 联合身份验证服务配置向导通过或通过 Windows PowerShell 你新联合身份验证服务。 详细信息，请参阅"在新的联合 server 场配置的第一个联盟服务器"中[配置联盟服务器](configure-a-federation-server.md)。  

在完成此步骤，必须遵循以下说明进行操作：  
  
-   你必须域管理员权限才能配置联合身份验证服务。  
  
-   使用该广告 FS 2.0 或在 Windows Server 2012 的广告 F，必须使用相同的联合身份验证服务名称 （电场的日落名称）。 如果你不使用的相同的联合身份验证服务的名称，你备份的证书将无法在你尝试配置 Windows Server 2012 R2 联合身份验证服务。  
  
-   指定是否 WID 或 SQL Server 联合 server 场。 如果是 SQL 电场的日落，指定的 SQL Server 数据库位置和实例名称。  
  
-   你必须提供 pfx 文件包含备份作为准备广告 FS 迁移过程的一部分 SSL 服务器的身份验证证书。  
  
-   你必须指定广告 FS 2.0 或 Windows Server 2012 电场的日落中的广告 F 中使用的相同服务帐户标识。  
  
2.  初始节点配置后，你可以向你的新场添加更多个节点。 详细信息，请参阅"添加到现有联盟服务器场联合服务器"中[配置联盟服务器](configure-a-federation-server.md)。  
  
##  <a name="import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm"></a>将配置数据原始导入到 Windows Server 2012 R2 广告 FS 电场的日落  
 现在，你有运行 Windows Server 2012 R2 广告 FS 联盟服务器场，你可以导入的原始广告 FS 配置数据。  
  
1.  导入和配置其他自定义的广告 FS 证书，包括外部登记令牌签名和加密的标记-解密/证书和服务通信证书是否不同于 SSL 证书。  
  
在广告 FS 管理控制台中，选择**证书**。 检查每个对时准备迁移到 certificates.txt 文件导出这些值，以验证服务通信，令牌-加密月解密和令牌签名证书。  
  
若要更改外部证书从默认自签名证书的标记解密或令牌签名证书，必须先禁用默认启用自动证书翻转功能。 若要执行此操作，你可以使用下面的 Windows PowerShell 命令：  
  
``` powershell 
Set-ADFSProperties –AutoCertificateRollover $false  
```  
  
2.  配置使用一组 AdfsProperties cmdlet AutoCertificateRollover 或 SSO 生存期如任何自定义广告 FS 服务设置。  
  
3.  若要导入广告 FS 信赖方信任和索赔提供商信任，您必须登录以管理员身份 (但是，不域管理员作为) 你联盟到服务器，并运行以下 Windows PowerShell 脚本，它是位于 \support\adfs 文件夹中的 Windows Server 2012 R2 安装光盘：  
  
``` powershell 
import-federationconfiguration.ps1  
```  
  
> [!IMPORTANT]
>  导入脚本参数如下：  
>   
>  -  导入 FederationConfiguration.ps1-路径 < string\ > [-ComputerName < string\ >] [-凭据 < pscredential\ >] [-强制] [-LogPath < string\ >] [-CertificatePassword < securestring\ >]  
> -   导入 FederationConfiguration.ps1-路径 < string\ > [-ComputerName < string\ >] [-凭据 < pscredential\ >] [-强制] [-LogPath < string\ >] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustIdentifier < 字符串 [>] [-ClaimsProviderTrustIdentifier < 字符串 [>  
> -   导入 FederationConfiguration.ps1-路径 < string\ > [-ComputerName < string\ >] [-凭据 < pscredential\ >] [-强制] [-LogPath < string\ >] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustName < 字符串 [>] [-ClaimsProviderTrustName < 字符串 [>]  
>   
>  **-RelyingPartyTrustIdentifier < 字符串 [>** -cmdlet 仅导入依赖方信任字符串深刻中指定的标识符。 导入任何依赖方信任是默认值。 如果没有 RelyingPartyTrustIdentifier、 ClaimsProviderTrustIdentifier、 RelyingPartyTrustName，以及 ClaimsProviderTrustName 指定，脚本将导入所有依赖方信任和索赔信任提供商。  
>   
>  **-ClaimsProviderTrustIdentifier < 字符串 [>** -cmdlet 只导入的索赔提供商信任字符串深刻中指定的标识符。 默认值为导入无索赔提供商信任。  
>   
>  **-RelyingPartyTrustName < 字符串 [>** -cmdlet 仅导入依赖方信任字符串深刻中指定的名称。 导入任何依赖方信任是默认值。  
>   
>  **-ClaimsProviderTrustName < 字符串 [>** -cmdlet 只导入的索赔提供商信任字符串深刻中指定的名称。 默认值为导入无索赔提供商信任。  
>   
>  **-路径 < string\ >** -包含要导入的配置文件的文件夹中的路径。  
>   
>  **-LogPath < string\ >** -通往将包含导入日志文件的文件夹。 在此文件夹中，将创建一个名为"import.log"的日志文件。  
>   
>  **-ComputerName < string\ >** -指定主机 STS 服务器名称。 默认设置为在本地计算机。 如果要在 Windows Server 2012 R2 的广告 FS 到迁移广告 FS 2.0 或在 Windows Server 2012 的广告 F，此参数应该设置为旧版广告 FS 服务器的主机。  
>   
>  **-凭据 < PSCredential\ >**-指定具有执行此操作的权限的用户帐户。 默认值为当前用户。  
>   
>  **-强制**– 指定到无法提示用户进行确认。  
>   
>  **-CertificatePassword < SecureString\ >** -指定导入广告 FS 证书专用键的密码。 如果未指定，脚本会提示输入密码，如果使用专用密钥广告 FS 证书需要导入。  
>   
>  **输入：**字符串-此命令采用作为输入导入文件夹路径。 你可以对此命令管道导出 FederationConfiguration。  
>   
>  **输出：**无。  
  
在信赖的方信任 WSFedEndpoint 任何尾随空格可能会导致导入到错误脚本。 在此情况下，从之前的文件导入手动删除空间。 例如，这些条目导致错误：  
  
    ```  
    <URI N="WSFedEndpoint">https://127.0.0.1:444 /</URI>  
    ```  
  
    ```  
    <URI N="WSFedEndpoint">https://myapp.cloudapp.net:83 /</URI>  
    ```  
  
     They must be edited to:  
  
    ```  
    <URI N="WSFedEndpoint">https://127.0.0.1:444/</URI>  
    ```  
  
    ```  
    <URI N="WSFedEndpoint">https://myapp.cloudapp.net:83/</URI>  
    ```  
> [!IMPORTANT]
>  如果你有 Active Directory 的索赔提供商信任来源系统上的任何自定义声明规则 （以外的广告 FS 默认规则），这些将不会通过这些脚本迁移。 这是因为 Windows Server 2012 R2 的新的默认值。 必须通过手动添加到新的 Windows Server 2012 R2 场中 Active Directory 索赔提供商信任合并任何自定义规则。  
  
4.  配置所有自定义的广告 FS 端点设置。 在广告 FS 管理控制台中，选择**端点**。 检查启用的广告 FS 端点根据启用广告 FS 端点准备广告 FS 迁移时导出到一个文件的列表。  
  
     \ 且的  
  
     配置任何自定义声明说明。 在广告 FS 管理控制台中，选择**索赔说明**。 检查广告 FS 索赔说明根据列表索赔描述准备广告 FS 迁移时导出到一个文件的列表。 添加任何包含在你的文件，但不是包括在广告 FS 默认列表中的自定义声明说明。 请注意到文件中 ClaimType 都地图中管理主机的索赔标识符。  
  
5.  安装和配置所有备份自定义特性官方商城。 以 administrator 身份，确保任何自定义特性官方商城二进制文件的广告 FS 配置指向它们在更新之前将升级到.NET Framework 4.0 或更高版本。  
  
6.  配置将映射到传统 web.config 文件参数的服务属性。  
  
    -   如果**useRelayStateForIdpInitiatedSignOn**已添加到**web.config**必须在你在 Windows Server 2012 R2 农场里的广告 FS 配置以下服务的属性，然后在你的广告 FS 2.0 或在 Windows 服务器 2012年电场的日落的广告 F 文件：  
  
        -   在 Windows Server 2012 R2 的广告 FS 包括**%systemroot%\ADFS\Microsoft.IdentityServer.Servicehost.exe.config**文件。 使用相同的语法作为创建元素**web.config**文件元素： `<useRelayStateForIdpInitiatedSignOn enabled="true" />`。 作为的一部分包含此元素**< microsoft.identityserver.web >**部分**Microsoft.IdentityServer.Servicehost.exe.config**文件。  
  
    -   如果**< persistIdentityProviderInformation 启用 ="真 & #124; false"lifetimeInDays ="90"enablewhrPersistence ="真 & #124; false"/ \ >**已添加到**web.config**你必须配置 Windows Server 2012 R2 场中你的广告 FS 中以下服务的属性，然后在你的广告 FS 2.0 或在 Windows 服务器 2012年电场的日落的广告 F 文件：  
  
        1.  在 Windows Server 2012 R2 的广告 FS，运行以下 Windows PowerShell 命令： `Set-AdfsWebConfig –HRDCookieEnabled –HRDCookieLifetime`。  
  
    -   如果**< singleSignOn 启用 ="真 & #124; false"/ \ >**已添加到**web.config**文件在你的广告 FS 2.0 或 Windows 服务器 2012年场中的广告 F 中，你不需要在 Windows Server 2012 R2 电场的日落设置广告 FS 中的任何其他服务属性。 默认情况下在 Windows Server 2012 R2 场中广告 FS 单一登录已启用。  
  
    -   如果 localAuthenticationTypes 设置添加了**web.config**必须在你在 Windows Server 2012 R2 农场里的广告 FS 配置以下服务的属性，然后在你的广告 FS 2.0 或在 Windows 服务器 2012年电场的日落的广告 F 文件：  
  
        -   集成，表单、 TlsClient，基本转换到等效广告 FS 在 Windows Server 2012 R2 的列表中的全局身份验证策略设置支持两种联合身份验证服务和代理身份验证类型。 这些设置可以配置中管理中贴靠下广告 FS**认证策略**。  
  
 导入的原始配置数据后，你可以根据需要自定义对广告 FS 登录页面。 有关详细信息，请参阅[自定义的广告 FS 登录页面](../operations/AD-FS-Customization-in-Windows-Server-2016.md)。  
  
## <a name="next-steps"></a>后续步骤
 [将 Active Directory 联合身份验证服务角色服务迁移到 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [准备迁移广告 FS 联盟服务器](prepare-migrate-ad-fs-server-r2.md)   
 [迁移广告 FS 联合身份验证的服务器代理](migrate-fed-server-proxy-r2.md)   
 [验证广告 FS 迁移到 Windows Server 2012 R2](verify-ad-fs-migration.md)
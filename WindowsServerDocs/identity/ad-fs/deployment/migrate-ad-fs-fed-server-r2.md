---
title: 迁移 AD FS 2.0 联合服务器
description: 提供有关将 AD FS 服务器迁移到 Windows Server 2012 R2 的信息。
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9e947f1894516de232a0db50bcbb56c7452098cd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359419"
---
# <a name="migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>将 AD FS 2.0 联合服务器迁移到 Windows Server 2012 R2 上的 AD FS

若要将属于单节点 AD FS 场、WIF 场或 SQL Server 场的 AD FS 联合服务器迁移到 Windows Server 2012 R2，则必须执行以下任务：  
  
1.  [导出并备份 AD FS 配置数据](migrate-ad-fs-fed-server-r2.md#export-and-backup-the-ad-fs-configuration-data)  
  
2.  [创建 Windows Server 2012 R2 联合服务器场](migrate-ad-fs-fed-server-r2.md#create-a-windows-server-2012-r2-federation-server-farm)  
  
3.  [将原始配置数据导入到 Windows Server 2012 R2 AD FS 场中](migrate-ad-fs-fed-server-r2.md#import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm)  
  
##  <a name="export-and-backup-the-ad-fs-configuration-data"></a>导出并备份 AD FS 配置数据  
 若要导出 AD FS 配置设置，请执行以下过程：  
  
###  <a name="to-export-service-settings"></a>导出服务设置  
  
1.  确保你有权访问 .pfx 文件中的以下证书及其私钥：  
  
    -   要迁移的联合服务器场使用的 SSL 证书  
  
    -   要迁移的联合服务器场使用的服务通信证书（如果该证书不同于 SSL 证书）  
  
    -   要迁移的联合服务器场使用的所有第三方令牌签名或令牌加密/解密证书  
  
若要查找 SSL 证书，请打开 Internet Information Services （IIS）管理控制台，在左窗格中选择 "**默认**网站"，然后单击 "**绑定 ...** " 在 "**操作**" 窗格中，查找并选择 https 绑定，单击 "**编辑**"，然后单击 "**查看**"。  
  
必须将联合身份验证服务使用的 SSL 证书及其私钥导出到 .pfx 文件。 有关详细信息，请参阅 [导出服务器身份验证证书的私钥部分](export-the-private-key-portion-of-a-server-authentication-certificate.md)。  
  
> [!NOTE]
>  如果你计划在 Windows Server 2012 R2 中运行 AD FS 的过程中部署设备注册服务，则必须获取新的 SSL 证书。有关详细信息，请参阅[适用于 AD FS 注册 SSL 证书](enroll-an-ssl-certificate-for-ad-fs.md)并[使用 Device Registration Service 配置联合身份验证服务器](configure-a-federation-server-with-device-registration-service.md)。  
  
若要查看使用的令牌签名、令牌解密和服务通信证书，请运行以下 Windows PowerShell 命令，以便在文件中创建使用的所有证书的列表：  
  
``` powershell
Get-ADFSCertificate | Out-File “.\certificates.txt”  
 ```  
  
2. 将 AD FS 联合身份验证服务属性（例如联合身份验证服务名称、联合身份验证服务显示名称和联合服务器标识符）导出到文件中。  
  
若要导出联合身份验证服务属性，请打开 Windows PowerShell 并运行以下命令： 

``` powershell
Get-ADFSProperties | Out-File “.\properties.txt”`.  
``` 

输出文件将包含以下重要配置值：  
 
|**Set-adfsproperties 报告的联合身份验证服务属性名称**|**AD FS 管理控制台中联合身份验证服务属性名称**|
|-----|-----|  
|HostName|联合身份验证服务名称|  
|标识符|联合身份验证服务标识符|  
|DisplayName|联合身份验证服务显示名称|  
  
3. 备份应用程序配置文件。 除了其他设置以外，此文件还包含策略数据库连接字符串。  
  
若要备份应用程序配置文件，必须手动将 `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` 文件复制到备份服务器上的安全位置。  
  
> [!NOTE]
>  记下此文件中紧接在“policystore connectionstring=”后面的数据库连接字符串。 如果该连接字符串指定了 SQL Server 数据库，则在联合服务器上还原原始 AD FS 配置时需要使用该值。  
>   
>  以下是 WID 连接字符串的示例：`“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`。 以下是 SQL Server 连接字符串的示例：`"Data Source=databasehostname;Integrated Security=True"`。  
  
4. 记录 AD FS 联合身份验证服务帐户的标识以及此帐户的密码。  
  
若要查找标识值，请在“服务” 控制台中查看“AD FS 2.0 Windows 服务” 的“登录为” 列并手动记录此值。  
  
> [!NOTE]
>  对于独立的联合身份验证服务，将使用内置的 NETWORK SERVICE 帐户。  在此情况下，你不需要使用密码。  
  
5. 将已启用的 AD FS 终结点列表导出到文件中。  
  
为此，请打开 Windows PowerShell 并运行以下命令： 

``` powershell
Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
``` 

6. 将所有自定义声明说明导出到文件中。  
  
为此，请打开 Windows PowerShell 并运行以下命令： 

``` powershell
Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
 ```

7. 如果你在 web.config 中配置了 useRelayStateForIdpInitiatedSignOn 等自定义设置，请确保备份 web.config 文件以供参考。 在 IIS 中，你可以复制映射到虚拟路径 **“/adfs/ls”** 的目录中的文件。 默认情况下，它位于 **%systemdrive%\inetpub\adfs\ls** 目录中。  
  
###  <a name="to-export-claims-provider-trusts-and-relying-party-trusts"></a>导出声明提供程序信任和信赖方信任  
  
1.  若要导出 AD FS 声明提供方信任和信赖方信任，你必须以管理员身份（但不能以域管理员身份）登录联合服务器，并运行位于**media/server_support 中的以下 Windows PowerShell 脚本**Windows Server 2012 R2 安装 CD 的/adfs 文件夹： `export-federationconfiguration.ps1`。  
  
> [!IMPORTANT]
>  导出脚本使用以下参数：  
> 
> - Export-federationconfiguration.ps1-Path < string @ no__t-0 [-ComputerName < string @ no__t-1] [-Credential < pscredential @ no__t] [-Force] [-CertificatePassword < securestring @ no__t]  
>   -   Export-federationconfiguration.ps1-Path < string @ no__t-0 [-ComputerName < string @ no__t-1] [-Credential < pscredential @ no__t-2] [-Force] [-CertificatePassword < securestring @ no__t-3] [-RelyingPartyTrustIdentifier < string []>] [-ClaimsProviderTrustIdentifier < string [] >]  
>   -   Export-federationconfiguration.ps1-Path < string @ no__t-0 [-ComputerName < string @ no__t-1] [-Credential < pscredential @ no__t-2] [-Force] [-CertificatePassword < securestring @ no__t-3] [-RelyingPartyTrustName < string [] >] [-ClaimsProviderTrustName < string [] >]  
> 
>   **-RelyingPartyTrustIdentifier <string[]>** — cmdlet 只会导出字符串数组中已指定了标识符的信赖方信任。 默认设置为不导出任何信赖方信任。 如果 RelyingPartyTrustIdentifier、ClaimsProviderTrustIdentifier、RelyingPartyTrustName 和 ClaimsProviderTrustName 都未指定，则脚本将导出所有信赖方信任和声明提供程序信任。  
> 
>   **-ClaimsProviderTrustIdentifier <string[]>** — cmdlet 只会导出字符串数组中已指定了标识符的声明提供程序信任。 默认设置为不导出任何声明提供程序信任。  
> 
>   **-RelyingPartyTrustName <string[]>** — cmdlet 只会导出字符串数组中已指定了名称的信赖方信任。 默认设置为不导出任何信赖方信任。  
> 
>   **-ClaimsProviderTrustName <string[]>** — cmdlet 只会导出字符串数组中已指定了名称的声明提供程序信任。 默认设置为不导出任何声明提供程序信任。  
> 
>   **-Path < 字符串\>**  -包含导出文件的文件夹的路径。  
> 
>   **-ComputerName < 字符串\>**  -指定 STS 服务器主机名。 默认值为本地计算机。 如果要将 Windows Server 2012 中的 AD FS 2.0 或 AD FS 迁移到 Windows Server 2012 R2 中的 AD FS，则为原有 AD FS 服务器的主机名。  
> 
>   **-Credential < PSCredential\>**  -指定有权执行此操作的用户帐户。 默认为当前用户。  
> 
>   **-Force** – 指定不提示用户进行确认。  
> 
>   **-CertificatePassword < SecureString\>**  -指定用于导出 AD FS 证书私钥的密码。 如果不指定此项，则需要导出带有私钥的 AD FS 证书时，脚本将提示用户提供密码。  
> 
>   **Inputs**：无  
> 
>   **Outputs**：字符串 - 此 cmdlet 将返回导出文件夹路径。 可以通过管道将返回的对象传递给 Import-FederationConfiguration。  
  
###  <a name="to-back-up-custom-attribute-stores"></a>备份自定义特性存储  
  
1.  你必须手动导出要在 Windows Server 2012 R2 的新 AD FS 场中保留的所有自定义属性存储。  
  
> [!NOTE]
>  在 Windows Server 2012 R2 中，AD FS 需要基于 .NET Framework 4.0 或更高版本的自定义属性存储。 按照 [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653) 中的说明安装和设置 .Net Framework 4.5。  
  
可以通过运行以下 Windows PowerShell 命令来查找 AD FS 所用的自定义特性存储的相关信息： 

``` powershell
Get-ADFSAttributeStore
```  

升级或迁移自定义特性存储的步骤将有所不同。  
  
2. 还必须手动导出要保留在 Windows Server 2012 R2 的新 AD FS 场中的自定义属性存储的所有 .dll 文件。 升级或迁移自定义特性存储的 .dll 文件的步骤将有所不同。  
  
##  <a name="create-a-windows-server-2012-r2-federation-server-farm"></a>创建 Windows Server 2012 R2 联合服务器场  
  
1.  在要用作联合服务器的计算机上安装 Windows Server 2012 R2 操作系统，然后添加 AD FS 服务器角色。 有关详细信息，请参阅 [Install the AD FS Role Service](install-the-ad-fs-role-service.md)。 然后，通过 Active Directory 联合身份验证服务配置向导或 Windows PowerShell 配置新的联合身份验证服务。 有关详细信息，请参阅[配置联合服务器](configure-a-federation-server.md)中的“配置新的联合服务器场中的第一个联合服务器”。  

在完成此步骤时，必须遵照以下说明：  
  
-   只有具有域管理员权限才能配置联合身份验证服务。  
  
-   使用的联合身份验证服务名称（场名称）必须与 Windows Server 2012 中的 AD FS 2.0 或 AD FS 内使用的名称相同。 如果不使用相同的联合身份验证服务名称，你备份的证书将无法在你尝试配置的 Windows Server 2012 R2 联合身份验证服务中正常运行。  
  
-   请指定这是 WID 还是 SQL Server 联合服务器场。 如果是 SQL 场，请指定 SQL Server 数据库位置和实例名称。  
  
-   必须提供一个 pfx 文件，其中包含你在准备 AD FS 迁移的过程中备份的 SSL 服务器身份验证证书。  
  
-   指定的服务帐户标识必须与 Windows Server 2012 场中的 AD FS 2.0 或 AD FS 内使用的标识相同。  
  
2. 配置初始节点后，可以将更多节点添加到新场中。 有关详细信息，请参阅[配置联合服务器](configure-a-federation-server.md)中的“向现有的联合服务器场中添加联合服务器”。  
  
##  <a name="import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm"></a>将原始配置数据导入 Windows Server 2012 R2 AD FS 场  
 现在，你已在 Windows Server 2012 R2 中运行 AD FS 联合服务器场，你可以将原始 AD FS 配置数据导入其中。  
  
1.  导入并配置其他自定义 AD FS 证书，包括外部注册的令牌签名和令牌解密/加密证书，以及服务通信证书（如果不同于 SSL 证书）。  
  
在 AD FS 管理控制台中选择“证书”。 验证服务通信、令牌加密/解密和令牌签名证书，方法是根据你在准备迁移时导出到 certificates.txt 文件中的值检查每个证书。  
  
若要将令牌解密或令牌签名证书从默认的自签名证书更改为外部证书，必须先禁用按默认启用的自动证书滚动功能。 为此，可以使用以下 Windows PowerShell 命令：  
  
``` powershell 
Set-ADFSProperties –AutoCertificateRollover $false  
```  
  
2. 使用 Set-AdfsProperties cmdlet 来配置任何自定义 AD FS 服务设置，例如 AutoCertificateRollover 或 SSO 生存期。  
  
3. 要导入 AD FS 信赖方信任和声明提供程序信任，你必须以管理员身份（但不能以域管理员身份）登录联合服务器，并运行位于 \support\adfs 文件夹中的以下 Windows PowerShell 脚本Windows Server 2012 R2 安装 CD：  
  
``` powershell 
import-federationconfiguration.ps1  
```  
  
> [!IMPORTANT]
>  导入脚本使用以下参数：  
> 
> - Import-federationconfiguration.ps1-Path < string @ no__t-0 [-ComputerName < string @ no__t-1] [-Credential < pscredential @ no__t-2] [-Force] [-LogPath < string @ no__t-3] [-CertificatePassword < securestring @ no__t-4]  
>   -   Import-federationconfiguration.ps1-Path < string @ no__t-0 [-ComputerName < string @ no__t-1] [-Credential < pscredential @ no__t-2] [-Force] [-LogPath < string @ no__t-3] [-CertificatePassword < securestring @ no__t-4] [-RelyingPartyTrustIdentifier < string [] >] [-ClaimsProviderTrustIdentifier < string [] >  
>   -   Import-federationconfiguration.ps1-Path < string @ no__t-0 [-ComputerName < string @ no__t-1] [-Credential < pscredential @ no__t-2] [-Force] [-LogPath < string @ no__t-3] [-CertificatePassword < securestring @ no__t-4] [-RelyingPartyTrustName < string [] >] [-ClaimsProviderTrustName < string [] >]  
> 
>   **-RelyingPartyTrustIdentifier <string[]>** — cmdlet 只会导入字符串数组中已指定了标识符的信赖方信任。 默认设置为不导入任何信赖方信任。 如果 RelyingPartyTrustIdentifier、ClaimsProviderTrustIdentifier、RelyingPartyTrustName 和 ClaimsProviderTrustName 都未指定，则脚本将导入所有信赖方信任和声明提供程序信任。  
> 
>   **-ClaimsProviderTrustIdentifier <string[]>** — cmdlet 只会导入字符串数组中已指定了标识符的声明提供程序信任。 默认设置为不导入任何声明提供程序信任。  
> 
>   **-RelyingPartyTrustName <string[]>** — cmdlet 只会导入字符串数组中已指定了名称的信赖方信任。 默认设置为不导入任何信赖方信任。  
> 
>   **-ClaimsProviderTrustName <string[]>** — cmdlet 只会导入字符串数组中已指定了名称的声明提供程序信任。 默认设置为不导入任何声明提供程序信任。  
> 
>   **-Path < 字符串\>**  -包含要导入的配置文件的文件夹的路径。  
> 
>   **-LogPath < 字符串\>**  -将包含导入日志文件的文件夹的路径。 将在此文件夹中创建名为“import.log”的日志文件。  
> 
>   **-ComputerName < 字符串\>**  -指定 STS 服务器的主机名。 默认值为本地计算机。 如果要将 Windows Server 2012 中的 AD FS 2.0 或 AD FS 迁移到 Windows Server 2012 R2 中的 AD FS，应将此参数设置为原有 AD FS 服务器的主机名。  
> 
>   **-Credential < PSCredential\>** -指定有权执行此操作的用户帐户。 默认为当前用户。  
> 
>   **-Force** – 指定不提示用户进行确认。  
> 
>   **-CertificatePassword < SecureString\>**  -指定用于导入 AD FS 证书私钥的密码。 如果不指定此项，则需要导入带有私钥的 AD FS 证书时，脚本将提示用户提供密码。  
> 
>   **Inputs：** 字符串 - 此命令使用导入文件夹路径作为输入。 可以通过管道将 Export-FederationConfiguration 传递到此命令。  
> 
>   **输出：** 无。  
  
在信赖方信任的 WSFedEndpoint 属性中使用任何尾随空格都可能会导致导入脚本出错。 对于这种情况，请在导入之前从文件中手动删除这些空格。 例如，下列条目会导致错误：  
  
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
>  如果针对源系统中的 Active Directory 声明提供程序信任使用了任何自定义声明规则（除 AD FS 默认规则以外的规则），脚本将不会迁移这些规则。 这是因为 Windows Server 2012 R2 具有新的默认值。 所有自定义规则必须通过手动添加到新的 Windows Server 2012 R2 场中的 Active Directory 声明提供方信任中进行合并。  
  
4. 配置所有自定义 AD FS 终结点设置。 在 AD FS 管理控制台中选择“终结点”。 根据你在准备 AD FS 迁移时导出到文件中的已启用 AD FS 终结点列表检查已启用的 AD FS 终结点。  
  
    \-与  
  
    配置所有自定义声明说明。 在 AD FS 管理控制台中选择“声明说明”。 针对你在准备 AD FS 迁移时导出到文件中的声明说明列表，检查 AD FS 声明说明的列表。 在 AD FS 中添加包含在文件中，但未包含在默认列表中的所有自定义声明说明。 请注意，管理控制台中的 Claim 标识符将映射到文件中的 ClaimType。  
  
5. 安装并配置所有已备份的自定义特性存储。 在更新 AD FS 配置之前，管理员应确保将所有自定义特性存储二进制文件升级到 .NET Framework 4.0 或更高版本，这样才能指向这些二进制文件。  
  
6. 配置要映射到原有 web.config 文件参数的服务属性。  
  
   -   如果已将**了 userelaystateforidpinitiatedsignon**添加到 windows server 2012 场中 AD FS 2.0 或 AD FS 中的**web.config**文件，则必须在 windows server 2012 R2 场中的 AD FS 上配置以下服务属性：  
  
       -   Windows Server 2012 R2 中的 AD FS 包含 **%systemroot%\ADFS\Microsoft.IdentityServer.Servicehost.exe.config**文件。 使用与**web.config 文件元素相同的语法**创建一个元素： `<useRelayStateForIdpInitiatedSignOn enabled="true" />`。 将此元素作为 **> identityserver**文件的 < 的一部分包含在**identityserver**文件的一部分中。  
  
   -   如果 **< persistIdentityProviderInformation enabled = "true&#124;false" lifetimeInDays = "90" enablewhrPersistence = "true&#124;false"/\>** 已添加到 AD FS 2.0**中的 web.config**文件或 Windows Server 中的 AD FS2012场，你必须在 Windows Server 2012 R2 场中的 AD FS 中配置以下服务属性：  
  
       1.  在 Windows Server 2012 R2 的 AD FS 中，运行以下 Windows PowerShell 命令： `Set-AdfsWebConfig –HRDCookieEnabled –HRDCookieLifetime`。  
  
   -   如果 **< 单一登录 enabled = "true&#124;false"/\>** 已添加到 AD FS 2.0 中的 web.config 文件或 windows server 2012 场中的 AD FS **，则无**AD FS 需在 windows server 2012R2 场。 默认情况下，在 Windows Server 2012 R2 场内 AD FS 中启用了单一登录。  
  
   -   如果已将 localAuthenticationTypes 设置添加到 Windows Server 2012 场中 AD FS 2.0 或 AD FS 中的**web.config**文件，则必须在 windows Server 2012 R2 场中的 AD FS 上配置以下服务属性：  
  
       -   Windows Server 2012 R2 中的 "集成"、"表单"、"TlsClient"、"基本" 转换列表与等效 AD FS 具有全局身份验证策略设置，用于支持联合身份验证服务和代理身份验证类型。 可以在“身份验证策略”下的“管理”管理单元中的 AD FS 内配置这些设置。  
  
   导入原始配置数据后，可以根据需要自定义 AD FS 登录页。 有关详细信息，请参阅 [Customizing the AD FS Sign-in Pages](../operations/AD-FS-Customization-in-Windows-Server-2016.md)。  
  
## <a name="next-steps"></a>后续步骤
 [将 Active Directory 联合身份验证服务角色服务迁移到 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [正在准备迁移 AD FS 联合服务器](prepare-migrate-ad-fs-server-r2.md)   
 [迁移 AD FS 联合服务器代理](migrate-fed-server-proxy-r2.md)   
 [验证 AD FS 迁移到 Windows Server 2012 R2](verify-ad-fs-migration.md)
---
title: 准备迁移独立的 AD FS 联合服务器
description: 提供有关准备将独立 AD FS 服务器迁移到 Windows Server 2012 的信息。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 09b8cbd9097a95cd00b1413ce9e32ff9bf2f44c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359325"
---
#  <a name="prepare-to-migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>准备迁移独立 AD FS 联合服务器或单节点 AD FS 场  
 
若要准备将独立 AD FS 2.0 联合服务器或单节点 AD FS 场迁移到 Windows Server 2012，则必须从该服务器导出并备份 AD FS 配置数据。  
  
若要导出 AD FS 配置数据，请执行以下任务：  
  
-   [步骤 1：导出服务设置 @ no__t-0  
  
-   [步骤 2：导出声明提供方信任 @ no__t-0  
  
-   [步骤 3：导出信赖方信任 @ no__t-0  
  
-   [步骤 4：备份自定义属性存储 @ no__t-0  
  
-   [步骤 5：备份网页自定义项 @ no__t-0  
  
## <a name="step-1-export-service-settings"></a>第 1 步：导出服务设置  
 若要导出服务设置，请执行以下过程：  
  
### <a name="to-export-service-settings"></a>导出服务设置  
  
1.  记录由联合身份验证服务使用的 SSL 证书的证书使用者名称和指纹值。 若要查找 SSL 证书，请打开 Internet Information Services （IIS）管理控制台，在左窗格中选择 "**默认**网站"，然后单击 "**绑定 ...** " 在 "**操作**" 窗格中，查找并选择 https 绑定，单击 "**编辑**"，然后单击 "**查看**"。  
  
> [!NOTE]
>  或者，你也可以将联合身份验证服务使用的 SSL 证书及其私钥导出到 .pfx 文件。 有关详细信息，请参阅 [导出服务器身份验证证书的私钥部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)。  
>   
>  导出 SSL 证书是可选的，因为此证书存储在本地计算机个人证书存储中，并且在操作系统升级过程中保留。  
  
2. 记录 AD FS 服务通信的配置、令牌解密和令牌签名证书。  若要查看使用的所有证书，请打开 Windows PowerShell 并运行以下命令，将 AD FS cmdlet 添加到你的 Windows PowerShell 会话： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后运行以下命令，在文件中创建所有使用中证书的列表 `PSH:>Get-ADFSCertificate | Out-File “.\certificates.txt”`。  
  
> [!NOTE]
>  或者，除了所有自签名证书以外，你还可以导出任何令牌签名、令牌加密或服务通信证书和非内部生成的密钥。 你可以通过使用 Windows PowerShell 查看在你的服务器上使用的所有证书。 打开 Windows PowerShell 并运行以下命令，将 AD FS cmdlet 添加到你的 Windows PowerShell 会话： `PSH:>add-pssnapin “Microsoft.adfs.powershell`。 然后运行以下命令以查看你的服务器正在使用的所有证书 `PSH:>Get-ADFSCertificate`。 此命令的输出包括指定每个证书的存储位置的 StoreLocation 和 StoreName 值。 然后，你可以使用[导出服务器身份验证证书的私钥部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)中的指南，将每个证书及其私钥导出到 .pfx 文件。  
>   
>  导出这些证书是可选的，因为在操作系统升级过程中将保留所有外部证书。  
  
3. 将 AD FS 2.0 联合身份验证服务属性（例如联合身份验证服务名称、联合身份验证服务显示名称和联合服务器标识符）导出到文件。  
  
若要导出联合身份验证服务属性，请打开 Windows PowerShell 并运行以下命令，将 AD FS cmdlet 添加到你的 Windows PowerShell 会话： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后运行以下命令以导出联合身份验证服务属性：`PSH:> Get-ADFSProperties | Out-File “.\properties.txt”`。  
  
输出文件将包含以下重要配置值：  
  
    
|**Set-adfsproperties 报告的联合身份验证服务属性名称**|**AD FS 管理控制台中联合身份验证服务属性名称**|
|------|------|
|HostName|联合身份验证服务名称|  
|标识符|联合身份验证服务标识符|  
|DisplayName|联合身份验证服务显示名称|  
  
4. 备份应用程序配置文件。 除了其他设置以外，此文件还包含策略数据库连接字符串。  
  
若要备份应用程序配置文件，必须手动将 `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` 文件复制到备份服务器上的安全位置。  
  
> [!NOTE]
>  记下此文件中紧接在“policystore connectionstring=”后面的数据库连接字符串。 如果该连接字符串指定了 SQL Server 数据库，则在联合服务器上还原原始 AD FS 配置时需要使用该值。  
>   
>  以下是 WID 连接字符串的示例：`“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`。 以下是 SQL Server 连接字符串的示例：`"Data Source=databasehostname;Integrated Security=True"`。  
  
5. 记录 AD FS 2.0 联合身份验证服务帐户的标识以及此帐户的密码。  
  
若要查找标识值，请在“服务” 控制台中查看“AD FS 2.0 Windows 服务” 的“登录为” 列并手动记录此值。  
  
> [!NOTE]
>  对于独立的联合身份验证服务，将使用内置的 NETWORK SERVICE 帐户。  在此情况下，你不需要使用密码。  
  
6. 将已启用的 AD FS 终结点列表导出到文件中。  
  
为此，请打开 Windows PowerShell 并运行以下命令，将 AD FS cmdlet 添加到你的 Windows PowerShell 会话： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后运行以下命令，将启用的 AD FS 终结点的列表导出到文件： `PSH:> Get-ADFSEndpoint | Out-File “.\endpoints.txt”`。  
  
7. 将所有自定义声明说明导出到文件中。  
  
为此，请打开 Windows PowerShell 并运行以下命令，将 AD FS cmdlet 添加到你的 Windows PowerShell 会话： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后运行以下命令将任何自定义声明说明导出到文件：`Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`。  
  
##  <a name="step-2-export-claims-provider-trusts"></a>步骤 2：导出声明提供方信任  
 若要导出声明提供方信任，请执行以下过程：  
  
### <a name="to-export-claims-provider-trusts"></a>导出声明提供程序信任  
  
1.  你可以使用 Windows PowerShell 导出所有声明提供方信任。 打开 Windows PowerShell 并运行以下命令，将 AD FS cmdlet 添加到你的 Windows PowerShell 会话： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后运行以下命令以导出所有声明提供方信任： `PSH:>Get-ADFSClaimsProviderTrust | Out-File “.\cptrusts.txt”`。  
  
## <a name="step-3-export-relying-party-trusts"></a>步骤 3:导出信赖方信任  
 若要导出信赖方信任，请执行以下过程：  
  
### <a name="to-export-relying-party-trusts"></a>导出信赖方信任  
  
1.  若要导出所有信赖方信任，请打开 Windows PowerShell 并运行以下命令，将 AD FS cmdlet 添加到你的 Windows PowerShell 会话： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后运行以下命令以导出所有信赖方信任：`PSH:>Get-ADFSRelyingPartyTrust | Out-File “.\rptrusts.txt”`。  
  
## <a name="step-4-back-up-custom-attribute-stores"></a>步骤 4：备份自定义属性存储  
 通过使用 Windows PowerShell，你可以由 AD FS 找到有关使用中的自定义属性存储的信息。 打开 Windows PowerShell 并运行以下命令，将 AD FS cmdlet 添加到你的 Windows PowerShell 会话： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后运行以下命令以查找有关自定义属性存储的信息： `PSH:>Get-ADFSAttributeStore`。 升级或迁移自定义特性存储的步骤将有所不同。  
  
## <a name="step-5-back-up-webpage-customizations"></a>步骤 5：备份网页自定义项  
 若要备份任何网页自定义项，请从映射到 IIS 中的虚拟路径 **“/adfs/ls”** 的目录复制 AD FS 网页和 **web.config** 文件。 默认情况下，它位于 **%systemdrive%\inetpub\adfs\ls** 目录中。  

## <a name="next-steps"></a>后续步骤
 [准备将 AD FS 2.0 联合服务器迁移](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移 AD FS 2.0 联合服务器代理](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [将 AD FS 2.0 联合服务器迁移](migrate-the-ad-fs-fed-server.md)   
 [迁移 AD FS 2.0 联合服务器代理](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移 AD FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)
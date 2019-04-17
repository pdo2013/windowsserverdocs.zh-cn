---
title: "准备迁移独立广告 FS 联盟服务器"
description: "在准备好将独立广告 FS 服务器迁移到 Windows Server 2012 上提供的信息。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0c1fd2bc1026d9aee25c591cf5c91a1c59f66ee0
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
#  <a name="prepare-to-migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>准备迁移独立广告 FS 联盟服务器或单节点广告 FS 电场的日落  
 
准备于 Windows Server 2012 迁移独立广告 FS 2.0 联盟服务器或单节点广告 FS 电场的日落（同一个 server 迁移），必须导出并且从此服务器备份广告 FS 配置数据。  
  
要导出的广告 FS 配置数据，请执行以下任务：  
  
-   [第 1 步：导出服务设置](#step-1-export-service-settings)  
  
-   [第 2 步：导出索赔提供商信任](#step-2-export-claims-provider-trusts)  
  
-   [第 3 步：导出信赖方信任](#step-3-export-relying-party-trusts)  
  
-   [第 4 步：备份自定义特性官方商城](#step-4-back-up-custom-attribute-stores)  
  
-   [第 5 步：备份网页自定义设置](#step-5-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>第 1 步：导出服务设置  
 若要导出服务设置，请执行以下步骤：  
  
### <a name="to-export-service-settings"></a>若要导出服务设置  
  
1.  录制证书主题名称和指纹的值 SSL 证书联合身份验证服务使用。 若要查找 SSL 证书，请打开 Internet 信息服务 (IIS) 管理控制台中，选择**默认网站**在左侧窗格中，单击**绑定…** 在**操作**窗格中，查找并选择 https 绑定、单击**编辑**，然后单击**视图**。  
  
> [!NOTE]
>  你还可以导出 SSL 证书联合身份验证服务和.pfx 文件其专用关键使用。 有关详细信息，请参阅[导出服务器身份验证证书专用键一部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)。  
>   
>  导出 SSL 证书是可选的因为此证书存储在本地计算机个人证书的应用商店，并在操作系统升级保留。  
  
2.  录制配置的广告 FS 服务通信，令牌解密和令牌签名证书。  若要查看所有证书的使用，请打开 Windows PowerShell，并运行以下命令以添加到你的 Windows PowerShell 会话广告 FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后，运行以下命令以创建文件中使用的所有证书列表 `PSH:>Get-ADFSCertificate | Out-File “.\certificates.txt”`  
  
> [!NOTE]
>  （可选）你还可以导出的任何令牌签名、令牌加密或服务通信证书和按键，没有内部生成，除了所有自签名证书。 你可以查看所有正在使用服务器通过使用 Windows PowerShell 的证书。 打开 Windows PowerShell 并运行以下命令以添加到你的 Windows PowerShell 会话广告 FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell`。 然后，运行以下命令以查看你的服务器正在使用的所有证书`PSH:>Get-ADFSCertificate`。 此命令的输出包括 StoreLocation 和 StoreName 指定的每个证书官方商城位置的值。 然后，你可以使用本指南中的[导出服务器身份验证证书专用键一部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)每个证书和及其专用键导出到一个.pfx 文件。  
>   
>  将这些证书导出是可选的因为操作系统升级过程中保留所有外部证书。  
  
3.  导出广告 FS 2.0 联合身份验证服务属性，如联合身份验证服务名称、联合身份验证服务显示到文件的名称和联合身份验证的服务器标识符。  
  
若要导出联合身份验证服务属性，请打开 Windows PowerShell，并运行以下命令以添加到你的 Windows PowerShell 会话广告 FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后，运行以下命令以导出联合身份验证服务属性：`PSH:> Get-ADFSProperties | Out-File “.\properties.txt”`。  
  
输出文件包含以下重要配置值：  
  
    
|**联合身份验证服务属性名称 Get-ADFSProperties 由报告**|**广告 FS 管理控制台在联合身份验证服务属性名称**|
|------|------|
|主机|联合身份验证服务名称|  
|标识符|联合身份验证服务的标识符|  
|显示名称|联合身份验证服务显示名称|  
  
4.  备份应用程序的配置文件。 其他设置，此文件包含策略数据库连接字符串。  
  
若要备份应用程序的配置文件，则必须手动复制`%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`为备份的服务器上的安全位置的文件。  
  
> [!NOTE]
>  在此文件，请记下的数据库连接字符串后立即位于"policystore 连接 =")。 如果连接字符串指定 SQL Server 数据库，在还原原始联合身份验证的服务器上的广告 FS 配置时需要值。  
>   
>  以下是 WID 连接字符串的一个示例：`“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`。 下面是它的 SQL Server 连接字符串一个示例：`"Data Source=databasehostname;Integrated Security=True"`。  
  
5.  录制广告 FS 2.0 联合身份验证服务帐户的身份，并且此帐户的密码。  
  
若要查找的身份值，检查**登录为**列**广告 FS 2.0 Windows 服务**中**服务**控制台和手动录制此值。  
  
> [!NOTE]
>  独立联合身份验证服务、使用内置的网络服务帐户。  在此情况下，你不需要有一个密码。  
  
6.  导出到一个文件的列表中启用广告 FS 端点。  
  
若要执行此操作，打开 Windows PowerShell，并运行以下命令以添加到你的 Windows PowerShell 会话广告 FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后，运行以下命令以导出到一个文件的列表中启用广告 FS 端点：`PSH:> Get-ADFSEndpoint | Out-File “.\endpoints.txt”`。  
  
7.  将任何自定义声明说明导出到一个文件。  
  
若要执行此操作，打开 Windows PowerShell，并运行以下命令以添加到你的 Windows PowerShell 会话广告 FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后，运行以下命令以导出到一个文件任何自定义声明说明：`Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`。  
  
##  <a name="step-2-export-claims-provider-trusts"></a>第 2 步：导出索赔提供商信任  
 若要导出的索赔提供商信任，请执行以下步骤：  
  
### <a name="to-export-claims-provider-trusts"></a>若要导出索赔提供商信任  
  
1.  你可以使用 Windows PowerShell 导出所有索赔提供商信任。 打开 Windows PowerShell 并运行以下命令以添加到你的 Windows PowerShell 会话广告 FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后运行以下命令以导出所有索赔提供商的信任：`PSH:>Get-ADFSClaimsProviderTrust | Out-File “.\cptrusts.txt”`。  
  
## <a name="step-3-export-relying-party-trusts"></a>第 3 步：导出信赖方信任  
 若要导出信赖方信任，请执行以下步骤：  
  
### <a name="to-export-relying-party-trusts"></a>若要导出信赖方信任  
  
1.  若要导出信赖所有方信任、打开 Windows PowerShell 和运行以下命令以添加到你的 Windows PowerShell 会话广告 FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后，运行以下命令以导出信赖所有方信任：`PSH:>Get-ADFSRelyingPartyTrust | Out-File “.\rptrusts.txt”`。  
  
## <a name="step-4-back-up-custom-attribute-stores"></a>第 4 步：备份自定义特性官方商城  
 你可以通过使用 Windows PowerShell 由广告 FS 使用找到有关自定义特性官方商城的信息。 打开 Windows PowerShell 并运行以下命令以添加到你的 Windows PowerShell 会话广告 FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后，运行以下命令以查找有关自定义特性官方商城的信息：`PSH:>Get-ADFSAttributeStore`。 升级或迁移自定义特性官方商城的步骤会有所不同。  
  
## <a name="step-5-back-up-webpage-customizations"></a>第 5 步：备份网页自定义设置  
 若要备份的所有网页自定义，复制广告 FS 网页和**web.config**文件的目录中映射到虚拟路径**"月 adfs 月 1！"** IIS 中。 默认情况下，它处于**%systemdrive%\inetpub\adfs\ls**目录。  

## <a name="next-steps"></a>后续步骤
 [准备迁移广告 FS 2.0 联盟服务器](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移广告 FS 2.0 联盟服务器代理服务器](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [迁移广告 FS 2.0 联盟服务器](migrate-the-ad-fs-fed-server.md)   
 [迁移广告 FS 2.0 联盟服务器代理服务器](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移广告 FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)
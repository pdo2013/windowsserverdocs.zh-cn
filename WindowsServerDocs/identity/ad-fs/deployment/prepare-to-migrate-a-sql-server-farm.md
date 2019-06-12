---
title: 准备迁移 AD FS SQL 场
description: 正在准备将 AD FS 服务器 SQL 场迁移到 Windows Server 2012 上提供的信息。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3f735c45582bc9d1746f18c0ac7c9888a4b3ac88
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445559"
---
# <a name="prepare-to-migrate-a-sql-server-farm"></a>准备迁移 SQL Server 场  
 若要准备将属于 Windows Server 2012 到 SQL Server 场的 AD FS 2.0 联合服务器迁移，必须导出并备份 AD FS 配置数据从这些服务器。  
  
 若要导出 AD FS 配置数据，请执行以下任务：  
  
-   [步骤 1：导出服务设置](#step-1-export-service-settings)  
  
-   [步骤 2：备份自定义属性存储](#step-2-back-up-custom-attribute-stores)  
  
-   [步骤 3：备份网页自定义项](#step-3-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>第 1 步：导出服务设置  
 若要导出服务设置，请执行以下过程：  
  
### <a name="to-export-service-settings"></a>导出服务设置  
  
1.  记录由联合身份验证服务使用的 SSL 证书的证书使用者名称和指纹值。 若要查找 SSL 证书，请打开 Internet 信息服务 (IIS) 管理控制台中，选择**Default Web Site**的左窗格中单击**绑定...** 在中**操作**窗格中，找到并选择 https 绑定中，单击**编辑**，然后单击**视图**。  
  
> [!NOTE]
>  或者，你也可以将 SSL 证书及其私钥导出到 .pfx 文件。 有关详细信息，请参阅 [导出服务器身份验证证书的私钥部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)。  
>   
>  此步骤是可选的，因为此证书存储在本地计算机个人证书存储中，并将在操作系统升级过程中保留。  
  
2. 导出任何其他令牌签名、令牌加密或服务通信证书以及不由 AD FS 内部生成的密钥。  
  
你可以通过使用 Windows PowerShell 查看在你的服务器上由 AD FS 使用的所有证书。 打开 Windows PowerShell 并运行以下命令，将 AD FS cmdlet 添加到你的 Windows PowerShell 会话： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后运行以下命令以查看你的服务器正在使用的所有证书 `PSH:>Get-ADFSCertificate`。 此命令的输出包括指定每个证书的存储位置的 StoreLocation 和 StoreName 值。  
  
> [!NOTE]
>  然后，你可以使用[导出服务器身份验证证书的私钥部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)中的指南将每个证书及其私钥导出到 .pfx 文件。 此步骤是可选的，因为在操作系统升级过程中将保留所有外部证书。  
  
3. 备份应用程序配置文件。 除了其他设置以外，此文件还包含策略数据库连接字符串。  
  
若要备份应用程序配置文件，必须手动将 `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` 文件复制到备份服务器上的安全位置。  
  
> [!NOTE]
>  请在以下文件中的“policystore connectionstring =”后记录 SQL Server 连接字符串：  `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`。 当你在联合服务器上还原原始 AD FS 配置时，需要此字符串。  
  
4. 记录 AD FS 2.0 联合身份验证服务帐户的标识以及此帐户的密码。  
  
若要查找标识值，请在“服务”  控制台中查看“AD FS 2.0 Windows 服务”  中的“登录为”  列，并手动记录此值。  
  
## <a name="step-2-back-up-custom-attribute-stores"></a>步骤 2：备份自定义属性存储  
 通过使用 Windows PowerShell，你可以由 AD FS 找到有关使用中的自定义属性存储的信息。 打开 Windows PowerShell 并运行以下命令，将 AD FS cmdlet 添加到你的 Windows PowerShell 会话： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后运行以下命令以查找有关自定义属性存储的信息： `PSH:>Get-ADFSAttributeStore`。 升级或迁移自定义特性存储的步骤将有所不同。  
  
## <a name="step-3-back-up-webpage-customizations"></a>步骤 3:备份网页自定义项  
 若要备份任何网页自定义项，请从映射到 IIS 中的虚拟路径 **“/adfs/ls”** 的目录复制 AD FS 网页和 **web.config** 文件。 默认情况下，它位于 **%systemdrive%\inetpub\adfs\ls** 目录中。  
  
## <a name="next-steps"></a>后续步骤
 [准备迁移 AD FS 2.0 联合服务器](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移 AD FS 2.0 联合服务器代理](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [迁移 AD FS 2.0 联合服务器](migrate-the-ad-fs-fed-server.md)   
 [迁移 AD FS 2.0 联合服务器代理](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移 AD FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)
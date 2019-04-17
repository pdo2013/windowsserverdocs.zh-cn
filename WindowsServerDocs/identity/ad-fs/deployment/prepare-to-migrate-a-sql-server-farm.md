---
title: "准备迁移广告 FS SQL 电场的日落"
description: "对 Windows Server 2012 的 SQL 场已准备好迁移广告 FS 服务器上提供的信息。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 284e02174b4a8c06f114640223d289dc63ea3a26
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="prepare-to-migrate-a-sql-server-farm"></a>准备迁移 SQL Server 电场的日落  
 准备迁移到 Windows Server 2012 的 SQL Server 场属于广告 FS 2.0 联盟服务器，必须导出并且备份广告 FS 配置数据，这些服务器从。  
  
 要导出的广告 FS 配置数据，请执行以下任务：  
  
-   [第 1 步：导出服务设置](#step-1-export-service-settings)  
  
-   [第 2 步：备份自定义特性官方商城](#step-2-back-up-custom-attribute-stores)  
  
-   [第 3 步：备份网页自定义设置](#step-3-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>第 1 步：导出服务设置  
 若要导出服务设置，请执行以下步骤：  
  
### <a name="to-export-service-settings"></a>若要导出服务设置  
  
1.  录制证书主题名称和指纹的值 SSL 证书联合身份验证服务使用。 若要查找 SSL 证书，打开 Internet 信息服务 (IIS) 管理控制台中，选择**默认网站**在左侧窗格中，单击**绑定…** 在**操作**窗格中，查找并选择 https 绑定、单击**编辑**，然后单击**视图**。  
  
> [!NOTE]
>  你还可以导出 SSL）证书，其专用关键.pfx 文件。 有关详细信息，请参阅[导出服务器身份验证证书专用键一部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)。  
>   
>  这一步是可选的这是因为该证书存储在本地计算机个人证书的应用商店，并将保留在操作系统升级。  
  
2.  导出令牌签名、令牌加密或服务通信证书和不由广告 FS 内部生成的键。  
  
你可以查看所有证书的使用 Windows PowerShell 是由广告 FS 服务器上使用。 打开 Windows PowerShell 并运行以下命令以添加到你的 Windows PowerShell 会话广告 FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后，运行以下命令以查看你的服务器正在使用的所有证书`PSH:>Get-ADFSCertificate`。 此命令的输出包括 StoreLocation 和 StoreName 指定的每个证书官方商城位置的值。  
  
> [!NOTE]
>  （可选），然后使用在指南[导出服务器身份验证证书专用键一部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)每个证书和及其专用键导出到一个.pfx 文件。 因为操作系统升级过程中保留所有外部证书，这一步是可选的。  
  
3.  备份应用程序的配置文件。 其他设置，此文件包含策略数据库连接字符串。  
  
若要备份应用程序的配置文件，则必须手动复制`%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`为备份的服务器上的安全位置的文件。  
  
> [!NOTE]
>  录制 SQL Server 连接字符串后的"policystore 连接 ="在以下文件：`%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`。 在还原原始联合身份验证的服务器上的广告 FS 配置时，你将需要此字符串。  
  
4.  录制广告 FS 2.0 联合身份验证服务帐户的身份，并且此帐户的密码。  
  
若要查找的身份值，检查**登录为**列**广告 FS 2.0 Windows 服务**中**服务**控制台和手动记录的值。  
  
## <a name="step-2-back-up-custom-attribute-stores"></a>第 2 步：备份自定义特性官方商城  
 你可以通过使用 Windows PowerShell 由广告 FS 使用找到有关自定义特性官方商城的信息。 打开 Windows PowerShell 并运行以下命令以添加到你的 Windows PowerShell 会话广告 FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后，运行以下命令以查找有关自定义特性官方商城的信息：`PSH:>Get-ADFSAttributeStore`。 升级或迁移自定义特性官方商城的步骤会有所不同。  
  
## <a name="step-3-back-up-webpage-customizations"></a>第 3 步：备份网页自定义设置  
 若要备份的所有网页自定义，复制广告 FS 网页和**web.config**文件的目录中映射到虚拟路径**"月 adfs 月 1！"** IIS 中。 默认情况下，它处于**%systemdrive%\inetpub\adfs\ls**目录。  
  
## <a name="next-steps"></a>后续步骤
 [准备迁移广告 FS 2.0 联盟服务器](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移广告 FS 2.0 联盟服务器代理服务器](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [迁移广告 FS 2.0 联盟服务器](migrate-the-ad-fs-fed-server.md)   
 [迁移广告 FS 2.0 联盟服务器代理服务器](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移广告 FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)
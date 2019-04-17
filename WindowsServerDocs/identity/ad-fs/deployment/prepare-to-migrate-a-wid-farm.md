---
title: "准备迁移广告 FS 2.0 WID 电场的日落"
description: "在准备好将广告 FS 2.0 服务器 WID 电场的日落迁移到 Windows Server 2012 上提供的信息。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4985a8d16614bd12bce991e196d105464d37634d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="prepare-to-migrate-an-ad-fs-20-wid-farm"></a>准备迁移广告 FS 2.0 WID 电场的日落  
 准备迁移到 Windows Server 2012 的 Windows 内部数据库 (WID) 场属于广告 FS 2.0 联盟服务器，必须导出并且备份广告 FS 配置数据，这些服务器从。  
  
 要导出的广告 FS 配置数据，请执行以下任务：  
  
-   [第 1 步:-导出服务设置](#step-1-export-service-settings)  
  
-   [第 2 步：备份自定义特性官方商城](#step-2-back-up-custom-attribute-stores)  
  
-   [第 3 步：备份网页自定义设置](#step-3-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>第 1 步：导出服务设置  
 若要导出服务设置，请执行以下步骤：  
  
### <a name="to-export-service-settings"></a>若要导出服务设置  
  
1.  录制证书主题名称和指纹的值 SSL 证书联合身份验证服务使用。 若要查找 SSL 证书，打开 Internet 信息服务 (IIS) 管理控制台中，选择**默认网站**在左侧窗格中，单击**绑定…** 在**操作**窗格中，查找并选择 https 绑定、单击**编辑**，然后单击**视图**。  
  
> [!NOTE]
>  （可选）你还可以导出 SSL 证书和其专用密钥到.pfx 文件。 有关详细信息，请参阅[导出服务器身份验证证书专用键一部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)。  
>   
>  这一步是可选的这是因为该证书存储在本地计算机个人证书的应用商店，并将保留在操作系统升级。  
  
2.  导出任何令牌签名、令牌加密或服务通信证书和按键，没有内部生成，除了自签名证书。  
  
你可以查看所有正在使用服务器通过使用 Windows PowerShell 的证书。 打开 Windows PowerShell 并运行以下命令以添加到你的 Windows PowerShell 会话广告 FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后，运行以下命令以查看你的服务器正在使用的所有证书`PSH:>Get-ADFSCertificate`。 此命令的输出包括 StoreLocation 和 StoreName 指定的每个证书官方商城位置的值。  然后，你可以使用本指南中的[导出服务器身份验证证书专用键一部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)每个证书和及其专用键导出到一个.pfx 文件。  
  
> [!NOTE]
>  因为操作系统升级过程中保留所有外部证书，这一步是可选的。  
  
3.  录制广告 FS 2.0 联合身份验证服务帐户的身份，并且此帐户的密码。  
  
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
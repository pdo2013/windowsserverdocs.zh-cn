---
title: "准备迁移广告 FS 2.0 联盟服务器代理服务器"
description: "在准备好将广告 FS 服务器代理迁移到 Windows Server 2012 上提供的信息。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f207993580e6fd06c9ff185e58e5b7e81af60252
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-proxy"></a>准备迁移广告 FS 2.0 联盟服务器代理服务器

准备将广告 FS 2.0 联盟服务器代理迁移到 Windows Server 2012，必须导出并从该服务器代理服务器备份广告 FS 配置数据。  本主题中的步骤适用于一台代理联合身份验证的服务器方案或多个代理联合身份验证的服务器。  
  
 要导出的广告 FS 配置数据，请执行以下任务：  
  
-   [第 1 步：导出代理服务设置](#step-1-export-proxy-service-settings)  
  
-   [第 2 步：备份网页自定义设置](#step-2-back-up-webpage-customizations)  
  
##  <a name="step-1-export-proxy-service-settings"></a>第 1 步：导出代理服务设置  
 若要导出联合身份验证的服务器代理服务设置，请执行以下步骤：  
  
### <a name="to-export-proxy-service-settings"></a>若要导出代理服务设置  
  
1.  将安全套接字层 (SSL) 证书，其专用键导出到一个.pfx 文件。 有关详细信息，请参阅[导出服务器身份验证证书专用键一部分](export-the-private-key-portion-of-a-server-authentication-certificate.md)。  
  
> [!NOTE]
>  这一步是可选的这是因为该证书操作系统升级过程中保留。  
  
2.  导出到一个文件广告 FS 2.0 联合身份验证的代理服务器属性。 你可以通过使用 Windows PowerShell 执行该操作。  
  
打开 Windows PowerShell 并运行以下命令以添加到你的 Windows PowerShell 会话广告 FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后，运行以下命令以导出到一个文件的联合身份验证的代理服务器属性：`PSH:> Get-ADFSProxyProperties | out-file “.\proxyproperties.txt”`。  
  
3.  确保你知道是广告 FS 联盟服务器任一管理员帐户或广告 FS 联合身份验证服务所运行的服务帐户的凭据。  此信息很代理信任安装所必需的。  
  
 完成此步骤导致收集以下配置你的广告 FS 联合身份验证的服务器代理所需的信息：  
  
-   广告 FS 联合身份验证服务名称  
  
-   代理信任安装时需要域帐户名称  
  
-   地址和（如果广告 FS 联合身份验证的服务器代理和广告 FS 联盟服务器之间 HTTP 代理）HTTP 代理服务器端口  
  
##  <a name="step-2-back-up-webpage-customizations"></a>第 2 步：备份网页自定义设置  
 若要备份网页自定义设置，复制广告 FS 代理网页和**web.config**文件的目录中映射到虚拟路径**"月 adfs 月 1！"** IIS 中。  默认情况下，它处于**%systemdrive%\inetpub\adfs\ls**目录。  
  
## <a name="next-steps"></a>后续步骤
 [准备迁移广告 FS 2.0 联盟服务器](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移广告 FS 2.0 联盟服务器代理服务器](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [迁移广告 FS 2.0 联盟服务器](migrate-the-ad-fs-fed-server.md)   
 [迁移广告 FS 2.0 联盟服务器代理服务器](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移广告 FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)
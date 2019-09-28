---
title: 准备迁移 AD FS 2.0 联合服务器代理
description: 提供有关准备将 AD FS 服务器代理迁移到 Windows Server 2012 的信息。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 20fbf3ea9231706635df2bd4c1d541fde0c1484b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408207"
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-proxy"></a>准备迁移 AD FS 2.0 联合服务器代理

要准备将 AD FS 2.0 联合服务器代理迁移到 Windows Server 2012，你必须从此服务器代理导出并备份 AD FS 的配置数据。  本主题中的步骤适用于具有一个代理联合服务器或多个代理联合服务器的方案。  
  
 若要导出 AD FS 配置数据，请执行以下任务：  
  
-   [步骤 1：导出代理服务设置 @ no__t-0  
  
-   [步骤 2：备份网页自定义项 @ no__t-0  
  
##  <a name="step-1-export-proxy-service-settings"></a>第 1 步：导出代理服务设置  
 若要导出联合服务器代理服务设置，请执行以下过程：  
  
### <a name="to-export-proxy-service-settings"></a>导出代理服务设置  
  
1.  将安全套接字层 (SSL) 证书及其私钥导出到 .pfx 文件。 有关详细信息，请参阅 [导出服务器身份验证证书的私钥部分](export-the-private-key-portion-of-a-server-authentication-certificate.md)。  
  
> [!NOTE]
>  此步骤是可选的，因为在操作系统升级过程中将保留此证书。  
  
2. 将 AD FS 2.0 联合代理属性导出到文件。 可以使用 Windows PowerShell 执行该操作。  
  
打开 Windows PowerShell 并运行以下命令，将 AD FS cmdlet 添加到你的 Windows PowerShell 会话： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后运行以下命令以将联合代理属性导出到文件： `PSH:> Get-ADFSProxyProperties | out-file “.\proxyproperties.txt”`。  
  
3. 确保你知道某个帐户的凭据，该帐户要么是 AD FS 联合服务器的管理员，要么是 AD FS 联合身份验证服务运行所使用的服务帐户。  代理信任设置需要此信息。  
  
   完成此步骤会收集配置 AD FS 联合服务器代理所需的以下信息：  
  
-   AD FS 联合身份验证服务名称  
  
-   代理信任设置需要的域帐户的名称  
  
-   HTTP 代理的地址和端口（如果 AD FS 联合服务器代理与 AD FS 联合服务器之间存在 HTTP 代理）  
  
##  <a name="step-2-back-up-webpage-customizations"></a>步骤 2：备份网页自定义项  
 若要备份网页自定义项，请从映射到 IIS 中的虚拟路径 **“/adfs/ls”** 的目录复制 AD FS 代理网页和 **web.config** 文件。  默认情况下，它位于 **%systemdrive%\inetpub\adfs\ls** 目录中。  
  
## <a name="next-steps"></a>后续步骤
 [准备将 AD FS 2.0 联合服务器迁移](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移 AD FS 2.0 联合服务器代理](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [将 AD FS 2.0 联合服务器迁移](migrate-the-ad-fs-fed-server.md)   
 [迁移 AD FS 2.0 联合服务器代理](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移 AD FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)
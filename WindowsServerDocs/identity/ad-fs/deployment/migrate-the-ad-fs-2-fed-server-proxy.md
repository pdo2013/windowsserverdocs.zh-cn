---
title: 迁移 AD FS 2.0 联合服务器代理
description: 提供有关将 AD FS 联合服务器代理迁移到 Windows Server 2012 的信息。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: eddb0d3432c69ecff4542ff1b8f2204b96ce0820
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445661"
---
# <a name="migrate-the-ad-fs-20-federation-server-proxy"></a>迁移 AD FS 2.0 联合服务器代理
本文档提供了详细的信息的 AD FS 2.0 联合身份验证代理服务器迁移到 Windows Server 2012。

## <a name="migrate-the-proxy"></a>迁移代理

若要将 AD FS 2.0 联合服务器代理迁移到 Windows Server 2012 中，执行以下过程：  
  
1.  每个计划将迁移到 Windows Server 2012 的联合身份验证服务器代理，查看并执行中的过程[Prepare to Migrate the AD FS 2.0 联合服务器代理](prepare-to-migrate-ad-fs-fed-proxy.md)。  
  
2.  从负载平衡器中删除联合服务器代理。  
  
3.  在此服务器从 Windows Server 2008 R2 或 Windows Server 2008 到 Windows Server 2012 中执行的操作系统的就地升级。 有关详细信息，请参阅[安装 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作为操作系统升级的结果，此服务器上的 AD FS 代理配置将丢失并且 AD FS 2.0 的服务器角色会被删除。 相反，安装 Windows Server 2012 AD FS 服务器角色，但未配置。 你必须手动创建原始 AD FS 代理配置并还原剩余的 AD FS 代理设置，以完成联合服务器代理迁移。  
  
4. 通过使用“AD FS 联合服务器代理配置向导” 创建原始 AD FS 代理配置。 有关详细信息，请参阅[将计算机配置为联合服务器代理角色](configure-a-computer-for-the-federation-server-proxy-role.md)。 执行该向导时，请按如下所示使用在“准备迁移 AD FS 2.0 联合服务器代理”中收集的信息：  
  
 
|**联合服务器代理向导输入选项**|**使用以下值**|
|-----|-----|  
|**联合身份验证服务名称**|输入 proxyproperties.txt 文件中的 BaseHostName 值|  
|“将请求发送到此联合身份验证服务时使用 HTTP 代理服务器”复选框|如果 proxyproperties.txt 文件包含 ForwardProxyUrl 属性的值，则选中此框|  
|**HTTP 代理服务器地址**|输入 proxyproperties.txt 文件中的 ForwardProxyUrl 值|  
|凭据提示|输入某个帐户的凭据，该帐户要么是 AD FS 联合服务器的管理员，要么是 AD FS 联合身份验证服务运行所使用的服务帐户。|  
  
5. 更新你在此服务器上的 AD FS 网页。 如果联合服务器代理准备迁移时备份你的自定义 AD FS 代理网页，使用你的备份数据来覆盖默认 AD FS 网页在默认情况下创建 **%systemdrive%\inetpub\adfs\ls** Windows Server 2012 中的 AD FS 代理配置结果目录。  
  
6. 将此服务器添加回负载均衡器。  
  
7. 如果有其他要迁移的 AD FS 2.0 联合服务器代理，请对剩下的联合服务器代理计算机重复步骤 2 到 6。  
  
  
## <a name="next-steps"></a>后续步骤
 [准备迁移 AD FS 2.0 联合服务器](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移 AD FS 2.0 联合服务器代理](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [迁移 AD FS 2.0 联合服务器](migrate-the-ad-fs-fed-server.md)   
 [迁移 AD FS 2.0 联合服务器代理](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移 AD FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)
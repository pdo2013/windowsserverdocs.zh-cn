---
title: 迁移 AD FS 2.0 联合服务器代理
description: 提供有关将 AD FS 联合服务器代理迁移到 Windows Server 2012 的信息。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e97a1b3b66d7c12c96570022b7e69caaf0e92f65
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408226"
---
# <a name="migrate-the-ad-fs-20-federation-server-proxy"></a>迁移 AD FS 2.0 联合服务器代理
本文档详细介绍了如何将 AD FS 2.0 联合代理服务器迁移到 Windows Server 2012。

## <a name="migrate-the-proxy"></a>迁移代理

若要将 AD FS 2.0 联合服务器代理迁移到 Windows Server 2012，请执行以下步骤：  
  
1.  对于计划迁移到 Windows Server 2012 的每个联合服务器代理，请查看并执行[准备迁移 AD FS 2.0 联合服务器代理](prepare-to-migrate-ad-fs-fed-proxy.md)中的过程。  
  
2.  从负载平衡器中删除联合服务器代理。  
  
3.  将此服务器上的操作系统从 Windows Server 2008 R2 或 Windows Server 2008 就地升级到 Windows Server 2012。 有关详细信息，请参阅[安装 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作为操作系统升级的结果，此服务器上的 AD FS 代理配置将丢失并且 AD FS 2.0 的服务器角色会被删除。 改为安装 Windows Server 2012 AD FS Server 角色，但未对其进行配置。 你必须手动创建原始 AD FS 代理配置并还原剩余的 AD FS 代理设置，以完成联合服务器代理迁移。  
  
4. 通过使用“AD FS 联合服务器代理配置向导”创建原始 AD FS 代理配置。 有关详细信息，请参阅[将计算机配置为联合服务器代理角色](configure-a-computer-for-the-federation-server-proxy-role.md)。 执行该向导时，请按如下所示使用在“准备迁移 AD FS 2.0 联合服务器代理”中收集的信息：  
  
 
|**联合服务器代理向导输入选项**|**使用以下值**|
|-----|-----|  
|**联合身份验证服务名称**|输入 proxyproperties.txt 文件中的 BaseHostName 值|  
|“将请求发送到此联合身份验证服务时使用 HTTP 代理服务器”复选框|如果 proxyproperties.txt 文件包含 ForwardProxyUrl 属性的值，则选中此框|  
|**HTTP 代理服务器地址**|输入 proxyproperties.txt 文件中的 ForwardProxyUrl 值|  
|凭据提示|输入某个帐户的凭据，该帐户要么是 AD FS 联合服务器的管理员，要么是 AD FS 联合身份验证服务运行所使用的服务帐户。|  
  
5. 更新你在此服务器上的 AD FS 网页。 如果在准备联合服务器代理以进行迁移时备份自定义 AD FS 代理网页，则使用备份数据来覆盖默认情况下在**位于%systemdrive%\inetpub\adfs\ls**目录中创建的 AD FS 网页作为 Windows Server 2012 中的 AD FS 代理配置的结果。  
  
6. 将此服务器添加回负载均衡器。  
  
7. 如果有其他要迁移的 AD FS 2.0 联合服务器代理，请对剩下的联合服务器代理计算机重复步骤 2 到 6。  
  
  
## <a name="next-steps"></a>后续步骤
 [准备将 AD FS 2.0 联合服务器迁移](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移 AD FS 2.0 联合服务器代理](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [将 AD FS 2.0 联合服务器迁移](migrate-the-ad-fs-fed-server.md)   
 [迁移 AD FS 2.0 联合服务器代理](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移 AD FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)
---
title: "迁移广告 FS 2.0 联盟服务器代理服务器"
description: "将广告 FS 联合身份验证的服务器代理迁移到 Windows Server 2012 上提供信息。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 98e28c9be808f63ed39a3ac24dd95014b388d001
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-the-ad-fs-20-federation-server-proxy"></a>迁移广告 FS 2.0 联盟服务器代理服务器
本文将广告 FS 2.0 联合身份验证的代理服务器迁移到 Windows Server 2012 上提供详细的信息。

## <a name="migrate-the-proxy"></a>迁移代理服务器

若要将广告 FS 2.0 联盟服务器代理迁移到 Windows Server 2012，请执行以下步骤：  
  
1.  为你计划迁移到 Windows Server 2012 每联盟服务器代理服务器，查看并执行中的步骤[到迁移广告 FS 准备 2.0 联合身份验证的服务器代理](prepare-to-migrate-ad-fs-fed-proxy.md)。  
  
2.  删除负载平衡联合身份验证的服务器代理。  
  
3.  执行此从 Windows Server 2008 R2 或 Windows Server 2008 于 Windows Server 2012 的服务器上的操作系统的就地升级。 有关详细信息，请参阅[安装 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作为操作系统升级的结果，此服务器上的广告 FS 代理配置失去并且删除广告 FS 2.0 server 角色。 而是安装了 Windows Server 2012 广告 FS server 角色，但它未配置。 必须手动创建原始广告 FS 代理配置并还原剩余广告 FS 代理服务器设置，以完成联合身份验证的服务器代理迁移。  
  
4.  通过创建原始广告 FS 代理配置**广告 FS 联盟服务器的代理配置向导**。 有关详细信息，请参阅[将计算机配置为联合身份验证的服务器代理角色](configure-a-computer-for-the-federation-server-proxy-role.md)。 当你执行该向导，请使用收集的信息您准备中迁移广告 FS 到 2.0 联合身份验证的服务器代理，如下所示：  
  
 
|**联合身份验证的代理服务器向导输入的选项**|**使用以下值**|
|-----|-----|  
|**联合身份验证服务名称**|从 proxyproperties.txt 文件输入 BaseHostName 值|  
|**发送到此联盟请求时，使用 HTTP 代理服务器**服务的复选框|如果你 proxyproperties.txt 文件中包含的值 ForwardProxyUrl 属性，请选中该框|  
|**HTTP 的代理服务器地址**|从 proxyproperties.txt 文件输入 ForwardProxyUrl 值|  
|凭据提示|输入凭据的广告 FS 联盟服务器任一管理员帐户或广告 FS 联合身份验证服务所运行的服务帐户。|  
  
5.  更新你的广告 FS 网页此服务器上。 如果准备迁移您联合身份验证的代理服务器服务器时备份你的自定义广告 FS 代理网页，使用你的备份数据覆盖默认广告 FS 网页中的默认情况下创建的**%systemdrive%\inetpub\adfs\ls**目录导致在 Windows Server 2012 的广告 FS 代理配置。  
  
6.  返回到负载平衡添加该服务器。  
  
7.  如果你拥有其他广告 FS 2.0 联合身份验证的服务器代理迁移，请重复步骤 2 到 6 其余联合身份验证的服务器代理计算机的。  
  
  
## <a name="next-steps"></a>后续步骤
 [准备迁移广告 FS 2.0 联盟服务器](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移广告 FS 2.0 联盟服务器代理服务器](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [迁移广告 FS 2.0 联盟服务器](migrate-the-ad-fs-fed-server.md)   
 [迁移广告 FS 2.0 联盟服务器代理服务器](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移广告 FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)
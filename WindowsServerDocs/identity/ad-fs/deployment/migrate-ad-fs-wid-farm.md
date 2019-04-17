---
title: "迁移广告 FS 2.0 联盟服务器 WID 电场的日落"
description: "将广告 FS 2.0 服务器 WID 电场的日落迁移到 Windows Server 2012 上提供信息"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dbef7d07041a1fd32656c95947d5202b566c068a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>迁移广告 FS 2.0 WID 电场的日落  
本文档提供有关广告的迁移的详细的信息 2.0 Windows 内部 FS 数据库 (WID) 场对 Windows Server 2012。

## <a name="migrate-an-ad-fs-wid-farm"></a>迁移广告 FS WID 电场的日落
若要迁移到 Windows Server 2012 WID 电场的日落，请执行以下步骤：  
  
1.  中每个节点（服务器）WID 电场的日落，查看并执行中的步骤[准备迁移 WID 场](prepare-to-migrate-a-wid-farm.md)。  
  
2.  从负载平衡删除任何非主要节点。  
  
3.  从 Windows Server 2008 R2 或 Windows Server 2008 此服务器 Windows Server 2012 上的操作系统的升级。 有关详细信息，请参阅[安装 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作为操作系统升级的结果，此服务器上的广告 FS 配置失去并且删除广告 FS 2.0 server 角色。 而是安装了 Windows Server 2012 广告 FS server 角色，但它未配置。 你必须创建原始广告 FS 配置并还原剩余的广告 FS 设置，以完成联盟 server 迁移。  
  
4.  在此服务器创建原始广告 FS 配置。  
  
你可以通过使用创建原始广告 FS 配置**广告 FS 联盟服务器配置向导**添加到 WID 场联合身份验证的服务器。 有关详细信息，请参阅[添加到联合身份验证的服务器场联合服务器](add-a-federation-server-to-a-federation-server-farm.md)。  
  
> [!NOTE]
> 当你转**指定的主联合身份验证的服务器和服务帐户**页中**广告 FS 联盟服务器配置向导**、输入 WID 农场里的主要联盟服务器的名称，然后务必输入准备广告 FS 迁移时记录服务帐户信息。 有关详细信息，请参阅[到迁移广告 FS 准备 2.0 联合身份验证的服务器](prepare-to-migrate-a-wid-farm.md)。 
>  
> 当你转**指定联合身份验证服务名称**页上，请务必在中选择你在"准备迁移 WID 场"录制相同 SSL 证书[到迁移广告 FS 准备 2.0 联合身份验证的服务器](prepare-to-migrate-a-wid-farm.md)。  
  
5.  更新你的广告 FS 网页此服务器上。 如果同时准备迁移备份你自定义的广告 FS 网页，你需要使用你的备份数据覆盖默认广告 FS 网页中的默认情况下创建的**%systemdrive%\inetpub\adfs\ls**目录广告 FS 配置 Windows Server 2012 上的结果。  
  
6.  添加你只需到负载平衡升级到 Windows Server 2012 的服务器。  
  
7.  其余 WID 场中辅助服务器重复步骤 1 到 6。  
  
8.  推广升级后次要的服务器来将主服务器 WID 场中的之一。 若要执行此操作，请打开 Windows PowerShell，并运行以下命令：`PSH:> Set-AdfsSyncProperties –Role PrimaryComputer`。  
  
9. 删除负载平衡 WID 场原始主服务器。  
  
10. 降级 WID 场是通过使用 Windows PowerShell 辅助服务器中的原主服务器。 打开 Windows PowerShell 并运行以下命令以添加到你的 Windows PowerShell 会话广告 FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后，运行以下命令以降级原主服务器辅助服务器：`PSH:> Set-AdfsSyncProperties – Role SecondaryComputer –PrimaryComputerName <FQDN of the Primary Federation Server>`。  
  
11. Windows Server 2012 到从 Windows Server 2008 R2 或 Windows Server 2008 WID 场中升级（服务器上）此上次节点上的操作系统。 有关详细信息，请参阅[安装 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作为升级操作系统的结果，此服务器上的广告 FS 配置失去并且删除广告 FS 2.0 server 角色。 而是安装了 Windows Server 2012 广告 FS server 角色，但它未配置。 必须手动创建原始广告 FS 配置并还原剩余的广告 FS 设置，以完成联盟 server 迁移。  
  
12. 在此 WID 场中的最后一节点（服务器）上创建原始广告 FS 配置。  
  
你可以通过使用创建原始广告 FS 配置**广告 FS 联盟服务器配置向导**添加到 WID 场联合身份验证的服务器。 有关详细信息，请参阅[添加到联合身份验证的服务器场联合服务器](add-a-federation-server-to-a-federation-server-farm.md)。  
  
> [!NOTE]
> 当你转**指定的主联合身份验证的服务器和服务帐户**页中**广告 FS 联盟服务器配置向导**，输入准备广告 FS 迁移时记录服务帐户信息。 有关详细信息，请参阅[到迁移广告 FS 准备 2.0 联合身份验证的服务器](prepare-to-migrate-a-wid-farm.md)。 
>  
> 当你转**指定联合身份验证服务名称**页上，请务必选择中记录的同一 SSL 证书[到迁移广告 FS 准备 2.0 联合身份验证的服务器](prepare-to-migrate-a-wid-farm.md)。  
  
13. 更新此 WID 场中的最后一个服务器上的你的广告 FS 网页。 如果时迁移准备备份你的自定义广告 FS 网页，使用你的备份数据覆盖默认广告 FS 网页中的默认情况下创建的**%systemdrive%\inetpub\adfs\ls**目录广告 FS 配置 Windows Server 2012 上的结果。  
  
14. 添加你只需到负载平衡升级到 Windows Server 2012 你 WID 场此上次服务器。  
  
15. 还原任何剩余广告 FS 自定义设置，如自定义特性官方商城。  
  
## <a name="next-steps"></a>后续步骤
 [准备迁移广告 FS 2.0 联盟服务器](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移广告 FS 2.0 联盟服务器代理服务器](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [迁移广告 FS 2.0 联盟服务器](migrate-the-ad-fs-fed-server.md)   
 [迁移广告 FS 2.0 联盟服务器代理服务器](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移广告 FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)
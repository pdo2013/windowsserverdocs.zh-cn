---
title: "迁移广告 FS 2.0 联盟服务器 SQL 电场的日落"
description: "将广告 FS 2.0 服务器 SQL 电场的日落迁移到 Windows Server 2012 上提供信息"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8433219850793aa34b646a3bf14cba42d3de4988
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>迁移广告 FS 2.0 WID 电场的日落  
本文将广告 FS 2.0 SQL 场迁移到 Windows Server 2012 上提供详细的信息。


## <a name="migrate-a-sql-server-farm"></a>迁移 SQL Server 电场的日落  
 若要迁移到 Windows Server 2012 SQL Server 电场的日落，请执行以下步骤：  
  
1.  对于每个服务器你 SQL Server 电场的日落，查看并执行中的步骤[迁移 SQL Server 场](prepare-to-migrate-a-sql-server-farm.md)。  
  
2.  SQL Server 场中删除负载平衡任何服务器。  
  
3.  从 Windows Server 2008 R2 或 Windows Server 2008 的 SQL Server 场中此服务器操作系统升级到 Windows Server 2012。 有关详细信息，请参阅[安装 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作为操作系统升级的结果，此服务器上的广告 FS 配置失去并且删除广告 FS 2.0 server 角色。 而是安装了 Windows Server 2012 广告 FS server 角色，但它未配置。 必须手动创建原始广告 FS 配置并还原剩余的广告 FS 设置，以完成联盟 server 迁移。  
  
4.  通过广告 FS Windows PowerShell cmdlet 添加到现有场服务器，在此服务器 SQL Server 场中创建原始广告 FS 配置。  
  
> [!IMPORTANT]
>  你必须使用 Windows PowerShell 创建原始广告 FS 配置，如果你使用 SQL Server 来存储您的广告 FS 配置数据库。  

  - 打开 Windows PowerShell 和运行以下命令：`$fscredential = Get-Credential`。  
  - 输入姓名和时迁移准备 SQL Server 场记录服务帐户的密码。  
  - 运行以下命令：`Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`，其中`Data Source`策略官方商城连接字符串值，以下文件中的数据来源值：`%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`。  
  
5.  添加你只需到负载平衡升级到 Windows Server 2012 的服务器。  
  
6.  重复步骤 2 到 6 SQL Server 场中的其余节点。  
  
7.  当中的所有服务器 SQL Server 场升级到 Windows Server 2012、还原任何剩余广告 FS 自定义设置，如自定义特性官方商城。  

## <a name="next-steps"></a>后续步骤
 [准备迁移广告 FS 2.0 联盟服务器](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移广告 FS 2.0 联盟服务器代理服务器](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [迁移广告 FS 2.0 联盟服务器](migrate-the-ad-fs-fed-server.md)   
 [迁移广告 FS 2.0 联盟服务器代理服务器](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移广告 FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)




---
title: 迁移 AD FS 2.0 联合服务器 WID 场
description: 提供有关将 AD FS 2.0 服务器 WID 场迁移到 Windows Server 2012 的信息
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 89da3de4bc626e12a1fc34752841f2de1afb5322
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408244"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>迁移 AD FS 2.0 WID 场  
本文档详细介绍了如何将 AD FS 2.0 Windows 内部数据库（WID）场迁移到 Windows Server 2012。

## <a name="migrate-an-ad-fs-wid-farm"></a>迁移 AD FS 的 WID 场
若要将 WID 场迁移到 Windows Server 2012，请执行以下过程：  
  
1.  对于 WID 场中的每个节点（服务器），查看并执行[准备迁移 WID 场](prepare-to-migrate-a-wid-farm.md)中的过程。  
  
2.  从负载均衡器中删除任何非主要节点。  
  
3.  将此服务器上的操作系统从 Windows Server 2008 R2 或 Windows Server 2008 升级到 Windows Server 2012。 有关详细信息，请参阅[安装 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作为操作系统升级的结果，在此服务器上的 AD FS 配置将丢失并且 AD FS 2.0 的服务器角色会被删除。 改为安装 Windows Server 2012 AD FS Server 角色，但未对其进行配置。 你必须创建原始 AD FS 配置并还原剩余的 AD FS 设置，以完成联合服务器迁移。  
  
4. 在此服务器上创建原始 AD FS 配置。  
  
你可以通过使用“AD FS 联合服务器配置向导” 创建原始 AD FS 配置，从而将联合服务器添加到 WID 场。 有关详细信息，请参阅[将联合服务器添加到联合服务器场](add-a-federation-server-to-a-federation-server-farm.md)。  
  
> [!NOTE]
> 在访问“AD FS 联合服务器配置向导” 中的“指定主联合服务器和服务帐户”页面时，输入 WID 场的主要联合服务器名称，然后确保输入你在准备 AD FS 迁移时所记录的服务帐户信息。 有关详细信息，请参阅[准备迁移 AD FS 2.0 联合服务器](prepare-to-migrate-a-wid-farm.md)。 
>  
> 当你到达 "**指定联合身份验证服务名称**" 页面时，请确保选择你在[准备迁移 AD FS 2.0 联合服务器](prepare-to-migrate-a-wid-farm.md)的 "准备迁移 WID 场" 中记录的相同 SSL 证书。  
  
5. 更新你在此服务器上的 AD FS 网页。 如果你在准备迁移时备份你的自定义 AD FS 网页，则需要使用你的备份数据来覆盖默认 AD FS 网页，这些网页在默认情况下作为 AD FS 的结果在**位于%systemdrive%\inetpub\adfs\ls**目录中创建Windows Server 2012 上的配置。  
  
6. 将刚刚升级到 Windows Server 2012 的服务器添加到负载均衡器。  
  
7. 对于 WID 场中剩余的辅助服务器，请重复步骤 1 到 6。  
  
8. 将升级后的一台辅助服务器升级为你的 WID 场中的主服务器。 为此，请打开 Windows PowerShell 并运行以下命令： `PSH:> Set-AdfsSyncProperties –Role PrimaryComputer`。  
  
9. 从负载平衡器中删除你的 WID 场的原始主服务器。  
  
10. 通过使用 Windows PowerShell，将你的 WID 场中的原始主服务器降级为辅助服务器。 打开 Windows PowerShell 并运行以下命令，将 AD FS cmdlet 添加到你的 Windows PowerShell 会话： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后运行以下命令将原始主服务器降级为辅助服务器： `PSH:> Set-AdfsSyncProperties – Role SecondaryComputer –PrimaryComputerName <FQDN of the Primary Federation Server>`。  
  
11. 将你的 WID 场中的最后一个节点（服务器）上的操作系统从 Windows Server 2008 R2 或 Windows Server 2008 升级到 Windows Server 2012。 有关详细信息，请参阅[安装 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作为升级操作系统的结果，在此服务器上的 AD FS 配置将丢失并且 AD FS 2.0 的服务器角色会被删除。 改为安装 Windows Server 2012 AD FS Server 角色，但未对其进行配置。 你必须手动创建原始 AD FS 配置并还原剩余的 AD FS 设置，以完成联合服务器迁移。  
  
12. 在你的 WID 场中的最后一个节点（服务器）上创建原始 AD FS 配置。  
  
你可以通过使用“AD FS 联合服务器配置向导” 创建原始 AD FS 配置，从而将联合服务器添加到 WID 场。 有关详细信息，请参阅[将联合服务器添加到联合服务器场](add-a-federation-server-to-a-federation-server-farm.md)。  
  
> [!NOTE]
> 在访问“AD FS 联合服务器配置向导” 中的“指定主联合服务器和服务帐户”页面时，输入你在准备 AD FS 迁移时记录的服务帐户信息。 有关详细信息，请参阅[准备迁移 AD FS 2.0 联合服务器](prepare-to-migrate-a-wid-farm.md)。 
>  
> 当你到达 "**指定联合身份验证服务名称**" 页面时，请确保选择你在[准备迁移 AD FS 2.0 联合服务器](prepare-to-migrate-a-wid-farm.md)时所记录的相同 SSL 证书。  
  
13. 更新在你的 WID 场中最后一台服务器上的 AD FS 网页。 如果你在准备迁移时备份你的自定义 AD FS 网页，则使用你的备份数据来覆盖默认情况下在**位于%systemdrive%\inetpub\adfs\ls**目录中作为 AD FS 的结果创建的默认 AD FS 网页Windows Server 2012 上的配置。  
  
14. 将你刚刚升级到 Windows Server 2012 的 WID 场中的最后一台服务器添加到负载均衡器。  
  
15. 还原任何剩余 AD FS 自定义项，如自定义属性存储。  
  
## <a name="next-steps"></a>后续步骤
 [准备将 AD FS 2.0 联合服务器迁移](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移 AD FS 2.0 联合服务器代理](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [将 AD FS 2.0 联合服务器迁移](migrate-the-ad-fs-fed-server.md)   
 [迁移 AD FS 2.0 联合服务器代理](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移 AD FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)
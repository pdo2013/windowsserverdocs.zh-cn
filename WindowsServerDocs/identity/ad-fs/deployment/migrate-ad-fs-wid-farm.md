---
title: 迁移 AD FS 2.0 联合服务器 WID 场
description: 提供有关将 AD FS 2.0 服务器 WID 场迁移到 Windows Server 2012 的信息
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c85a02ae6a71cf31fd172ec012a14cd81c126e16
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445648"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>迁移 AD FS 2.0 WID 场  
本文档提供有关迁移 AD 的详细的信息与 Windows Server 2012 的 2.0 Windows 内部 FS 数据库 (WID) 场。

## <a name="migrate-an-ad-fs-wid-farm"></a>迁移 AD FS WID 场
若要将 WID 场迁移到 Windows Server 2012 中，执行以下过程：  
  
1.  对于 WID 场中每个节点 （服务器），查看并执行中的过程[准备迁移 WID 场](prepare-to-migrate-a-wid-farm.md)。  
  
2.  从负载均衡器中删除任何非主要节点。  
  
3.  在此服务器从 Windows Server 2008 R2 或 Windows Server 2008 到 Windows Server 2012 上的操作系统升级。 有关详细信息，请参阅[安装 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作为操作系统升级的结果，在此服务器上的 AD FS 配置将丢失并且 AD FS 2.0 的服务器角色会被删除。 相反，安装 Windows Server 2012 AD FS 服务器角色，但未配置。 你必须创建原始 AD FS 配置并还原剩余的 AD FS 设置，以完成联合服务器迁移。  
  
4. 在此服务器上创建原始 AD FS 配置。  
  
你可以通过使用“AD FS 联合服务器配置向导”  创建原始 AD FS 配置，从而将联合服务器添加到 WID 场。 有关详细信息，请参阅[将联合服务器添加到联合服务器场](add-a-federation-server-to-a-federation-server-farm.md)。  
  
> [!NOTE]
> 在访问“AD FS 联合服务器配置向导”  中的“指定主联合服务器和服务帐户”  页面时，输入 WID 场的主要联合服务器名称，然后确保输入你在准备 AD FS 迁移时所记录的服务帐户信息。 有关详细信息，请参阅[准备迁移 AD FS 2.0 联合身份验证服务器](prepare-to-migrate-a-wid-farm.md)。 
>  
> 当到达**指定联合身份验证服务名称**页上，确保选择在"准备迁移 WID 场"记录的相同 SSL 证书中[准备迁移 AD FS 2.0 联合身份验证服务器](prepare-to-migrate-a-wid-farm.md).  
  
5. 更新你在此服务器上的 AD FS 网页。 如果准备迁移时备份你的自定义 AD FS 网页，需要使用你的备份数据来覆盖默认 AD FS 网页在默认情况下创建 **%systemdrive%\inetpub\adfs\ls**作为目录Windows Server 2012 上的 AD FS 配置结果。  
  
6. 添加你刚升级到 Windows Server 2012 到负载均衡器的服务器。  
  
7. 对于 WID 场中剩余的辅助服务器，请重复步骤 1 到 6。  
  
8. 将升级后的一台辅助服务器升级为你的 WID 场中的主服务器。 为此，请打开 Windows PowerShell 并运行以下命令： `PSH:> Set-AdfsSyncProperties –Role PrimaryComputer`。  
  
9. 从负载平衡器中删除你的 WID 场的原始主服务器。  
  
10. 通过使用 Windows PowerShell，将你的 WID 场中的原始主服务器降级为辅助服务器。 打开 Windows PowerShell 并运行以下命令，将 AD FS cmdlet 添加到你的 Windows PowerShell 会话： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然后运行以下命令将原始主服务器降级为辅助服务器： `PSH:> Set-AdfsSyncProperties – Role SecondaryComputer –PrimaryComputerName <FQDN of the Primary Federation Server>`。  
  
11. 在你从 Windows Server 2008 R2 或 Windows Server 2008 到 Windows Server 2012 的 WID 场中升级的最后一个节点 （服务器） 上的操作系统。 有关详细信息，请参阅[安装 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作为升级操作系统的结果，在此服务器上的 AD FS 配置将丢失并且 AD FS 2.0 的服务器角色会被删除。 相反，安装 Windows Server 2012 AD FS 服务器角色，但未配置。 你必须手动创建原始 AD FS 配置并还原剩余的 AD FS 设置，以完成联合服务器迁移。  
  
12. 在你的 WID 场中的最后一个节点（服务器）上创建原始 AD FS 配置。  
  
你可以通过使用“AD FS 联合服务器配置向导”  创建原始 AD FS 配置，从而将联合服务器添加到 WID 场。 有关详细信息，请参阅[将联合服务器添加到联合服务器场](add-a-federation-server-to-a-federation-server-farm.md)。  
  
> [!NOTE]
> 在访问“AD FS 联合服务器配置向导”  中的“指定主联合服务器和服务帐户”  页面时，输入你在准备 AD FS 迁移时记录的服务帐户信息。 有关详细信息，请参阅[准备迁移 AD FS 2.0 联合身份验证服务器](prepare-to-migrate-a-wid-farm.md)。 
>  
> 当到达**指定联合身份验证服务名称**页上，确保选择你在中记录的相同 SSL 证书[准备迁移 AD FS 2.0 联合身份验证服务器](prepare-to-migrate-a-wid-farm.md)。  
  
13. 更新在你的 WID 场中最后一台服务器上的 AD FS 网页。 如果准备迁移时备份你的自定义 AD FS 网页，使用你的备份数据来覆盖默认 AD FS 网页在默认情况下创建 **%systemdrive%\inetpub\adfs\ls**结果的目录Windows Server 2012 上的 AD FS 配置。  
  
14. 添加此最后一台服务器的 WID 场中的你刚升级到 Windows Server 2012 到负载均衡器。  
  
15. 还原任何剩余 AD FS 自定义项，如自定义属性存储。  
  
## <a name="next-steps"></a>后续步骤
 [准备迁移 AD FS 2.0 联合服务器](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移 AD FS 2.0 联合服务器代理](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [迁移 AD FS 2.0 联合服务器](migrate-the-ad-fs-fed-server.md)   
 [迁移 AD FS 2.0 联合服务器代理](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移 AD FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)
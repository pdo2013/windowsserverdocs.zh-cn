---
ms.assetid: cd70b0aa-0a67-4966-a041-4dd3f302c98b
title: "创建 DNS 基础结构设计"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6e5093b05fd81a693cec87ddb00d39e70483df23
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-dns-infrastructure-design"></a>创建 DNS 基础结构设计

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

创建 Active Directory 林和域设计后，你必须设计域名系统 (DNS) 基础结构，以支持你的 Active Directory 逻辑结构。 DNS 使用户可以使用很容易记住，若要连接到计算机和其他资源 IP 网络上的友好名称。 Active Directory 域服务(广告 DS) 在 Windows Server 2008 需要 DNS。  
  
设计 DNS 支持广告 DS 的过程不同根据你的组织已是否有现有 DNS 服务器服务或你正在部署新的 DNS 服务器服务：  
  
-   如果你已经有现有 DNS 基础结构，你必须将 Active Directory 命名空间集成到此环境中。 有关详细信息，请参阅[集成到一个现有 DNS 基础结构的广告 DS](../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)。  
  
-   如果你没有在地方 DNS 基础结构，你必须设计和部署新的 DNS 基础结构，以支持广告 DS。 有关详细信息，请参阅部署域名系统 (DNS) ([https://go.microsoft.com/fwlink/?LinkId=93656](https://go.microsoft.com/fwlink/?LinkId=93656))。  
  
如果你的组织有现有 DNS 基础结构，你必须确保你了解你 DNS 基础结构与 Active Directory 命名空间的交互方式。 为了帮助您中记录你的现有 DNS 基础结构设计工作表中，下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip 从工作辅助为 Windows Server 2003 部署工具包 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，然后打开"DNS 清单"(DSSLOGI_8.doc)。  
  
> [!NOTE]  
> 除了 IP 版本 4 (IPv4) 地址、Windows Server 2008 还支持的版本 6 (IPv6) IP 地址。 以帮助你列出记录你的当前 DNS 结构递归名称解决方法时 IPv6 地址工作表中，请参阅[附录 a: DNS 清单](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)。  
  
设计你 DNS 基础结构，以支持广告 DS 之前，可以阅读有关 DNS 层次、DNS 名称分辨率过程，以及如何 DNS 支持广告 DS 很有帮助。 有关 DNS 层次和名称分辨率进程的详细信息，请参阅 DNS 技术参考 ([https://go.microsoft.com/fwlink/?LinkID=48145](https://go.microsoft.com/fwlink/?LinkID=48145))。 有关如何 DNS 支持广告 DS 的详细信息，请参阅 Active Directory 技术参考 DNS 支持 ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147))。  
  
## <a name="in-this-section"></a>在此部分中  
  
-   [检查 DNS 概念](../../ad-ds/plan/Reviewing-DNS-Concepts.md)  
  
-   [DNS 和广告 DS](../../ad-ds/plan/DNS-and-AD-DS.md)  
  
-   [广告 DS 负责人的角色指定 DNS](../../ad-ds/deploy/Assigning-the-DNS-for-AD-DS-Owner-Role.md)  
  
-   [集成到一个现有 DNS 基础结构的广告 DS](../../ad-ds/plan/../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)  
  



---
title: Active Directory 服务器的性能优化
description: Active Directory 服务器的性能优化
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab; v-tea
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: c4d14cfb4bfc8a6919683a360ec171de8250799a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370318"
---
# <a name="performance-tuning-active-directory-servers"></a>Active Directory 服务器的性能优化

Active Directory 的性能优化侧重于两个目标：
- 以最佳方式配置 Active Directory 来以尽可能高效的方式为负载提供服务
- 提交到 Active Directory 的工作负荷应当尽可能高效

这需要恰当注意以下三个单独区域：
- 正确的容量计划 – 确保实施足够的硬件来支持现有负载
- 服务器端优化 – 配置域控制器来尽可能高效地处理负载
- Active Directory 客户端/应用程序优化 – 确保客户端和应用程序以最佳方式使用 Active Directory

## <a name="start-with-capacity-planning"></a>开始容量计划

在正确的域、正确的区域设置中正确地部署足够数量的域控制器并提供冗余对于确保及时为客户端请求提供服务至关重要。 这是一个深入的主题，不在本指南的范围内。 建议读者先阅读并理解 [Active Directory 域服务的容量计划](capacity-planning-for-active-directory-domain-services.md)中的建议和指南，然后开始其 Active Directory 性能优化。

>[!Important]
> 正确配置 Active Directory 以及设置其大小对整个系统和工作负荷性能有重大的潜在影响。 强烈建议读者首先阅读 [Active Directory 域服务的容量计划](capacity-planning-for-active-directory-domain-services.md)。

## <a name="updates-and-evolving-recommendations"></a>更新和不断演变的建议

在过去几代操作系统中，Active Directory 和客户端性能优化都进行了大量改进，这些工作还在继续。 建议部署最新版本的平台来获得好处，包括：

- 更高的可靠性
- 更好的性能
- 用于故障排除的更好的日志记录和工具

但是，我们知道这需要时间，并且许多环境的运行场景不可能完全采用最新平台。 较旧版本的平台中也添加了一些改进，我们将继续添加更多改进。

我们建议你关注我们的团队博客[“向目录服务团队提问”](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/bg-p/AskDS)来了解有关管理 ADDS 的最新新闻、指南和最佳做法。

## <a name="see-also"></a>另请参阅

- [AD DS 容量计划](capacity-planning-for-active-directory-domain-services.md)
- [硬件注意事项](hardware-considerations.md)
- [内存使用情况注意事项](memory-usage-considerations.md)
- [LDAP 注意事项](ldap-considerations.md)
- [域控制器的正确放置和站点注意事项](site-definition-considerations.md)
- [AD DS 性能疑难解答](troubleshoot.md)  
  

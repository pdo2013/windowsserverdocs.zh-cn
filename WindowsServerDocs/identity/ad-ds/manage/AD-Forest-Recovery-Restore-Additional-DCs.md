---
title: "广告森林恢复-剩余 Dc 重新部署"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 66b0bc65b3b8e5dfbf5f1a85350dab60ac3a11c8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>广告森林恢复-剩余 Dc 重新部署

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 到目前为止的步骤适用于所有林：对于每个域查找有效备份、隔离在域中恢复、将它们重新连接、全球的目录，重置和清理。 在这一下一步中，您将重新部署森林。 执行此操作的方法将大大取决于森林设计，你服务级别协议、站点结构、可用带宽和许多其他因素。 你将需要设计根据原则，并在此部分中，以最适合企业要求的一种建议自己重新部署套餐。  
  
 下一步是上不存在之前森林恢复发生的所有域控制器安装广告 DS。 如果仍然存在域控制器，广告 DS 服务将需要强行，删除或 Dc 可以重新安装。 有关这些 Dc 任何现有备份无法重新使用，因为已森林恢复过程中删除相应的元数据。 简单的环境中此重新部署过程可以像重新连接到生产网络中，已恢复的 Dc 并根据需要升级新 Dc 简单。  
  
 与全球的基础结构怪脸庞的大型企业，在需要更完善的套餐。 第一阶段通常是还原作为一项服务; 广告这意味着战略安装放置域控制器，以便所有关键业务的分隔和应用程序可以重新开始工作。 它可能适用于临时有降低性能，因此分支机构。 作为第二个阶段，所有剩余和不太重要的 Dc 正在重新部署。  
  
 有两种方法可以安装其他 Dc，可自动这两种：  
  
-   复制  
  
     对于运行 Windows Server 2012 的虚拟化环境，复制是恢复 Dc 了大量的最快且最简单方法。 从备份中还原到一个虚拟化的 DC 后可以自动某个域中的所有虚拟化 Dc 恢复。  
  
     有关复制和先决条件的详细信息，请参阅[简介 Active Directory 域服务 (广告 DS) 虚拟化 (级别 100)](https://technet.microsoft.com/library/hh831734.aspx)。  
  
-   通过使用运行 Windows Server 2012（或 Dcpromo.exe 运行较早版本的 Windows Server 的服务器上）的服务器上的 Windows PowerShell 或使用用户界面重新安装广告 DS  
  
     若要加快重新安装广告 DS，可用于安装媒体 (IFM) 选项从在安装期间减少复制通信。 有关如何使用**ntdsutil ifm**命令创建安装媒体，请参阅[安装广告 DS 从媒体](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx)。  
  
 请考虑为虚拟化 DC 复制或通过安装（而不是从备份中还原）广告 DS 恢复树林中的每个副本 DC 其他以下几点：  
  
-   复制必须能够要复制的用作源直流的所有软件。 复制启动之前，应删除应用程序和服务，不能复制。 如果不能，替代的虚拟化的 DC 应该选择的源。  
  
-   复制从第一个虚拟化域控制器要还原的其他虚拟化域控制器，如果需要关闭时将其 VHDX 文件复制源 DC。 然后它将运行并提供联机时需要克隆虚拟 Dc 都是第一次启动。 如果所需的关机宕机不满意的第一个恢复域控制器，部署额外的虚拟化的 DC 安装广告 DS 用作复制的来源。  
  
-   不没有主机名称复制的虚拟化的 DC 或要安装广告 DS 服务器任何限制。 你可以使用新的主名称或之前所使用的主机名称。 有关 DNS 主机名称语法的详细信息，请参阅[创建 DNS 计算机名称](https://technet.microsoft.com/library/cc785282.aspx)([https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564))。  
  
-   将每个服务器森林 (已还原根域中的第一个 DC) 中的第一个 DNS 服务器具有配置 TCP/IP 属性其网络适配器中首选 DNS 服务器。 有关详细信息，请参阅[配置 TCP/IP 使用 DNS](https://technet.microsoft.com/library/cc779282.aspx)。  
  
-   重新部署在域中，所有 Rodc 了虚拟化 DC 复制，如果多个 Rodc 部署在中心的位置，或通过的传统重新它们生成通过删除并重新安装广告 DS，如果他们在独立所在的位置，例如分支机构单独部署方法。  
  
     重新生成 Rodc 可以确保他们不包含任何延迟对象，并可帮助防止复制冲突更高版本。 当你从 RODC，删除广告 DS*保留 DC 元数据的选项中选择*。 使用此选项可 krbtgt 帐户保留 RODC 保留委派的 RODC 管理员帐户和密码复制策略 (PRP) 的权限和会阻止你无需使用域管理员凭据删除并重新安装 RODC 广告 DS。 如果在安装到 RODC 最初还保留了 DNS 服务器和全球目录角色。  
  
     当你重新生成 Dc（Rodc 或写 Dc），则可能会在其重新安装期间增加复制通信。 以帮助减少该影响，可以交错的日程安排 RODC 安装中，并且你可以使用安装从媒体 (IFM) 选项。 如果你使用的 IFM 选项，运行**ntdsutil ifm**在你信任是免费的损坏数据可写直流命令。 这有助于防止从广告 DS 重新安装完成后显示上 RODC 可能已损坏。 有关 IFM 的详细信息，请参阅[安装广告 DS 从媒体](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx)。  
  
     有关重新生成 Rodc 的详细信息，请参阅[RODC 删除并重新安装](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx)。  
  
-   如果 DC 运行 DNS 服务器服务森林故障之前，安装并配置广告 DS 在安装期间 DNS 服务器服务。 否则，将其前者 DNS 客户端配置为与其他 DNS 服务器。  
  
-   如果需要额外的全球目录共享身份验证或查询加载的用户或应用程序，你可以添加到源全球目录复制之前虚拟化 DC 你也可以制作 DC 全球目录服务器广告 DS 在安装期间。  
  
## <a name="next-steps"></a>后续步骤
-   [广告森林恢复-先决条件](AD-Forest-Recovery-Prerequisties.md)  
-   [广告森林恢复-在寻找自定义森林恢复套餐](AD-Forest-Recovery-Devising-a-Plan.md)  
- [广告森林恢复-找出问题](AD-Forest-Recovery-Identify-the-Problem.md)
-   [广告森林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [广告森林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)  
-   [广告森林恢复-常见问题](AD-Forest-Recovery-FAQ.md)  
-   [广告森林恢复-恢复单个域中多域森林](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [广告森林恢复-森林恢复与 Windows Server 2003 该域控制器](AD-Forest-Recovery-Windows-Server-2003.md)  
---
title: AD 林恢复-剩余 Dc 重新部署
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: af9ec02a480b35e573edebcdd5928451174b6c1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811998"
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>AD 林恢复-剩余 Dc 重新部署

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

到目前为止的步骤适用于所有林状结构： 对于每个域中查找有效的备份、 恢复中隔离的域、 将它们重新连接、 重置全局编录和清理。 此下一步中，将重新部署该林。 若要执行此操作的方式将极大地依赖于林设计、 服务级别协议、 站点结构、 可用带宽和许多其他因素。 您将需要设计您自己基于原则和建议在此部分中，最适合于您的业务需求的方式的重新部署计划。  
  
下一步是在林恢复发生之前存在的所有域控制器上安装 AD DS。 如果在域控制器仍存在，将需要强制，删除 AD DS 服务或域控制器可以重新安装。 不能重复使用这些 Dc 的任何现有备份，因为已在林恢复过程中删除相应的元数据。 在不复杂环境中此重新部署过程可能很简单，重新连接到生产网络中，已恢复的 Dc 并根据需要提升新 Dc。  
  
在大型企业面临着全球基础结构，需要更复杂的计划。 第一阶段通常是还原即服务; AD这意味着若要安装多个巧妙布局放置 Dc，这样所有关键的业务部门和应用程序可以再次开始工作。 它可能是可接受的分支机构暂时会降低其性能结果。 作为第二个阶段中，所有剩余和不太重要的 Dc 重新部署。  
  
 有两种方法来安装其他域控制器，可以自动执行这两种：  
  
- 克隆  
   - 对于运行 Windows Server 2012 的虚拟化环境，克隆是恢复 Dc 的大量的速度最快和最简单方法。 从备份中还原单个虚拟化的 DC 后，可以自动执行所有虚拟化 Dc 的域中的恢复。  
   - 有关克隆和先决条件的详细信息，请参阅[Active Directory 域服务 (AD DS) 虚拟化 (级别 100) 简介](https://technet.microsoft.com/library/hh831734.aspx)。  
- 通过用户界面或通过使用 Windows PowerShell 在运行 Windows Server 2012 （或在运行 Windows Server 的早期版本的服务器上的 Dcpromo.exe） 的服务器上重新安装 AD DS  
   - 若要加快重新安装 AD DS，你可以从媒体 (IFM) 选项使用安装在安装期间减少复制流量。 有关使用详细信息**ntdsutil ifm**命令来创建安装介质，请参阅[从介质安装 AD DS](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx)。  

请考虑虚拟化 DC 克隆，或者通过安装 AD DS （而不是从备份中还原） 恢复在林中每个副本 DC 的以下附加点：  
  
- 在用作克隆源 DC 上的所有软件都必须能够克隆。 克隆启动之前，应删除应用程序和服务不能被克隆。 如果这不可能，应为源选择备用的虚拟化的 DC。  
- 如果克隆从第一个要还原的虚拟化 DC 的其他虚拟化的 Dc，将需要关闭其 VHDX 文件将复制时源 DC。 然后它将需要运行和可用联机时克隆虚拟域控制器是首次启动。 如果关闭所需的停机时间是不可接受的第一个恢复域控制器，请安装 AD DS 作为克隆源部署其他虚拟化域控制器。  
- 不没有主机名上克隆的虚拟化的 DC 或你想要安装 AD DS 的服务器的任何限制。 可以使用新的主机名或以前已被使用的主机名。 有关 DNS 主机名称语法的详细信息，请参阅[创建 DNS 计算机名](https://technet.microsoft.com/library/cc785282.aspx)([https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564))。  
- 将每个林 (已还原根级域中的第一个 DC) 中的第一个 DNS 服务器具有服务器配置为其网络适配器的 TCP/IP 属性中的首选 DNS 服务器。 有关详细信息，请参阅[配置 TCP/IP，使用 DNS](https://technet.microsoft.com/library/cc779282.aspx)。  
- 重新部署所有 Rodc 的域中，通过虚拟化 DC 克隆，如果多个 Rodc 部署在一个中心位置，或通过删除并重新安装 AD DS，如果它们单独部署在独立的所在位置重新生成它们的传统方法如分支机构。  
   - 重新生成 Rodc 可确保它们不包含任何延迟对象，并且可以帮助防止以后发生复制冲突。 当从 RODC 中，删除 AD DS*选择用于保留 DC 元数据的选项*。 使用此选项保留为 RODC 的 krbtgt 帐户和保留的委派的 RODC 管理员帐户和密码复制策略 (PRP) 的权限并防止您无需使用域管理员凭据来删除并重新安装 AD DSRODC。 如果它们最初安装在 RODC 上还保留了对 DNS 服务器和全局编录角色。  
   - 当重新生成 Dc （Rodc 或可写 Dc） 时，可能会增加其重新安装期间复制流量。 若要帮助降低这种影响，可以分阶段的 RODC 安装，计划，并可以使用从介质安装 (IFM) 选项。 如果使用 IFM 选项，请运行**ntdsutil ifm**命令的可写 DC，信任是免费的损坏数据。 这有助于防止使其不显示在 RODC 上的 AD DS 重新安装完成后可能已损坏。 有关 IFM 的详细信息，请参阅[从介质安装 AD DS](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx)。  
   - 有关重建 Rodc 的详细信息，请参阅[RODC 删除和重新安装](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx)。  
- 如果 DC 运行 DNS 服务器服务林工作不正常之前，安装和配置 DNS 服务器服务在 AD ds 安装过程。 否则，与其他 DNS 服务器配置其以前的 DNS 客户端。  
- 如果需要其他全局编录共享身份验证或用户或应用程序的查询负载，可以添加到源的全局编录之前克隆虚拟化 DC，或者可以进行 DC 全局编录服务器的 AD DS 安装过程。  
  
## <a name="next-steps"></a>后续步骤

- [AD 林恢复的系统必备组件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 林恢复-设计出一个自定义林恢复计划](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 林恢复-识别问题](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)  
- [AD 林恢复-方面的常见问题](AD-Forest-Recovery-FAQ.md)  
- [AD 林恢复-恢复单个的多域林中域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 林恢复-与 Windows Server 2003 域控制器的林恢复](AD-Forest-Recovery-Windows-Server-2003.md)

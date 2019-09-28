---
title: AD 林恢复-重新部署剩余 Dc
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: fbab907c5624a76540ab6a28c568afbd9192c028
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390247"
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>AD 林恢复-重新部署剩余 Dc

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

到目前为止的步骤适用于所有林：查找每个域的有效备份，单独恢复域，重新连接这些域，重置全局编录，并清除。 在下一步中，你将重新部署林。 执行此操作的方式将很大程度上取决于林设计、服务级别协议、站点结构、可用带宽以及其他许多因素。 你将需要根据此部分中的原则和建议设计自己的重新部署计划，这种方法最适合你的业务需求。  
  
下一步是在执行林恢复之前在所有 Dc 上安装 AD DS。 如果 Dc 仍然存在，则需要强制删除 AD DS 服务，或者可以重新安装 Dc。 不能重复使用这些 Dc 的任何现有备份，因为在林恢复期间，相应的元数据已被删除。 在复杂的环境中，此重新部署过程可以很简单，只需将已恢复的 Dc 重新连接到生产网络，并根据需要提升新的 Dc。  
  
在面临全球基础结构的大型企业中，需要一个更复杂的计划。 第一阶段通常是将广告还原为服务;这意味着安装策略放置的 Dc，以便所有关键业务部门和应用程序都可以再次开始工作。 由于此原因，分支机构暂时降低了性能。 作为第二个阶段，将重新部署所有剩余和不太重要的 Dc。  
  
 可以通过两种方法来安装其他 Dc，这两种方法都可以自动执行：  
  
- 克隆  
   - 对于运行 Windows Server 2012 的虚拟化环境，克隆是最快且最简单的方法来恢复大量 Dc。 从备份中还原单个虚拟 DC 后，可以自动恢复域中的所有虚拟化 dc。  
   - 有关克隆和先决条件的详细信息，请参阅[Active Directory 域服务（AD DS）虚拟化简介（等级100）](https://technet.microsoft.com/library/hh831734.aspx)。  
- 在运行 Windows Server 2012 的服务器（或运行早期版本 Windows Server 的服务器上的 Dcpromo.exe）或使用用户界面的服务器上使用 Windows PowerShell 重新安装 AD DS  
   - 为了加快 AD DS 的重新安装，你可以使用 "从媒体安装（IFM）" 选项来在安装过程中减少复制流量。 有关使用**ntdsutil ifm**命令创建安装媒体的详细信息，请参阅[从媒体安装 AD DS](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx)。  

对于在林中通过虚拟化 DC 克隆或安装 AD DS （而不是从备份还原）恢复的每个副本 DC，请注意以下附加点：  
  
- DC 上用作克隆源的所有软件都必须能够克隆。 在启动克隆之前，应删除无法克隆的应用程序和服务。 如果无法做到这一点，则应选择备用的虚拟化 DC 作为源。  
- 如果从要还原的第一个虚拟化 DC 克隆其他虚拟化的 dc，则需要在复制其 VHDX 文件时关闭源 DC。 然后，当克隆虚拟 Dc 首次启动时，它需要运行并联机。 如果关闭所需的停机时间对于第一个已恢复 DC 是不可接受的，则通过安装 AD DS 来部署其他虚拟化 DC，作为克隆的源。  
- 对于克隆的虚拟化 DC 或要在其上安装 AD DS 的服务器的主机名没有限制。 你可以使用以前使用的新主机名或主机名。 有关 DNS 主机名语法的详细信息，请参阅[创建 Dns 计算机名称](https://technet.microsoft.com/library/cc785282.aspx)（[@no__t](https://go.microsoft.com/fwlink/?LinkId=74564)）。  
- 将每台服务器配置为林中的第一个 DNS 服务器（在根域中还原的第一个 DC）作为其网络适配器的 TCP/IP 属性中的首选 DNS 服务器。 有关详细信息，请参阅[将 Tcp/ip 配置为使用 DNS](https://technet.microsoft.com/library/cc779282.aspx)。  
- 如果将多个 Rodc 部署到一个中心位置，或通过删除并重新安装 AD DS 的传统方法重新部署域中的所有 Rodc，则可以通过删除并重新安装来重新部署该域中的所有 Rodc例如分支机构。  
   - 重新生成 Rodc 可确保它们不包含任何延迟对象，并可帮助防止在以后出现复制冲突。 从 RODC 中删除 AD DS 时，请*选择保留 DC 元数据的选项*。 使用此选项将保留 RODC 的 krbtgt 帐户，并保留委派的 RODC 管理员帐户和密码复制策略（PRP）的权限，并阻止你使用域管理员凭据删除和重新安装 AD DSRODC。 如果 DNS 服务器和全局编录角色最初安装在 RODC 上，则它还会保留这些角色。  
   - 重新生成 Dc （Rodc 或可写 Dc）时，可能会在重新安装时增加复制流量。 为了帮助降低这种影响，您可以错开 RODC 安装计划，并且可以使用 "从媒体安装（IFM）" 选项。 如果使用 IFM 选项，请在你信任的可写 DC 上运行**ntdsutil IFM**命令，以释放损坏的数据。 这有助于防止在 AD DS 重新安装完成后，在 RODC 上出现可能的损坏。 有关 IFM 的详细信息，请参阅[从媒体安装 AD DS](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx)。  
   - 有关重新生成 Rodc 的详细信息，请参阅[RODC 删除和重新安装](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx)。  
- 如果在林发生故障之前 DC 正在运行 DNS 服务器服务，请在安装 AD DS 期间安装和配置 DNS 服务器服务。 否则，请将其以前的 DNS 客户端配置为其他 DNS 服务器。  
- 如果需要额外的全局编录来共享用户或应用程序的身份验证或查询负载，可以在克隆之前将全局编录添加到源虚拟化 DC 中，也可以在安装 AD DS 期间使 DC 成为全局编录服务器。  
  
## <a name="next-steps"></a>后续步骤

- [AD 林恢复 - 先决条件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 林恢复-设计自定义林恢复计划](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 林恢复-识别问题](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)  
- [AD 林恢复-常见问题](AD-Forest-Recovery-FAQ.md)  
- [AD 林恢复-恢复多域林中的单个域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 林恢复-通过 Windows Server 2003 域控制器恢复林](AD-Forest-Recovery-Windows-Server-2003.md)

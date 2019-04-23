---
title: AD 林恢复-恢复单个域中的多域林
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 267541be-2ea7-4af6-ab34-8b5a3fedee2d
ms.technology: identity-adds
ms.openlocfilehash: fae2cc40af0b43dd38d72c2622720a6bb17b0a66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863468"
---
# <a name="ad-forest-recovery---recovering-a-single-domain-in-a-multidomain-forest"></a>AD 林恢复-恢复单个域中的多域林

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

可能有必要恢复单个域的林中的具有多个域，而不是完整林恢复时的时间。 本主题介绍有关恢复单个域和恢复的可能策略注意事项。  
  
单个域恢复颇具挑战性的重新生成全局编录 (GC) 服务器。 例如，如果从已创建此前，一周的备份，然后在林中的所有其他 Gc 还原域的第一个域控制器 (DC) 必须为该域比已还原的 DC 的更多最新的数据。 若要重新建立 GC 数据一致性，有几个选项：  
  
- Unhost，然后重新恢复的域分区中的林，除了那些已恢复的域中的所有 Gc 从托管在同一时间。  
- 执行林恢复过程，恢复在域中，然后从其他域中的 Gc 移除延迟对象。  
  
以下部分提供每个选项的一般注意事项。 针对不同的 Active Directory 环境而异的整套需要执行恢复的步骤。  
  
## <a name="rehost-all-gcs"></a>重新托管所有 Gc  

> [!WARNING]
> 在成为 GC 以供登录到的问题会阻止访问的情况下，所有域的域管理员帐户的密码必须是供使用。  

完成重新承载所有 Gc 使用 repadmin / unhost 和 repadmin /rehost 命令 （repadmin /experthelp 的一部分）。 在每次 GC 未恢复每个域中，将运行 repadmin 命令。 它需要确保，所有的 Gc 不不再包含已恢复的域的副本。 若要实现此目的，unhost 域分区首先来自所有域控制器的林中的所有无恢复域范围内第一次。 所有的 Gc 不不再包含分区后，您可以将其重新托管。 重新承载时，站点和复制的结构的你的林，为例，完成每个站点在重新承载该站点的其他 Dc 前一个 DC 的 rehost。  
  
此选项可以是小型组织中具有几个域控制器为每个域具有明显的优势。 所有 Gc 可以重新生成在星期五夜间上，并且如有必要，在星期一早上之前分区的所有只读的域的完整复制。 但如果你需要恢复站点涵盖在全球范围内的大型域，重新承载所有 Gc 上的只读的域分区的其他域可显著影响操作，可能需要停机。  
  
## <a name="remove-lingering-objects"></a>移除延迟对象

类似于林恢复过程，一个 DC 从备份还原需要恢复、 执行剩余的 Dc 的元数据清理和重新安装 AD DS 以构建出域的域中。 在所有林中其他域的 Gc，删除已恢复的域的只读分区延迟对象。  

延迟对象清理的源必须是已恢复的域的 DC。 为特定源 DC 不具有任何域分区的任何延迟对象，可以删除全局编录，如果它是一个 GC。  

移除延迟对象是有好处的大型组织，不能与其他选项相关联的停机时间的风险。  

有关详细信息，请参阅[使用 Repadmin 删除延迟对象](https://technet.microsoft.com/library/cc785298.aspx)。

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

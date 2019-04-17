---
title: 广告森林恢复-单个域多域森林中的恢复
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 267541be-2ea7-4af6-ab34-8b5a3fedee2d
ms.technology: identity-adfs
ms.openlocfilehash: 3a1c9d0671a732eee83aa707e061afdbbf106fa5
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2018
---
# <a name="ad-forest-recovery---recovering-a-single-domain-in-a-multidomain-forest"></a>广告森林恢复-单个域多域森林中的恢复

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

可以使用它是恢复单个域具有多个域，而不是完全森林恢复树林中的所需的时间。 本主题介绍注意事项恢复单个域，可能用于恢复的策略。  
  
 一个域恢复提供的全球目录 (GC) 服务器重新生成一个唯一的难题。 例如，如果域的第一个域控制器 (DC) 还原的备份中创建的一周更早版本，然后森林中的所有其他 Gc 从将有更多最新比还原直流该域数据。 若要重新建立 GC 数据一致性，有以下几个选项：  
  
-   Unhost，然后同时 rehost 从除恢复的域中的树林中的所有 Gc 域恢复的分区。  
  
-   按照森林恢复过程恢复域中，并从 Gc 其他域中删除延迟对象。  
  
 下面提供了每个选项常规事项。 一整套需要进行恢复的步骤会有所不同针对不同的 Active Directory 环境。  
  
## <a name="rehost-all-gcs"></a>Rehost 所有 Gc  

> [!WARNING]
>  问题阻止访问 GC 用于登录的情况下，则必须是可供使用的所有域管理员帐户的密码。  

 完成变换主机的所有 Gc 使用 repadmin//unhost 和 repadmin /rehost 命令 (repadmin 的一部分 /experthelp)。 你将在每次 GC 没有恢复每个域中运行 repadmin 命令。 它需要确保，所有 Gc 不再都按一份恢复的域。 为此，unhost 域分区首先从所有域控制器所有无恢复的以至森林第一次。 所有 Gc 再不都包含分区后，你可以 rehost 它。 当变换主机，请考虑站点-和复制-结构之你森林，例如，完成的站点之前变换主机其他 Dc 该网站的每一个直流 rehost。  
  
 此选项非常适用于小型组织具有仅少数域控制器，以便每个域。 所有 Gc 可能会重新组装在周五晚，并在必要情况下在星期一早晨之前分区，所有只读域完整复制。 但是，如果你需要恢复涵盖站点在全球范围内的大型域，变换主机的所有 Gc 只读域分区对于其他域可以显著影响操作，并且可能需要向下时间。  
  
## <a name="remove-lingering-objects"></a>删除延迟对象  
 类似于森林恢复过程，一个 DC 从备份中还原需要恢复、执行其余 Dc 的元数据清理并重新安装广告 DS 出域生成的域中。 森林中的所有其他域 Gc，在你删除恢复的域只读分区延迟的对象。  
  
 延迟的对象清理对于源必须 DC 恢复的域中。 若要某些源 DC 没有任何域分区任何延迟对象，你可以删除全球目录 GC 时。  
  
 删除延迟对象是适用于无法风险与其他选项的关机时间的大型企业。  
  
 有关详细信息，请参阅[使用 Repadmin 去延迟对象](https://technet.microsoft.com/library/cc785298.aspx)。

## <a name="next-steps"></a>后续步骤
-   [广告森林恢复-先决条件](AD-Forest-Recovery-Prerequisties.md)  
-   [广告森林恢复-在寻找自定义森林恢复套餐](AD-Forest-Recovery-Devising-a-Plan.md)  
-   [广告森林恢复-找出问题](AD-Forest-Recovery-Identify-the-Problem.md)
-   [广告森林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [广告森林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)  
-   [广告森林恢复-常见问题](AD-Forest-Recovery-FAQ.md)  
-   [广告森林恢复-恢复单个域中多域森林](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [广告森林恢复-森林恢复与 Windows Server 2003 该域控制器](AD-Forest-Recovery-Windows-Server-2003.md)  

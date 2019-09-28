---
title: 确定软件限制策略的“允许-拒绝”列表和应用程序清单
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0abb73b6-b5d8-4505-8ab1-2f29e4bf0411
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 4ddea6daeb2150bd9fd3131a8457a6a4b408cfc3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357656"
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>确定软件限制策略的“允许-拒绝”列表和应用程序清单

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

适用于 IT 专业人员的本主题提供了有关如何为从 Windows Server 2008 和 Windows Vista 开始的软件限制策略（SRP）管理的应用程序创建 "允许" 和 "拒绝" 列表。

## <a name="introduction"></a>介绍
软件限制策略 (SRP) 是基于组策略的功能，用于标识在域中的计算机上运行的软件程序，以及控制这些程序的运行能力。 你可以使用软件限制策略创建计算机的高度受限配置，从而仅允许运行专门标识的应用程序。 它们与 Microsoft Active Directory 域服务和组策略集成在一起，但也可以在独立计算机上进行配置。 有关 SRP 的起点，请参阅[软件限制策略](software-restriction-policies.md)。

从 Windows Server 2008 R2 和 Windows 7 开始，可以使用 Windows AppLocker，而不是与 SRP 一起使用，以获得部分应用程序控制策略。

有关如何使用 SRP 完成特定任务的信息，请参阅以下内容：

-   [使用软件限制策略规则](work-with-software-restriction-policies-rules.md)

-   [使用软件限制策略来帮助保护你的计算机免受电子邮件病毒侵害](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>要选择的默认规则：允许或拒绝
软件限制策略可采用以下两种模式之一进行部署：允许列表或拒绝列表。 您可以创建一个策略，用于标识允许在您的环境中运行的每个应用程序;策略中的默认规则受到限制，并且会阻止你未明确允许运行的所有应用程序。 或者，你可以创建一个策略来识别每个无法运行的应用程序;默认规则是 "无限制"，只限制显式列出的应用程序。

> [!IMPORTANT]
> 对于你的组织而言，"拒绝列表" 模式可能是针对应用程序控制的高维护策略。 创建和维护阻止所有恶意软件和其他有问题的应用程序的不断演变的列表将非常耗时且容易出错。

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>为允许列表创建应用程序清单
若要有效地使用 "允许默认" 规则，需要确定组织中所需的应用程序。 有一些工具可用于生成应用程序清单，如 Microsoft 应用程序兼容性工具包中的清单收集器。 但 SRP 具有高级日志记录功能，可帮助你确切了解你的环境中正在运行的应用程序。

##### <a name="to-discover-which-applications-to-allow"></a>发现允许哪些应用程序

1.  在测试环境中，将 "默认规则" 设置为 "不受限制" 的软件限制策略，并删除任何其他规则。 如果在没有强制限制任何应用程序的情况下启用 SRP，SPR 将能够监视正在运行的应用程序。

2.  若要启用高级日志记录功能并将路径设置为要写入日志文件的位置，请创建以下注册表值。

    **"HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Safer\ CodeIdentifiers"**

    字符串值：*NameLogFile 的 NameLogFile 路径*

    由于 SRP 在运行时对所有应用程序进行评估，因此每次运行该应用程序时，都会将一个条目写入日志文件*NameLogFile* 。

3.  评估日志文件

    每个日志条目状态：

    -   软件限制策略的调用方和调用进程的进程 ID （PID）

    -   正在计算的目标

    -   运行应用程序时遇到的 SRP 规则

    -   SRP 规则的标识符。

    写入日志文件的输出示例：

**explorer （PID = 4728） identifiedC： \ Windows\system32\onenote.exe As 无限制 usingpath rule，Guid = {320bd852-aa7c-4674-82c5-9a80321670a3}**   SRP 检查并设置为 "阻止" 的所有应用程序和相关代码将记录在日志文件中，然后可以使用该日志文件来确定应将哪些可执行文件视为允许列表。



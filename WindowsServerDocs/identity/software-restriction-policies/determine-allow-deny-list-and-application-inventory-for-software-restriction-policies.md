---
title: 确定软件限制策略的“允许-拒绝”列表和应用程序清单
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 60e78912284715649938567d66ffb90b9890b1b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847288"
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>确定软件限制策略的“允许-拒绝”列表和应用程序清单

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题面向 IT 专业人员提供了指导如何创建允许和拒绝通过软件限制策略 (SRP) 从 Windows Server 2008 和 Windows Vista 管理的应用程序的列表。

## <a name="introduction"></a>简介
软件限制策略 (SRP) 是基于组策略的功能，用于标识在域中的计算机上运行的软件程序，以及控制这些程序的运行能力。 你可以使用软件限制策略创建计算机的高度受限配置，从而仅允许运行专门标识的应用程序。 这些与 Microsoft Active Directory 域服务和组策略集成，但也可以配置在独立计算机上。 有关 SRP 的起始点，请参阅[软件限制策略](software-restriction-policies.md)。

从 Windows Server 2008 R2 和 Windows 7 开始，Windows AppLocker 可以用于或 SRP 配合应用程序控制策略的一部分。

有关如何完成特定任务使用 SRP 的信息，请参阅：

-   [使用软件限制策略规则](work-with-software-restriction-policies-rules.md)

-   [使用软件限制策略来帮助保护计算机免受电子邮件病毒](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>若要选择哪个默认规则：允许或拒绝
软件限制策略可以部署在是默认规则的基础的两种模式之一：允许列表或拒绝列表。 可以创建一个策略，用于标识允许将环境; 中运行每个应用程序你的策略中的默认规则是受限制，将阻止所有不显式允许运行的应用程序。 也可以创建一个策略，标识不能运行; 每个应用程序默认规则是不受限制的并限制只有已显式列出的应用程序。

> [!IMPORTANT]
> 拒绝列表模式可能是涉及应用程序控制组织的大量维护策略。 创建和维护禁止所有恶意软件和其他有问题的应用程序的不断发展列表会耗时且容易出错。

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>创建您的允许列表的应用程序的清单
若要有效地使用允许默认规则，需要确定哪些应用程序在组织中所需的准确。 提供了用于生成应用程序清单，例如 Microsoft 应用程序兼容性工具包中的清单收集器的工具。 但是，SRP 具有一项高级日志记录功能来帮助你了解完全什么应用程序是在环境中运行。

##### <a name="to-discover-which-applications-to-allow"></a>若要了解允许哪些应用程序

1.  在测试环境中，默认规则设置为 unrestricted 不使用部署软件限制策略并删除任何其他规则。 如果不强制对它进行限制的任何应用程序的情况下启用 SRP，SPR 将能够监视哪些应用程序正在运行。

2.  创建以下注册表值以启用高级日志记录功能并将路径设置为应在其中写入日志文件。

    **"HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Safer\ CodeIdentifiers"**

    字符串值：*NameLogFile NameLogFile 路径*

    因为 SRP 评估所有应用程序在运行时，向日志文件写入一个条目*NameLogFile*每次运行应用程序。

3.  评估的日志文件

    每个日志条目表明：

    -   软件限制策略和过程的调用进程的 ID (PID) 的调用方

    -   正在计算目标

    -   该应用程序运行时遇到 SRP 规则

    -   SRP 规则标识符。

    将输出写入到日志文件的示例：

**explorer.exe (PID = 4728) identifiedC:\Windows\system32\onenote.exe 作为不受限制的 usingpath 规则，Guid = {320bd852-aa7c-4674-82c5-9a80321670a3}** 将在日志中记录所有应用程序和关联的代码，SRP 检查和设置为阻止文件，然后可用于确定对于允许列表中，应考虑的可执行文件。



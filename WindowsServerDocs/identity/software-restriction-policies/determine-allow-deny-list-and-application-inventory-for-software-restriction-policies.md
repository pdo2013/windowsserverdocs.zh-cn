---
title: "确定软件限制策略允许拒绝列表和应用程序目录获利"
description: "Windows Server 安全"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>确定软件限制策略允许拒绝列表和应用程序目录获利

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

此主题以供 IT 专业人员提供如何创建允许拒绝由与 Windows Server 2008 软件限制策略 (SRP) 开头的应用程序的列表和 Windows Vista 的指南。

## <a name="introduction"></a>简介
软件限制策略 (SRP) 是基于组策略的功能，以标识某个域中，在计算机上运行的软件程序，并控制这些程序能够运行。 你可以使用软件限制策略若要创建受限高度对于你在其中允许仅明确识别运行的应用程序的计算机，配置。 这些组策略与 Microsoft Active Directory 域服务集成，但也可单独的计算机上配置。 有关 SRP 的起始点，请参阅[软件限制策略](software-restriction-policies.md)。

开始与 Windows Server 2008 R2 和 Windows 7、 Windows AppLocker 可用于或配合 SRP 控制策略应用程序的一部分。

有关如何完成使用 SRP 特定的任务，请参阅以下信息：

-   [适用于软件限制策略规则](work-with-software-restriction-policies-rules.md)

-   [使用限制策略的软件来帮助保护你的计算机免受电子邮件病毒](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>若要选择哪个默认规则：允许或拒绝
可以在两种模式默认规则基础部署软件限制策略：允许列表或拒绝列表。 你可以创建标识每个应用允许在; 您的环境中运行程序策略你的策略中的默认规则是受限，将阻止你确实不明确允许运行的所有应用。 也可以创建标识每个应用程序不能运行; 策略默认规则是无限制和会限制你已明确列出的应用程序。

> [!IMPORTANT]
> 拒绝列表模式可能会涉及应用控制你的组织的高维护策略。 创建和维护会阻止所有恶意软件和其他有问题的应用的发展列表会耗时和遭受错误。

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>创建你的允许列表中的应用程序的目录获利
若要有效地使用允许默认规则，你需要确定哪些应用程序在你的组织中的需要完全。 有旨在产生应用程序清单，如 Microsoft 应用程序兼容性工具包中收集库存器工具。 但是 SRP 具有高级日志记录功能，可帮助您了解完全哪些应用程序正在运行环境中。

##### <a name="to-discover-which-applications-to-allow"></a>发现允许哪些应用程序

1.  测试环境中，在默认规则设置为无限制使用部署限制策略的软件，并删除任何其他规则。 如果您不强制限制有任何应用程序的情况下启用 SRP，SPR 将能够监视器哪些应用禁止运行。

2.  创建以便启用日志记录的高级的功能和设置的路径编写日志文件应该以下注册表值。

    **"HKEY_LOCAL_MACHINE\SOFTWARE\Policies\ Microsoft \Windows\Safer\ CodeIdentifiers"**

    字符串值：*NameLogFile NameLogFile 路径*

    因为 SRP 评估所有应用在运行时，会向日志文件写入条目*NameLogFile*应用程序运行每次。

3.  评估日志文件

    每个日志条目指出：

    -   软件的限制策略和的进程 ID (PID) 调用进程的调用方

    -   评估目标

    -   该应用运行时遇到 SRP 规则

    -   SRP 规则标识符。

    输出写入日志文件中的一个示例：

**explorer.exe (PID = 4728) identifiedC:\Windows\system32\onenote.exe 作为无限制的 usingpath 规则，Guid = {320bd852-aa7c-4674-82c5-9a80321670a3}**所有应用程序和相关的代码 SRP 检查并设置阻止述日志文件，然后可用于确定哪些可执行文件应该考虑为你的允许列表。



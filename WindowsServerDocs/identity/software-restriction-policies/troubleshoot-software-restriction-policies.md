---
title: 软件限制策略疑难解答
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fd53736-03e7-4bf9-ba90-d1212d93e19a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 8dff3e1542afcc3cba3645b6834981bd6ed33f58
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407165"
---
# <a name="troubleshoot-software-restriction-policies"></a>软件限制策略疑难解答

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题介绍了从 Windows Server 2008 和 Windows Vista 开始对软件限制策略（SRP）进行故障排除时遇到的常见问题及其解决方案。

## <a name="introduction"></a>介绍
软件限制策略 (SRP) 是基于组策略的功能，用于标识在域中的计算机上运行的软件程序，以及控制这些程序的运行能力。 你可以使用软件限制策略创建计算机的高度受限配置，从而仅允许运行专门标识的应用程序。 它们与 Microsoft Active Directory 域服务和组策略集成在一起，但也可以在独立计算机上进行配置。 有关 SRP 的详细信息，请参阅[软件限制策略](software-restriction-policies.md)。

从 Windows Server 2008 R2 和 Windows 7 开始，可以使用 Windows AppLocker，而不是与 SRP 一起使用，以获得部分应用程序控制策略。

### <a name="windows-cannot-open-a-program"></a>Windows 无法打开程序
用户会收到一条消息，显示 "Windows 无法打开此程序，因为它已被软件限制策略阻止。 有关详细信息，请打开事件查看器或与系统管理员联系。 " 或者，在命令行中，消息显示 "系统无法执行指定的程序。"

**原因：** 创建默认安全级别（或规则）是为了使软件程序设置为 "不**允许**"，因此不会启动。

**解决方案：** 在事件日志中查看消息的详细说明。 事件日志消息指示将哪个软件程序设置为**禁止**，并将哪个规则应用于程序。

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>修改的软件限制策略未生效
**原因：** 通过组策略在域中指定的软件限制策略会替代在本地配置的任何策略设置。 这可能意味着域中有一个替代策略设置的策略设置。

**原因：** 组策略可能未刷新其策略设置。 组策略会定期将更改应用于策略设置;因此，可能尚未刷新在目录中所做的策略更改。

**解决方案**

1.  修改网络的软件限制策略的计算机必须能够联系域控制器。 确保计算机可以与域控制器联系。

2.  刷新策略，方法是注销网络，然后再次登录到网络。 如果任何策略通过组策略应用，则重新登录将会刷新这些策略。

3.  您可以使用命令行实用工具 gpupdate 或从注销，然后重新登录到您的计算机，以刷新策略设置。 为获得最佳结果，请运行 gpupdate，然后注销并重新登录到您的计算机。 通常，工作站或服务器上的安全设置每90分钟刷新一次，在域控制器上每5分钟刷新一次。 还可将设置指定为每 16 个小时刷新一次，无论是否存在更改。 这些是可配置的设置，因此，每个域中的刷新间隔可能不同。

4.  检查应用的策略。 检查 "**没有替代**设置的域级别策略"。

5.  通过组策略在域中指定的软件限制策略会替代在本地配置的任何策略。 使用 Gpresult 命令行工具来确定策略的最终效果。 这可能意味着域中有一个替代本地设置的策略。

6.  如果 SRP 和 AppLocker 策略设置位于同一个 GPO 中，AppLocker 设置将优先于 Windows 7、Windows Server 2008 R2 和更高版本。 建议在不同的 Gpo 中放置 SRP 和 AppLocker 策略设置。

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>通过 SRP 添加规则后，不能登录到计算机
**原因：** 计算机启动时，计算机会访问许多程序和文件。 可能无意中将其中一个程序或文件设置为 "不**允许**"。 由于计算机无法访问程序或文件，因此无法正常启动。

**解决方案：** 在安全模式下启动计算机，以本地管理员身份登录，然后更改软件限制策略以允许程序或文件运行。

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>新的策略设置未应用于特定文件扩展名
**原因：** 文件扩展名不在支持的文件类型列表中。

**解决方案：** 将文件扩展名添加到 SRP 支持的文件类型列表中。

软件限制策略解决了对未知或不受信任的代码进行控制的问题。 软件限制策略是用于标识软件并控制其在本地计算机、站点、域或 OU 中运行，并且可以通过 GPO 实现的功能的安全设置。

### <a name="a-default-rule-is-not-restricting-as-expected"></a>默认规则不按预期方式进行限制
**原因：** 按特定顺序应用的规则，这可能会导致默认规则被特定规则覆盖。 SRP 按以下顺序应用规则（最特定于常规）：

1.  哈希规则

2.  证书规则

3.  路径规则

4.  Internet 区域规则

5.  默认规则

**解决方案：** 评估限制应用程序的规则，如果适用，则删除除默认规则之外的所有规则。

### <a name="unable-to-discover-which-restrictions-are-applied"></a>无法发现应用了哪些限制
**原因：** 出现意外行为没有任何明显的原因，并且 GPO 刷新未解决此问题，因此需要进一步进行调查。

**解决方案**

1.  调查系统事件日志，筛选 "软件限制策略" 的源。 这些项显式声明为每个应用程序实现了哪个规则。

2.  启用高级日志记录。 有关详细信息，请参阅[确定软件限制策略的允许/拒绝列表和应用程序清单](software-restriction-policies.md)。



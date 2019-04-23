---
title: 软件限制策略疑难解答
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: abf592930f5347fa10e83c644671f68f9dc1a3f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872378"
---
# <a name="troubleshoot-software-restriction-policies"></a>软件限制策略疑难解答

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题介绍常见的问题和其解决方案进行故障排除与 Windows Server 2008 和 Windows Vista 的软件限制策略 (SRP) 开始时。

## <a name="introduction"></a>简介
软件限制策略 (SRP) 是基于组策略的功能，用于标识在域中的计算机上运行的软件程序，以及控制这些程序的运行能力。 你可以使用软件限制策略创建计算机的高度受限配置，从而仅允许运行专门标识的应用程序。 这些与 Microsoft Active Directory 域服务和组策略集成，但也可以配置在独立计算机上。 有关 SRP 的详细信息，请参阅[软件限制策略](software-restriction-policies.md)。

从 Windows Server 2008 R2 和 Windows 7 开始，Windows AppLocker 可以用于或 SRP 配合应用程序控制策略的一部分。

### <a name="windows-cannot-open-a-program"></a>Windows 无法打开程序
用户会收到一条消息，指出"Windows 由于无法打开此程序已阻止通过软件限制策略。 有关详细信息，打开事件查看器或系统管理员联系。" 或者，在命令行中，一条消息指出"系统无法执行指定的程序"。

**原因：** 默认安全级别 （或规则） 创建，以便软件程序设置为**不允许**，因此它将不启动。

**解决方案：** 查看事件日志中的消息的详细说明。 事件日志消息指示哪些软件程序设置为**不允许**和哪个规则应用于该程序。

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>修改后的软件限制策略不能发挥作用
**原因：** 通过组策略的域中指定的软件限制策略替代本地配置任何策略设置。 这可能表示有一个替代策略设置的域的策略设置。

**原因：** 组策略可能不刷新其策略设置。 组策略更改应用于策略设置定期;因此，很可能在目录中进行了策略更改尚未刷新。

**解决方案：**

1.  修改网络软件限制策略的计算机必须能够联系域控制器。 请确保计算机可以联系域控制器。

2.  通过从网络日志记录，然后登录到网络再次刷新策略。 如果通过组策略应用任何策略，则重新登录后将刷新这些策略。

3.  你可以刷新策略设置与命令行实用程序 gpupdate 或通过从注销然后重新登录你的计算机日志记录。 为获得最佳结果，运行 gpupdate，，然后从注销和重新登录到您的计算机。 通常情况下，工作站或服务器上每隔 90 分钟和域控制器上每隔 5 分钟刷新的安全设置。 还可将设置指定为每 16 个小时刷新一次，无论是否存在更改。 这些是可配置的设置，因此每个域中刷新的时间间隔可能会不同。

4.  检查应用哪些策略。 检查的域级别策略**禁止替代**设置。

5.  通过组策略的域中指定的软件限制策略替代本地配置的所有策略。 使用 Gpresult 命令行工具来确定策略的净效果是什么。 这可能意味着没有来自域进行重写本地设置的策略。

6.  如果 SRP 和 AppLocker 策略设置在同一 GPO 中，AppLocker 设置将优先于 Windows 7，Windows Server 2008 R2 和更高版本。 建议将 SRP 和 AppLocker 策略设置放在不同的 Gpo。

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>添加后通过 SRP 规则，你无法登录到您的计算机
**原因：** 在启动时，您的计算机访问许多程序和文件。 您可能会无意中设置的这些程序或文件复制到一个**不允许**。 计算机无法访问的程序或文件，因为它不能正常启动。

**解决方案：** 在安全模式下启动计算机，以本地管理员身份，登录，然后更改软件限制策略，以允许该程序或文件中运行。

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>新的策略设置不应用到特定文件扩展名
**原因：** 文件扩展名不是受支持的文件类型的列表。

**解决方案：** 将文件扩展名添加到支持的 SRP 的文件类型的列表。

软件限制策略解决调整未知或不受信任的代码的问题。 软件限制策略是安全设置，以标识软件并控制其站点、 域或 OU 中的本地计算机上运行的能力，并可以通过 GPO 实现。

### <a name="a-default-rule-is-not-restricting-as-expected"></a>默认规则不按预期方式限制
**原因：** 按特定顺序，这可能导致默认规则，以按特定规则重写应用的规则。 SRP 应用规则按以下顺序 （最特定于常规）：

1.  哈希规则

2.  证书规则

3.  路径规则

4.  Internet 区域规则

5.  默认规则

**解决方案：** 计算限制的应用程序的规则，并且，如果适用，请删除除默认规则。

### <a name="unable-to-discover-which-restrictions-are-applied"></a>无法发现应用的限制
**原因：** 没有意外的行为，没有明显原因，因此有必要进行进一步调查，GPO 刷新未已解决此问题。

**解决方案：**

1.  调查系统事件日志，筛选源的"软件限制策略"。 条目显式声明每个应用程序实现的规则。

2.  启用高级日志记录。 请参阅[确定允许拒绝列表和有关软件限制策略的应用程序清单](software-restriction-policies.md)有关详细信息。



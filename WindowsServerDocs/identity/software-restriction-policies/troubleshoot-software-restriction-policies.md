---
title: "疑难解答软件限制策略"
description: "Windows Server 安全"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-software-restriction-policies"></a>疑难解答软件限制策略

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍常见问题和他们解决方案时疑难解答软件限制策略 (SRP) 与 Windows Server 2008 和 Windows Vista 的开头。

## <a name="introduction"></a>简介
软件限制策略 (SRP) 是基于组策略的功能，以标识某个域中，在计算机上运行的软件程序，并控制这些程序能够运行。 你可以使用软件限制策略若要创建受限高度对于你在其中允许仅明确识别运行的应用程序的计算机，配置。 这些组策略与 Microsoft Active Directory 域服务集成，但也可单独的计算机上配置。 有关 SRP 的详细信息，请参阅[软件限制策略](software-restriction-policies.md)。

开始与 Windows Server 2008 R2 和 Windows 7、 Windows AppLocker 可用于或配合 SRP 控制策略应用程序的一部分。

### <a name="windows-cannot-open-a-program"></a>Windows 无法打开程序
用户收到一条消息:"Windows 无法打开该程序因为软件限制策略阻止。 有关详细信息，打开事件查看器或与系统管理员联系。" 或在命令行，会显示消息"系统无法执行指定的程序"。

**原因：**的默认安全级别（或规则）创建以便软件程序设置为**不允许**，因此它不会启动并。

**解决方法：**在该消息的详细描述事件日志中查找。 事件日志消息指示何种软件程序设置为**不允许**和哪些规则应用于这项计划。

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>修改的软件限制策略没有生效
**原因：**通过组策略某个域中指定的软件限制策略覆盖配置本地任何策略设置。 这可能表示没有取代你的策略设置的域从某个策略设置。

**原因：**组策略可能不刷新其策略设置。 组策略更改策略设置为定期应用。因此，很有可能目录中所做的策略更改尚未刷新。

**解决方法：**

1.  你在其修改软件限制网络策略的计算机必须能够连接的域控制器。 确保计算机可以联系域控制器。

2.  通过网络注销，然后登录到该网络再次刷新策略。 通过组策略应用任何策略，如果重新登录将刷新这些策略。

3.  你可以恢复使用命令行实用程序 gpupdate 也可通过注销然后再重新登录你的计算机的策略设置。 获取最佳结果，运行 gpupdate，，然后注销并重新登录到你的计算机。 一般情况下，每个 90 分钟工作站或服务器上，域控制器上的每个 5 分钟刷新安全设置。 设置也将刷新每可用 16 个小时，有任何更改。 这些是可配置设置，这样在每个域刷新每隔可能会有所不同。

4.  查看哪些策略应用。 查看有关域级别策略**禁止替代**设置。

5.  在某个域，通过组策略中指定的软件限制策略覆盖配置本地任何策略。 使用 Gpresult 命令行工具来确定网络策略的效果。 这可能表示没有从取代你的本地设置的域的策略。

6.  如果 SRP 和 AppLocker 策略设置都在同一 GPO，AppLocker 设置将优先 Windows 7、Windows Server 2008 R2、及更高版本。 建议在不同 Gpo 使 SRP 和 AppLocker 策略设置。

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>添加 SRP 通过规则之后, 你无法登录到你的计算机
**原因：**启动电脑时，你的计算机将访问许多程序和文件。 可能会无意中设置完这些程序或文件复制到之一**不允许**。 由于计算机无法访问文件的程序，它无法正常启动。

**解决方法：**在安全模式下启动计算机的本地管理员的身份登录，然后更改软件限制策略允许运行文件的程序。

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>不会将新的策略设置应用到特定文件扩展名
**原因：**文件扩展名不受支持的文件类型的列表中。

**解决方法：**将文件扩展名添加到 SRP 支持的文件类型的列表。

软件限制策略解决该问题的调整未知的或不受信任的代码。 软件限制策略安全设置需要识别软件，并控制要在本地计算机上，在网站、域或组织单位运行，并且可以通过 GPO 中实现。

### <a name="a-default-rule-is-not-restricting-as-expected"></a>默认规则不是按预期限制
**原因：**规则其应用以特定的顺序，这可能会导致默认规则特定规则重写。 SRP 适用规则按以下顺序（最特定于常规）：

1.  哈希规则

2.  证书规则

3.  路径规则

4.  Internet 时区规则

5.  默认规则

**解决方法：**评估规则限制应用程序，并根据需要删除所有但默认规则。

### <a name="unable-to-discover-which-restrictions-are-applied"></a>找不到应用的限制
**原因：**意外的行为，任何明显的原因和 GPO 刷新不具有解决该问题以便有必要进行进一步调查。

**解决方法：**

1.  调查系统事件日志、筛选源"限制策略软件"。 条目明确说明哪个规则实现的每个应用。

2.  启用高级日志记录。 请参阅[确定允许拒绝列表和软件限制策略应用程序库存](software-restriction-policies.md)详细信息。



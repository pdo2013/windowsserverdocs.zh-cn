---
title: 文件服务器资源管理器疑难解答
description: 本文介绍如何解决在使用文件服务器资源管理器时遇到的常见问题
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 413cf51e5ceb1c4507b71fb77ee6005807a0ff13
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476180"
---
# <a name="troubleshooting-file-server-resource-manager"></a>文件服务器资源管理器疑难解答

> 适用于：Windows Server 2019，Windows Server 2016、 Windows 服务器 （半年频道）、 Windows Server 2012 R2 和 Windows Server 2012 中，Windows Server 2008 R2

本部分列出了在使用文件服务器资源管理器时可能遇到的常见问题。

> [!Note]
> 一个不错的疑难解答选项就是查看文件服务器资源管理器生成的事件日志。 文件服务器资源管理器的所有事件日志项均位于源 **SRMSVC** 下的**应用程序**事件日志中

## <a name="i-am-not-receiving-e-mail-notifications"></a>我无法接收电子邮件通知。

-   **原因**：电子邮件选项尚未配置或配置不正确。

-   **解决方案**：上**电子邮件通知**选项卡上，在**文件服务器资源管理器选项**对话框框中，验证的 SMTP 服务器和默认的电子邮件收件人已指定且有效。 发送测试电子邮件以确认信息正确，且用于发送通知的 SMTP 服务器正常工作。 有关详细信息，请参阅[配置电子邮件通知](configure-email-notifications.md)。


## <a name="i-am-only-receiving-one-e-mail-notification-even-though-the-event-that-triggered-that-notification-happened-several-times-in-a-row"></a>我只接收到一个电子邮件通知，即使触发该通知的事件连续多次发生也是如此。

-   **原因**：当用户多次尝试保存被阻止的文件或保存文件超出配额阈值，存在电子邮件通知配置为该文件屏蔽或配额事件，只有一个电子邮件发送到管理员在 60 分钟内按 默认值。 这可防止管理员的电子邮件帐户中出现大量重复的消息。

-   **解决方案**：上**通知限制**选项卡上，在**文件服务器资源管理器选项**对话框中，您可以为每个通知类型设置时间限制： 电子邮件、 事件日志、 命令和报表。 对于相同的问题，每项限制都会指定一个生成另一相同类型的通知前必须经过的时间段。 有关详细信息，请参阅[配置通知限制](configure-notification-limits.md)。


## <a name="my-storage-reports-keep-failing-and-little-or-no-information-is-available-in-the-event-log-regarding-the-source-of-the-failure"></a>我的存储报告总是出现故障，并且事件日志中几乎没有或完全没有关于故障原因的可用信息。

-   **原因**：报表的保存位置的卷可能已损坏。
-   **解决方案**：运行**chkdsk**卷并稍后重试生成报告。

## <a name="my-file-screening-audit-reports-do-not-contain-any-information"></a>我的文件屏蔽审核报告中没有任何信息。

-   **原因**：一个或多个以下可能原因：
    -   审核数据库未配置为记录文件屏蔽活动。
    -   审核数据库为空（即未记录任何文件屏蔽活动）。
    -   文件屏蔽审核报告的参数未从审核数据库中选择数据。
    
-   **解决方案**：上**文件屏蔽审核**选项卡上，在**文件服务器资源管理器选项**对话框中，验证**记录文件屏蔽审核数据库中的活动**检查选中框。
    -   有关记录文件屏蔽活动的详细信息，请参阅[配置文件屏蔽审核](configure-file-screen-audit.md)。
    -   若要配置文件屏蔽审核报告的默认参数，请参阅[配置存储报告](configure-storage-reports.md)。
    -   若要编辑计划报告任务或按需报告的报告参数，请参阅[存储报告管理](storage-reports-management.md)。

## <a name="the-used-and-available-values-for-some-of-the-quotas-i-have-created-do-not-correspond-to-the-actual-limit-setting"></a>我创建的部分配额的“已使用”和“可用”值与实际的“限制”设置不对应。

-   **原因**：可能有嵌套的配额，其中一个子文件夹的配额从其父文件夹的配额派生更为严格的限制。 例如，可能将一个 100 兆字节 (MB) 的配额限制应用于父文件夹，并将一个 200 MB 的配额分别应用于其中的每一个子文件夹。 如果父文件夹中共存储了 50 MB 的数据（存储在其子文件夹中的数据总量），则每一个子文件夹将仅列出 50 MB 的可用空间。

-   **解决方案**：下**配额管理**，单击**配额**。 在**结果**窗格中，选择正在解决的配额项。 在**操作**窗格中，单击**查看影响文件夹的配额**，然后查找应用于父文件夹的配额。 这将能够确定哪些父文件夹配额拥有低于所选配额的存储限制设置。


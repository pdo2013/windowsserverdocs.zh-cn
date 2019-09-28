---
title: 文件服务器资源管理器疑难解答
description: 本文介绍如何解决在使用文件服务器资源管理器时遇到的常见问题
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 0f30bcfd07f28ecd3fa1618e4f5d89b7a0b4d14f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403086"
---
# <a name="troubleshooting-file-server-resource-manager"></a>文件服务器资源管理器疑难解答

> 适用于：Windows Server 2019，Windows Server 2016，Windows Server （半年频道），Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2

本部分列出了在使用文件服务器资源管理器时可能遇到的常见问题。

> [!Note]
> 一个不错的疑难解答选项就是查看文件服务器资源管理器生成的事件日志。 文件服务器资源管理器的所有事件日志项均位于源 **SRMSVC** 下的**应用程序**事件日志中

## <a name="i-am-not-receiving-e-mail-notifications"></a>我无法接收电子邮件通知。

-   **原因**：电子邮件选项尚未配置或配置不正确。

-   **解决方案**：在 "**电子邮件通知**" 选项卡上的 "**文件服务器资源管理器选项**" 对话框中，验证 SMTP 服务器和默认电子邮件收件人是否已指定并且有效。 发送测试电子邮件以确认信息正确，且用于发送通知的 SMTP 服务器正常工作。 有关详细信息，请参阅[配置电子邮件通知](configure-email-notifications.md)。


## <a name="i-am-only-receiving-one-e-mail-notification-even-though-the-event-that-triggered-that-notification-happened-several-times-in-a-row"></a>我只接收到一个电子邮件通知，即使触发该通知的事件连续多次发生也是如此。

-   **原因**：当用户多次尝试保存被阻止的文件或保存一个超出配额阈值的文件，并且为该文件屏蔽或配额事件配置了电子邮件通知时，只会在60分钟内向管理员发送一封电子邮件， 缺省值. 这可防止管理员的电子邮件帐户中出现大量重复的消息。

-   **解决方案**：在 "**通知限制**" 选项卡上的 "**文件服务器资源管理器选项**" 对话框中，您可以为每种通知类型设置时间限制：电子邮件、事件日志、命令和报表。 对于相同的问题，每项限制都会指定一个生成另一相同类型的通知前必须经过的时间段。 有关详细信息，请参阅[配置通知限制](configure-notification-limits.md)。


## <a name="my-storage-reports-keep-failing-and-little-or-no-information-is-available-in-the-event-log-regarding-the-source-of-the-failure"></a>我的存储报告总是出现故障，并且事件日志中几乎没有或完全没有关于故障原因的可用信息。

-   **原因**：正在保存报表的卷可能已损坏。
-   **解决方案**：请在该卷上运行**chkdsk** ，然后尝试再次生成报告。

## <a name="my-file-screening-audit-reports-do-not-contain-any-information"></a>我的文件屏蔽审核报告中没有任何信息。

-   **原因**：可能是以下一种或多种原因：
    -   审核数据库未配置为记录文件屏蔽活动。
    -   审核数据库为空（即未记录任何文件屏蔽活动）。
    -   文件屏蔽审核报告的参数未从审核数据库中选择数据。
    
-   **解决方案**：在 "**文件屏蔽审核**" 选项卡上的 "**文件服务器资源管理器选项**" 对话框中，验证是否选中了 **"在审核数据库中记录文件屏蔽活动**" 复选框。
    -   有关记录文件屏蔽活动的详细信息，请参阅[配置文件屏蔽审核](configure-file-screen-audit.md)。
    -   若要配置文件屏蔽审核报告的默认参数，请参阅[配置存储报告](configure-storage-reports.md)。
    -   若要编辑计划报告任务或按需报告的报告参数，请参阅[存储报告管理](storage-reports-management.md)。

## <a name="the-used-and-available-values-for-some-of-the-quotas-i-have-created-do-not-correspond-to-the-actual-limit-setting"></a>我创建的部分配额的“已使用”和“可用”值与实际的“限制”设置不对应。

-   **原因**：你可能有一个嵌套配额，其中子文件夹的配额从其父文件夹的配额中派生了限制性更强的限制。 例如，可能将一个 100 兆字节 (MB) 的配额限制应用于父文件夹，并将一个 200 MB 的配额分别应用于其中的每一个子文件夹。 如果父文件夹中共存储了 50 MB 的数据（存储在其子文件夹中的数据总量），则每一个子文件夹将仅列出 50 MB 的可用空间。

-   **解决方案**：在 "**配额管理**" 下，单击 "**配额**"。 在**结果**窗格中，选择正在解决的配额项。 在**操作**窗格中，单击**查看影响文件夹的配额**，然后查找应用于父文件夹的配额。 这将能够确定哪些父文件夹配额拥有低于所选配额的存储限制设置。


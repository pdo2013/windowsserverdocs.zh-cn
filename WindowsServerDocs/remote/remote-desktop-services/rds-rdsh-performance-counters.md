---
title: 使用性能计数器来诊断在远程桌面会话主机上的应用程序响应能力问题
description: 您的应用程序运行速度缓慢 RDS 上？ 了解可用于诊断 RDSH 上的应用程序性能问题的性能计数器
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 09/19/2018
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 241b2b776a68cf5aec68a4d331201a07f0e5ea53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844648"
---
# <a name="use-performance-counters-to-diagnose-app-performance-problems-on-remote-desktop-session-hosts"></a>使用性能计数器来诊断在远程桌面会话主机上的应用程序性能问题

诊断的最困难问题之一是应用程序性能差 — 应用程序运行速度缓慢或没有响应。 传统上，您开始在诊断通过收集 CPU、 内存、 磁盘输入/输出和其他指标，然后使用 Windows 性能分析器等工具来尝试找出什么导致了问题。 遗憾的是在大多数情况下此数据不会帮助你确定根本原因，因为资源使用情况计数器具有常见和较大的变体。 这使得很难读取数据，并将其与所报告的问题相关联。 若要帮助您更多快速解决你的应用性能问题，我们添加了一些新的性能计数器 (可用[若要下载](#download-windows-server-insider-software)通过[Windows 预览体验计划](https://insider.windows.com)) 该度量值的用户输入的流。

用户输入延迟计数器可以帮助您快速识别错误 RDP 体验的最终用户的根本原因。 此计数器测量多长时间任何用户输入 （如鼠标或键盘的使用情况） 保持在队列中之前，过程选取和计数器在本地和远程会话中工作正常。

下图显示的大致表示用户输入流形式从客户端向应用程序。

![远程桌面-用户从用户远程桌面客户端应用程序的输入流](.\media\rds-user-input.png)

用户输入延迟计数器测量 （内的时间间隔） 最大增量正在排队的输入和时它将提取中的应用程序之间[传统的消息循环](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop)，下面的流程图中所示：

![远程桌面的用户输入延迟性能计数器流](.\media\rds-user-input-delay.png)

此计数器的一个重要细节是它在可配置时间间隔内报告的最大用户输入的延迟。 这是以访问应用程序，可能会影响重要和可见的操作，例如键入的速度的输入所需的最长时间。

例如下, 表中的用户输入的延迟会被报告为 1000 毫秒在此时间间隔内。 该计数器报告速度最慢的用户在因为的"慢速"的用户感觉速度最慢的输入时确定的间隔中输入的延迟 （最大） 他们的体验，不总的所有输入的平均速度。

|编号| 0 | 1 | 2 |
|------|---|---|---|
|延迟 |16 毫秒| 20 毫秒| 1000 毫秒|

## <a name="enable-and-use-the-new-performance-counters"></a>启用和使用新的性能计数器

若要使用这些新性能计数器，必须首先通过运行以下命令启用注册表项：

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> 如果在使用 Windows 10，版本 1809年或更高版本或 Windows Server 2019 或更高版本，无需启用注册表项。

接下来，重新启动服务器。 然后，打开性能监视器，并选择加号 （+），如以下屏幕截图中所示。

![远程桌面的屏幕截图显示了如何将用户添加输入延迟性能计数器](.\media\rds-add-user-input-counter-screen.png)

后执行该操作，应会看到添加计数器对话框中，可以在其中选择**每个进程的用户输入延迟**或**每个会话的用户输入延迟**。

![远程桌面的屏幕截图显示如何添加每个会话的用户输入的延迟](.\media\rds-user-delay-per-session.png)

![远程桌面的屏幕截图显示如何添加每个进程的用户输入的延迟](.\media\rds-user-delay-per-process.png)

如果选择**每个进程的用户输入延迟**，你将看到**选定对象的实例**（换而言之，进程） 中```SessionID:ProcessID <Process Image>```格式。

例如，如果计算器应用程序是否正在[会话 ID 为 1](https://msdn.microsoft.com/library/ms524326.aspx)，你将看到```1:4232 <Calculator.exe>```。

> [!NOTE]
> 包含不是所有进程都。 不会看到以系统身份运行的任何进程。

该计数器开始就立即将其添加报告用户输入的延迟。 请注意，最大的规模设置为 100 （毫秒） 默认情况下。 

![远程桌面的用户输入延迟，每个性能监视器中的进程的活动的示例](.\media\rds-sample-user-input-delay-perfmon.png)

接下来，让我们看看**每个会话的用户输入延迟**。 没有实例的每个会话 ID，并且其计数器显示在指定会话中的任何进程的用户输入的延迟。 此外，有两个名为"最大值"（最大用户输入延迟在所有会话之间） 和"Average"(平均 acorss 所有会话) 的实例。

此表显示了这些实例的可视示例。 （你可以获取相同的信息在 Perfmon 中切换到报表关系图类型。）

|类型的计数器|实例名|报告的延迟 （毫秒）|
|---------------|-------------|-------------------|
|每个进程的用户输入延迟|1:4232 <Calculator.exe>|  200|
|每个进程的用户输入延迟|2:1000 <Calculator.exe>|  16|
|每个进程的用户输入延迟|1:2000 <Calculator.exe>|  32|
|每个会话的用户输入延迟|1|    200|
|每个会话的用户输入延迟|2|    16|
|每个会话的用户输入延迟|平均值|  108|
|每个会话的用户输入延迟|最大|  200|

## <a name="counters-used-in-an-overloaded-system"></a>重载系统中使用计数器

现在让我们看看您将看到的内容在报表中应用的性能下降，如果。 下图显示了在 Microsoft Word 中远程工作的用户的读数。 在这种情况下，在 RDSH 服务器性能下降随着时间的推移，如更多的用户登录。

![远程桌面-运行 Microsoft Word 的 RDSH 服务器的示例性能图表](.\media\rds-user-input-perf-graph.png)

下面介绍了如何读取关系图的行：

- 粉红色的行显示服务器上登录的会话数。
- 红线是 CPU 使用率。
- 绿线是跨所有会话的最大用户输入的延迟。
- （显示为黑色，在此图中） 的蓝线表示的所有会话的平均用户输入的延迟。

您会注意到，没有与 CPU 峰值，并且用户输入的延迟-CPU 获取更多用途，如用户输入延迟会延长。 此外，随着更多的用户获取添加到系统中，获取更接近于 100%，从而导致更频繁的用户输入的延迟峰值 CPU 使用情况。 虽然此计数器是在服务器在其中运行由于资源不足的情况下非常有用，也可以使用跟踪与特定应用程序相关的用户输入的延迟

## <a name="configuration-options"></a>配置选项

在使用此性能计数器时要记住的重要一点是，它报告用户输入的延迟 1000 毫秒的时间间隔默认情况下。 如果到不同的任何内容 （如以下屏幕截图中所示） 设置性能计数器示例间隔属性，报告的值将不正确。

![远程桌面的性能监视器的属性](.\media\rds-user-input-perfmon-properties.png)

若要解决此问题，可以设置以下注册表项以匹配你想要使用的间隔 （以毫秒为单位）。 例如，如果我们将更改示例每隔 x 秒到 5 秒，我们需要将此项设置为 5000 毫秒。

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>如果在使用 Windows 10，版本 1809年或更高版本或 Windows Server 2019 或更高版本，无需设置 LagCounterInterval 若要解决的性能计数器。

我们还添加了几个可能会有所相同的注册表项下的项：

**LagCounterImageNameFirst** — 将此项设置为`DWORD 1`（默认值 0 或项不存在）。 这会对计数器名称更改为"映像名称 < SessionID:ProcessId >"。 例如，"资源管理器 < 1:7964 >。" 这是很有用，如果你想要按映像名称进行排序。

**LagCounterShowUnknown** — 将此项设置为`DWORD 1`（默认值 0 或项不存在）。 这将显示为服务或系统运行的任何进程。 某些进程会显示与设置为其会话"？。"

这是如果将这两个密钥打开如下所示：

![远程桌面-与这两个键上的性能监视器](.\media\rds-user-input-delay-with-two-counters.png)

## <a name="using-the-new-counters-with-non-microsoft-tools"></a>与非 Microsoft 工具中使用新的计数器

监视工具可以通过使用此计数器[Perfmon API](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx)。

## <a name="download-windows-server-insider-software"></a>下载 Windows Server Insider 软件

已注册预览体验成员可以直接导航到[Windows Server Insider Preview 下载页](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver)以获取最新的深入了解软件下载。  若要了解如何将注册为内部人员，请参阅[Server 入门](https://insider.windows.com/en-us/for-business-getting-started-server/)。

## <a name="share-your-feedback"></a>共享你的反馈

您可以提交反馈中心通过此功能的反馈。 选择**应用程序 > 所有其他应用**，包括"RDS 性能计数器，性能监视器"篇文章的标题中。

常规功能想法，请访问[RDS UserVoice 页面](https://aka.ms/uservoice-rds)。

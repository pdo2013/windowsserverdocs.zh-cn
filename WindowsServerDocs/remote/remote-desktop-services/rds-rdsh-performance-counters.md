---
title: 使用性能计数器来诊断远程桌面会话主机上的应用程序响应能力问题
description: 你的应用在 RDS 上是否运行缓慢？ 了解可用于诊断 RDSH 上的应用性能问题的性能计数器
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 07/11/2019
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 3eb1e4b6da971d788383b8facbf8bbcbe00a5953
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870906"
---
# <a name="use-performance-counters-to-diagnose-app-performance-problems-on-remote-desktop-session-hosts"></a>使用性能计数器来诊断远程桌面会话主机上的应用性能问题

> 适用于：Windows Server 2019、Windows 10

最难以诊断的问题之一是应用程序性能 — 应用程序运行缓慢或没有响应。 传统上，通过收集 CPU、内存、磁盘输入/输出和其他指标来启动诊断，然后使用 Windows Performance Analyzer 等工具来尝试找出问题的原因。 遗憾的是，在大多数情况下，此数据无法帮助你确定根本原因，因为资源消耗计数器具有频繁且较大的变化。 这样就难以读取数据并将其与报告的问题相关联。 为了帮助你快速解决应用性能问题，我们添加了可测量用户输入流的一些新性能计数器（可通过 [Windows 预览体验计划](https://insider.windows.com)[下载](#download-windows-server-insider-software)）。

>[!NOTE]
>“用户输入延迟”计数器仅与以下操作系统版本兼容：
> - Windows Server 2019 或更高版本
> - Windows 10 版本 1809 或更高版本

用户输入延迟计数器可以帮助你快速确定最终用户 RDP 不良体验的根本原因。 此计数器可测量任何用户输入（如鼠标或键盘使用）在被进程选取之前在队列中停留的时间，并且计数器同时可在本地和远程会话中运行。

下图大致表示了从客户端到应用程序的用户输入流。

![远程桌面 - 用户输入从用户远程桌面客户端流向应用程序](./media/rds-user-input.png)

“用户输入延迟”计数器可测量正在排队的输入和被[传统消息循环](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop)中的应用提取的输入之间的最大增量（在一个时间间隔内），如下面的流程图中所示：

![远程桌面 - 用户输入延迟性能计数器流](./media/rds-user-input-delay.png)

此计数器的一个重要细节是它在可配置的时间间隔内报告最大用户输入延迟。 这是输入到达应用程序所需的最长时间，可能会影响重要的可见操作（例如键入）的速度。

例如，在下表中，用户输入延迟在此时间间隔内会被报告为 1000 毫秒。 计数器报告该时间间隔内最慢的用户输入延迟，因为用户对“慢”的感知由体验到的最慢输入时间（最大值），而不是所有总输入的平均速度确定。

|编号| 0 | 1 | 2 |
|------|---|---|---|
|延迟 |16 毫秒| 20 毫秒| 1,000 毫秒|

## <a name="enable-and-use-the-new-performance-counters"></a>启用和使用新性能计数器

若要使用这些新性能计数器，必须首先通过运行以下命令来启用注册表项：

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> 如果使用的是 Windows 10 版本 1809 或更高版本或 Windows Server 2019 或更高版本，则无需启用注册表项。

接下来，重新启动该服务器。 然后打开性能监视器并选择加号 (+)，如以下屏幕截图中所示。

![远程桌面 - 显示如何添加用户输入延迟性能计数器的屏幕截图](./media/rds-add-user-input-counter-screen.png)

执行该操作后，应看到“添加计数器”对话框，你可以在其中选择“每个进程的用户输入延迟”  或“每个会话的用户输入延迟”  。

![远程桌面 - 显示如何添加每个会话的用户输入延迟的屏幕截图](./media/rds-user-delay-per-session.png)

![远程桌面 - 显示如何添加每个进程的用户输入延迟的屏幕截图](./media/rds-user-delay-per-process.png)

如果选择“每个进程的用户输入延迟”  ，你将看到采用 ```SessionID:ProcessID <Process Image>``` 格式的“选定对象的实例”  （换而言之，进程）。

例如，如果计算器应用正在[会话 ID 1](https://msdn.microsoft.com/library/ms524326.aspx) 中运行，你将看到 ```1:4232 <Calculator.exe>```。

> [!NOTE]
> 并非包含所有进程。 不会看到以系统身份运行的任何进程。

添加该计数器后，就会立即开始报告用户输入延迟。 请注意，最大规模默认设置为 100（毫秒）。 

![远程桌面 - 性能监视器中每个进程的用户输入延迟的活动示例](./media/rds-sample-user-input-delay-perfmon.png)

接下来，让我们看看“每个会话的用户输入延迟”  。 每个会话 ID 都有实例，并且其计数器显示指定会话中的任何进程的用户输入延迟。 此外，有两个名为“最大”（所有会话中的最大用户输入延迟）和“平均”（所有会话中的平均用户输入延迟）的实例。

此表显示了这些实例的可视示例。 （可以通过切换到报表图表类型来获取 Perfmon 中的相同信息。）

|计数器的类型|实例名|报告的延迟（毫秒）|
|---------------|-------------|-------------------|
|每个进程的用户输入延迟|1:4232 <Calculator.exe>|  200|
|每个进程的用户输入延迟|2:1000 <Calculator.exe>|  16|
|每个进程的用户输入延迟|1:2000 <Calculator.exe>|  32|
|每个会话的用户输入延迟|1|    200|
|每个会话的用户输入延迟|2|    16|
|每个会话的用户输入延迟|平均值|  108|
|每个会话的用户输入延迟|最大|  200|

## <a name="counters-used-in-an-overloaded-system"></a>重载系统中使用的计数器

现在，让我们看看在应用性能下降时将看到的报表内容。 下图显示了用户在 Microsoft Word 中远程工作的读数。 在这种情况下，当更多用户登录时，RDSH 服务器性能会随着时间的推移而下降。

![远程桌面 - 运行 Microsoft Word 的 RDSH 服务器的示例性能图表](./media/rds-user-input-perf-graph.png)

下面介绍了如何解读图表的线：

- 粉红色的线表示服务器上登录的会话数。
- 红色的线表示 CPU 使用率。
- 绿色的线表示所有会话中的最大用户输入延迟。
- 蓝色的线（在此图表中显示为黑色）表示所有会话中的平均用户输入延迟。

你会注意到，CPU 峰值与用户输入延迟之间存在关联，即当 CPU 使用率更高时，用户输入延迟就会增加。 此外，随着更多的用户添加到系统中， CPU 使用率更接近于 100%，从而导致更频繁的用户输入延迟峰值。 虽然此计数器在服务器用完资源的情况下非常有用，但也可以将其用于跟踪与特定应用程序相关的用户输入延迟。

## <a name="configuration-options"></a>配置选项

使用此性能计数器时需要记住的重要一点是，它在默认情况下以 1000 毫秒的时间间隔报告用户输入延迟。 如果将性能计数器示例间隔属性（如以下屏幕截图中所示）设置为其他属性，则报告的值将不正确。

![远程桌面 - 性能监视器的属性](./media/rds-user-input-perfmon-properties.png)

若要解决此问题，可以设置以下注册表项以匹配你想要使用的间隔（以毫秒为单位）。 例如，如果我们将每 x 秒示例更改为 5 秒，则需要将此项设置为 5000 毫秒。

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>如果使用的是 Windows 10 版本 1809 或更高版本或 Windows Server 2019 或更高版本，则无需设置 LagCounterInterval 即可修复性能计数器。

我们还在同一注册表项下添加了几个可能会有所帮助的项：

**LagCounterImageNameFirst** — 将此项设置为 `DWORD 1`（默认值 0 或项不存在）。 这会将计数器名称更改为“映像名称 <SessionID:ProcessId>”。 例如，“资源管理器 <1:7964>”。 如果希望按映像名称进行排序，这将很有用。

**LagCounterShowUnknown** — 将此项设置为 `DWORD 1`（默认值 0 或项不存在）。 这将显示作为服务或系统运行的任何进程。 某些进程会显示设置为“？”的会话。

下面是启用两个项时的显示情况：

![远程桌面 - 启用两个项的性能监视器](./media/rds-user-input-delay-with-two-counters.png)

## <a name="using-the-new-counters-with-non-microsoft-tools"></a>将新计数器与非 Microsoft 工具结合使用

监视工具可以通过使用 [Perfmon API](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx) 来使用此计数器。

## <a name="download-windows-server-insider-software"></a>下载 Windows Server Insider 软件

已注册预览体验成员可以直接导航到 [Windows Server Insider 预览版下载页](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver)以获取最新 Insider 软件下载。  若要了解如何注册为预览体验成员，请参阅[服务器入门](https://insider.windows.com/en-us/for-business-getting-started-server/)。

## <a name="share-your-feedback"></a>共享你的反馈

可以通过反馈中心提交此功能的反馈。 选择“应用”>“所有其他应用”  ，并在帖子标题中包括“RDS 性能计数器 - 性能监视器”。

若要了解常规功能想法，请访问 [RDS UserVoice 页面](https://aka.ms/uservoice-rds)。

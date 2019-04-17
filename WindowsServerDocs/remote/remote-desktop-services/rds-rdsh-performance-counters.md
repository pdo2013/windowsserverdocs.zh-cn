---
title: 使用性能计数器诊断远程桌面会话主机上的应用程序响应性问题
description: 你的应用运行缓慢上 RDS？ 了解有关可用于诊断 rdsh 应用性能问题的性能计数器
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
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133713"
---
# 使用性能计数器诊断远程桌面会话主机上的应用性能问题

要诊断最棘手问题之一是应用程序性能差-应用程序运行缓慢或未响应。 传统上来讲，你开始收集 CPU、 内存、 磁盘输入/输出，和其他指标你诊断，然后使用 Windows Performance Analyzer 等工具找出导致该问题。 遗憾的是在大多数情况下此数据不会帮助你确定的根本原因，因为资源消耗量计数器频繁和大型变体。 这使得很难读取数据，并将它与所报告问题关联起来。 为了帮助你更快速地解决你的应用的性能问题，我们添加了一些新的性能计数器 (可用[来下载](#download-windows-server-insider-software)通过[Windows 预览体验计划](https://insider.windows.com)) 的测量用户输入的流。

用户输入延迟计数器可以帮助你快速确定根本原因的错误 RDP 体验的最终用户。 此计数器测量多长时间任何用户输入 （如鼠标或键盘的使用情况） 停留在队列中它由一个过程中，选取之前，并在本地和远程会话中的工作原理计数器。

下图显示了粗略表示的用户输入流从客户端应用程序。

![远程桌面的用户从用户远程桌面客户端应用程序的输入流](.\media\rds-user-input.png)

用户输入延迟计数器测量 （内的时间间隔） 之间进行排队的输入和它由在[传统的消息循环](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop)中，应用选取的最大增量，如下面的流程图中所示：

![远程桌面的用户输入延迟性能计数器流程](.\media\rds-user-input-delay.png)

此计数器的一个重要的细节是它在可配置的时间间隔内报告的最大用户输入的延迟。 这是访问应用程序，可能会影响的重要并且可见的操作，如键入速度的输入所需的最长时间。

例如，在以下表中，用户输入的延迟将报告为 1000 毫秒在此时间间隔内。 计数器报告速度最慢的用户在因为"较慢"的用户感觉由的速度最慢的输入时间间隔中输入延迟 （最多） 他们的体验，不所有的总输入的平均速度。

|数字| 0 | 1 | 2 |
|------|---|---|---|
|延迟 |16 毫秒| 20 ms| 1000 ms|

## 启用并使用新的性能计数器

若要使用这些新的性能计数器，你必须首先通过运行此命令中启用注册表项：

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> 如果你使用的 Windows 10 1809年或更高版本或 Windows Server 2019 或更高版本，则无需启用的注册表项。

接下来，重启服务器。 然后，打开性能监视器，并选择加号 （+），如下面的屏幕截图中所示。

![远程桌面的屏幕截图显示了如何将用户添加输入延迟性能计数器](.\media\rds-add-user-input-counter-screen.png)

这样一来之后, 你应该看到添加计数器对话框中，你可以在其中选择**每个进程的用户输入延迟**或**每个会话的用户输入延迟**。

![远程桌面的屏幕截图显示了如何添加每个会话的用户输入的延迟](.\media\rds-user-delay-per-session.png)

![远程桌面的屏幕截图显示了如何添加每个进程的用户输入的延迟](.\media\rds-user-delay-per-process.png)

如果你选择**每个进程的用户输入延迟**，你将看到**所选对象的实例**（换言之，进程），在```SessionID:ProcessID <Process Image>```格式。

例如，如果在[会话 ID 1](https://msdn.microsoft.com/library/ms524326.aspx)中运行的计算器应用，你将看到```1:4232 <Calculator.exe>```。

> [!NOTE]
> 并非所有进程都也包括在内。 你将不会看到任何作为系统运行的进程。

计数器启动报告用户输入的延迟，只要你添加它。 请注意的最大缩放默认设置为 100 （毫秒为单位）。 

![远程桌面的每个进程性能监视器中用户的输入延迟的活动的示例](.\media\rds-sample-user-input-delay-perfmon.png)

接下来，让我们看一下**每个会话的用户输入延迟**。 每个会话 ID、 实例，并且它们的计数器指定会话内显示的任何进程的用户输入的延迟。 此外，还有两个名为"最大值"（最大用户输入延迟跨所有会话） 和"Average"(平均 acorss 所有会话) 的实例。

此表显示了这些实例的视觉示例。 （你可以获取相同信息在性能监视器通过切换到报告图类型。）

|计数器类型|实例名称|报告的延迟 （毫秒）|
|---------------|-------------|-------------------|
|每个进程的用户输入延迟|1:4232 < Calculator.exe >|  200|
|每个进程的用户输入延迟|2:1000 < Calculator.exe >|  16|
|每个进程的用户输入延迟|1:2000 < Calculator.exe >|  32|
|每个会话的用户输入延迟|1|    200|
|每个会话的用户输入延迟|2|    16|
|每个会话的用户输入延迟|正常|  108|
|每个会话的用户输入延迟|最大值|  200|

## 重载系统中使用的计数器

现在让我们看一下什么如果你将看到在报告中的应用的性能下降。 下图显示了 Microsoft Word 中的远程工作的用户的读数。 在此情况下，RDSH 服务器性能降低随着时间的推移，因为多个用户登录。

![远程桌面的示例性能图的 RDSH 服务器运行 Microsoft Word](.\media\rds-user-input-perf-graph.png)

下面介绍了如何读取音频图的行：

- 粉红色行显示的服务器上登录的会话的数量。
- 红线是 CPU 使用率。
- 绿色行是所有会话的最大用户输入的延迟。
- （显示为黑色，在此图中） 的蓝色行表示所有会话的平均用户输入的延迟。

你会注意到 CPU 峰值和用户输入的延迟之间的关系 — 用户使用更多的 CPU 越高，输入延迟增加。 此外，随着更多的用户获取添加到系统中，获取接近 100%，从而导致更频繁的用户输入的延迟峰值 CPU 使用率。 虽然此计数器在其中服务器耗尽资源的情况下非常有用，还可以使用要跟踪用户相关的特定应用程序的输入的延迟

## 配置选项

在使用此性能计数器时要记住的重要的是，它报告用户输入的延迟的 1000 毫秒间隔默认情况下。 如果你设置的性能计数器示例间隔属性 （如以下屏幕截图中所示） 对不同的任何内容，报告的值将不正确。

![远程桌面的性能监视器的属性](.\media\rds-user-input-perfmon-properties.png)

若要解决此问题，你可以设置以下注册表项以匹配你想要使用的时间间隔 （以毫秒为单位）。 例如，如果我们将更改示例每隔 x 秒为 5 秒，我们需要将此项设置为 5000 ms。

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>如果你使用的 Windows 10 1809年或更高版本或 Windows Server 2019 或更高版本，你无需设置 LagCounterInterval 解决性能计数器。

我们还添加了几个很有帮助的相同的注册表项下的密钥：

**LagCounterImageNameFirst** -将此项设置为`DWORD 1`（默认值为 0 或密钥不存在）。 这会计数器名称更改为"图像 Name < SessionID:ProcessId >"。 例如，"资源管理器 < 1:7964 >。" 这非常有用，如果你想要的图像名称进行排序。

**LagCounterShowUnknown** -将此项设置为`DWORD 1`（默认值为 0 或密钥不存在）。 这将显示任何作为服务或系统运行的进程。 某些进程将显示与他们的会话设置为"？。"

这是其外观如果你打开两个密钥：

![远程桌面的性能监视器与在这两个密钥](.\media\rds-user-input-delay-with-two-counters.png)

## 非 Microsoft 工具使用新的计数器

监视工具可以使用[性能监视器 API](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx)中使用此计数器。

## 下载 Windows Server 预览体验成员软件

注册预览体验成员可以直接导航到[Windows Server Insider Preview 下载页面](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver)以获取最新的预览体验成员软件下载。  若要了解如何注册成为预览体验成员，请参阅[服务器入门](https://insider.windows.com/en-us/for-business-getting-started-server/)。

## 共享你的反馈

你可以提交反馈中心通过此功能的反馈。 选择**应用 > 所有其他应用**，包括"RDS 性能计数器-性能监视器"在文章的标题。

常规功能的建议，请访问[RDS UserVoice 页面](https://aka.ms/uservoice-rds)。

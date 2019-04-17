---
title: 提高性能的 SMB 直通的文件服务器
description: 介绍了 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2016 中的 SMB 直通功能。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: ed8fd5b4114fc9fd9c7dc278a98cea8cc67a8749
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239224"
---
# SMB 直通

>适用于： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2016

Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2016 包括称为 SMB 直通支持使用具有远程直接内存访问 (RDMA) 功能的网络适配器的功能。 具有 RDMA 的网络适配器才可以全速使用非常低的延迟，同时使用非常少 CPU。 对于 HYPER-V 或 Microsoft SQL Server 等工作负荷，这使远程文件服务器以类似于本地存储。 SMB 直通包括：

- 增加了的吞吐量： 利用高速网络的网络适配器位置坐标的大量的行速度的数据传输的完整吞吐量。
- 低延迟： 提供非常快速响应网络请求，并且结果是，使远程文件存储感觉像是直接附加的数据块存储。
- 低 CPU 使用率： 使用更少 CPU 周期，当通过网络，离开的详细信息的传输数据电源适用于服务器应用程序。

SMB 直通自动配置的 Windows Server 2012 R2 和 Windows Server 2012。

## SMB 多通道和 SMB 直通

SMB 多通道是负责检测网络适配器的 RDMA 功能，以启用 SMB 直通的功能。 如果 SMB 多通道，没有 SMB 使用支持 RDMA 的网络适配器 （所有网络适配器都提供新的 RDMA 堆栈以及的 TCP/IP 堆栈） 的常规 TCP/IP。

使用 SMB 多通道 SMB 检测到的网络适配器是否具有 RDMA 功能，然后为该单个会话 （每两个接口） 创建多个 RDMA 连接。 这样，SMB，以使用高吞吐量、 低延迟、 和提供支持 RDMA 的网络适配器的低 CPU 使用率。 如果你使用的多个 RDMA 接口，它还提供容错能力。

>[!NOTE]
>如果你打算使用的 RDMA 功能的网络适配器，不应团队支持 RDMA 的网络适配器。 当成组，网络适配器不会支持 RDMA。
>创建至少一个 RDMA 网络连接后，不会再使用用于原始协议协商 TCP/IP 连接。 但是，在 RDMA 网络连接失败的情况下，将保留 TCP/IP 连接。

## 要求

SMB 直通有以下要求：

- 至少两台计算机运行 Windows Server 2012 R2 或 Windows Server 2012
- RDMA 功能与一个或多个网络适配器。

### 使用 SMB 直通时的注意事项

- 你可以使用 SMB 直通故障转移群集中;但是，你需要确保用于客户端访问的群集网络是足够用于 SMB 直通。 故障转移群集支持客户端访问使用多个网络，以及网络适配器，则 RSS （接收方缩放） 的支持和支持 RDMA 的。
- 你可以使用 SMB 直通上的 HYPER-V 管理操作系统以支持基于 SMB，使用 HYPER-V 和提供存储到虚拟机使用 HYPER-V 存储堆栈。 但是，支持 RDMA 的网络适配器不直接向 HYPER-V 客户端公开。 如果将支持 RDMA 的网络适配器连接到虚拟交换机，不会支持 RDMA 的从交换机的虚拟网络适配器。
- 如果你禁用 SMB 多通道，SMB 直通也会禁用。 由于 SMB 多通道检测网络适配器功能，并确定是否支持 RDMA 的网络适配器，SMB 直通无法由客户端如果使用 SMB 多通道处于禁用状态。
- SMB 直通上不支持 Windows 直角 SMB 直通需要支持支持 RDMA 的网络适配器，这是仅适用于 Windows Server 2012 R2 和 Windows Server 2012。
- 低级别版本的 Windows Server 上不受支持 SMB 直通。 它是仅在 Windows Server 2012 R2 和 Windows Server 2012 上受支持。

## 启用和禁用 SMB 直通

SMB 直通默认启用的安装 Windows Server 2012 R2 或 Windows Server 2012 时。 SMB 客户端自动检测并使用多个网络连接，如果适当配置进行标识。

### 禁用 SMB Direct

通常，你不需要禁用 SMB 直通，但是，可以通过运行以下 Windows PowerShell 脚本之一禁用它。

若要为特定接口禁用 RDMA，请键入：

```PowerShell
Disable-NetAdapterRdma <name>
```

若要为所有接口禁用 RDMA，请键入：

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Disabled
```

客户端或服务器上禁用 RDMA 时，系统不能使用它。 *网络直接*是 Windows Server 2012 R2 和 Windows Server 2012 基本网络的支持 RDMA 的接口的内部名称。

### 重新启用 SMB 直通

RDMA 之后，你可以重新启用它通过运行以下 Windows PowerShell 脚本之一。

若要重新启用 RDMA，对于特定接口，键入：

```PowerShell
Enable-NetAdapterRDMA <name>
```

若要重新启用 RDMA，对于所有接口，请键入：

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Enabled
```

你需要客户端和服务器以再次使用开始菜单上启用 RDMA。

## 测试的 SMB 直通的性能

你可以测试性能通过使用以下过程之一的工作方式。

### 带有和不使用 SMB 直通文件副本进行比较

下面介绍了如何测量的 SMB 直通提高的吞吐量：

1. 配置 SMB 直通
2. 测量的时间运行的大型文件副本使用 SMB 直通。
3. 禁用 RDMA 网络适配器上，请参阅[启用和禁用 SMB 直通](#enabling-and-disabling-smb-direct)。
4. 测量的时间，无需使用 SMB 直通运行较大的文件复制过程。
5. 重新的网络适配器上启用 RDMA，比较两个结果。
6. 若要避免的缓存的影响，应执行以下操作：
    1. 复制大量数据 （内存是能够处理多个数据）。
    2. 使用作为练习，然后计时的第二个副本的第一个副本两次，复制数据。
    3. 重启服务器和客户端每个测试之前，以确保它们在类似的情况下运行。

### 使用 SMB 直通文件复制过程失败之一多个网络适配器

下面介绍了如何确认 SMB 直通的故障转移功能：

1. 确保的 SMB 直通正常运行在多个网络适配器配置。
2. 运行较大的文件复制过程。 复制运行时，模拟网络路径之一的故障，通过断开连接电缆之一 （或通过禁用其中一个网络适配器）。
3. 确认文件复制继续使用一个剩余的网络适配器，并且没有文件复制错误。

>[!NOTE]
>若要避免不使用 SMB 直通的工作负荷的失败，请确保不有使用断开连接的网络路径的任何其他工作负荷。

## 详细信息

- [服务器消息块概述](file-server-smb-overview.md)
- [增加服务器、 存储和网络可用性： 方案概述](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [部署基于 SMB 的 Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)

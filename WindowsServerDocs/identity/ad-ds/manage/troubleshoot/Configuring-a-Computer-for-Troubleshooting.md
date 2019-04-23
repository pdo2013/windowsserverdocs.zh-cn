---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: 配置计算机进行故障排除
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1acb5f7d309d58ed4a5a3aca6bb89f01c0cbf933
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854118"
---
# <a name="configuring-a-computer-for-troubleshooting"></a>配置计算机进行故障排除

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

使用高级故障排除技术来识别并修复 Active Directory 问题之前，配置计算机，以进行故障排除。 此外应基本了解故障排除概念、 过程和工具。

有关 Windows Server 的监视工具的信息，请参阅循序渐进指南[性能和可靠性监视 Windows Server 中](https://go.microsoft.com/fwlink/?LinkId=123737)

## <a name="configuration-tasks-for-troubleshooting"></a>配置任务进行故障排除

若要配置你的计算机进行故障排除 Active Directory 域服务 (AD DS)，请执行以下任务：

### <a name="install-remote-server-administration-tools-for-ad-ds"></a>用于 AD DS 安装远程服务器管理工具

在安装 AD DS 以创建一个域控制器时，将自动安装用于管理 AD DS 管理工具。 如果你想要通过不是域控制器的计算机远程管理的域控制器，您可以在成员服务器或正在运行受支持的 Windows 版本的工作站上安装远程服务器管理工具 (RSAT)。 RSAT 替换从 Windows Server 2003 的 Windows 支持工具。

有关安装 RSAT 的信息，请参阅文章[远程服务器管理工具](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools)。

### <a name="configure-reliability-and-performance-monitor"></a>配置可靠性和性能监视器

Windows Server 包含 Windows 可靠性和性能监视器，它是 Microsoft 管理控制台 (MMC) 管理单元的单元，结合了以前独立工具，包括性能日志和警报，服务器性能顾问的功能和系统监视器。 此管理单元提供了用于自定义数据收集器集和事件跟踪会话的图形用户界面 (GUI)。

可靠性和性能监视器还包含可靠性监视器，mmc 管理单元跟踪对系统的更改并将其与更改比较在系统稳定性，提供其关系的图形视图。

### <a name="set-logging-levels"></a>设置日志记录级别

如果您在事件查看器中的目录服务日志中收到的信息不足以进行故障排除，通过使用相应的注册表项中引发的日志记录级别**HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics**。

默认情况下，所有条目的日志记录级别设置为**0**，它提供最少的信息。 最高的日志记录级别是**5**。 增加一个条目的级别会使其他以目录服务事件日志中记录的事件。

使用以下过程来更改诊断条目的日志记录级别。 必须至少具有 **Domain Admins** 中的成员身份或同等身份才能完成此过程。

> [!WARNING]
> 除非万不得已，否则建议不要直接编辑注册表。 对注册表的修改不会验证通过注册表编辑器或通过 Windows 之前应用，且结果是，可以存储不正确的值。 这可以在系统中导致不可恢复的错误。 如果可能，请使用组策略或其他 Windows 工具，如 MMC 管理单元来完成任务，而不是直接编辑注册表。 如果必须编辑注册表，请格外小心。
>

若要更改的诊断条目的日志记录级别

1. 单击**启动** > **运行**> 类型**regedit** > 单击**确定**。
2. 导航到你想要设置日志记录的条目。
   * 示例：HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics
3. 双击该条目，然后在**Base**，单击**十进制**。
4. 在中**值**，键入一个整数**0**通过**5**，然后单击**确定**。

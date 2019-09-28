---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: 配置计算机进行故障排除
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8e11883de9f89d0b95ed0fc35b4f5f3941ef82a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71368904"
---
# <a name="configuring-a-computer-for-troubleshooting"></a>配置计算机进行故障排除

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用高级故障排除技术识别和修复 Active Directory 问题之前，请配置计算机以进行故障排除。 您还应该对疑难解答的概念、过程和工具有一个基本的了解。

有关适用于 Windows Server 的监视工具的信息，请参阅[Windows server 中的性能和可靠性监视](https://go.microsoft.com/fwlink/?LinkId=123737)循序渐进指南

## <a name="configuration-tasks-for-troubleshooting"></a>疑难解答的配置任务

若要将计算机配置为进行故障排除 Active Directory 域服务（AD DS），请执行以下任务：

### <a name="install-remote-server-administration-tools-for-ad-ds"></a>为 AD DS 安装远程服务器管理工具

安装 AD DS 创建域控制器时，将自动安装用于管理 AD DS 的管理工具。 如果要从非域控制器的计算机远程管理域控制器，可以在运行受支持的 Windows 版本的成员服务器或工作站上安装远程服务器管理工具（RSAT）。 RSAT 替换 Windows Server 2003 中的 Windows 支持工具。

有关安装 RSAT 的信息，请参阅文章[远程服务器管理工具](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools)。

### <a name="configure-reliability-and-performance-monitor"></a>配置可靠性和性能监视器

Windows Server 包括 Windows 可靠性和性能监视器，这是一个 Microsoft 管理控制台（MMC）管理单元，它结合了以前的独立工具（包括性能日志和警报、服务器性能顾问、和系统监视器。 此管理单元提供了图形用户界面（GUI），用于自定义数据收集器集和事件跟踪会话。

可靠性和性能监视器还包含可靠性监视器，这是一个 MMC 管理单元，用于跟踪系统的更改并将其与系统稳定性的变化进行比较，从而提供其关系的图形视图。

### <a name="set-logging-levels"></a>设置日志记录级别

如果在目录服务登录事件查看器中收到的信息不足以进行故障排除，请在 HKEY_LOCAL_ 中使用相应的注册表项提高日志记录级别。 **MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics**。

默认情况下，所有条目的日志记录级别均设置为**0**，这将提供最小数量的信息。 最高日志记录级别为**5**。 增大条目的级别会导致在目录服务事件日志中记录其他事件。

使用以下过程来更改诊断条目的日志记录级别。 必须至少具有 **Domain Admins** 中的成员身份或同等身份才能完成此过程。

> [!WARNING]
> 除非万不得已，否则建议不要直接编辑注册表。 注册表编辑器或 Windows 在应用之前对其进行了修改，因此，可能会存储不正确的值。 这可能会导致系统中出现不可恢复的错误。 如果可能，请使用组策略或其他 Windows 工具（如 MMC 管理单元）来完成任务，而不是直接编辑注册表。 如果必须编辑注册表，请格外小心。
>

更改诊断条目的日志记录级别

1. 单击 "**开始** > "**运行**> 键入**Regedit** > 单击 **"确定"** 。
2. 导航到要为其设置日志记录的条目。
   * 示例：HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics
3. 双击条目，然后在 "**基本**" 中单击 "**十进制**"。
4. 在 "**值**" 中，键入从**0**到**5**的整数，然后单击 **"确定"** 。

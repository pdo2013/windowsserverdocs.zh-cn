---
title: pwlauncher
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0917bb7b-408a-40f7-a1c5-20e94c10d38b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8ec9748056b296bb0c74250b36c762fb86fa90ad
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564655"
---
# <a name="pwlauncher"></a>pwlauncher



启用或禁用 Windows To Go 启动选项 (pwlauncher)。 **Pwlauncher**命令行工具，可将计算机配置为自动启动到 Windows To Go 工作区 （假设某个不存在），而无需输入您的固件或更改您的启动选项。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
Pwlauncher {/enable | /disable}
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/enable|启用 Windows To Go 启动选项，以便从 USB 设备存在时，计算机将自动启动|
|/disable|禁用 Windows To Go 启动选项，以便在计算机无法启动从 USB 设备，除非在固件中手动配置。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

用户想要使用 Windows To Go 的最大的障碍获取从 USB 启动其计算机。 这通常可通过输入固件，并尝试不同的配置选项，直到计算机正确配置。 这不是一项对于大多数用户的简单任务，是非常危险，因为固件包含可呈现系统不可用，如果使用不正确的选项。 若要帮助缓解此问题，Windows 8and 更高版本操作系统包含一个名为"Windows To Go 启动选项"功能，允许用户从 Windows 中从 USB 启动其计算机配置-而无需不断输入其固件，只要其固件支持从 USB 启动。 启用 system 始终从 USB 启动首次有你应考虑的影响。 例如，可能会无意中启动包含恶意软件的 USB 设备来破坏系统，或多个 USB 驱动器无法插入的会导致启动冲突。 出于此原因，默认配置包含 Windows To Go 启动选项默认情况下禁用。 此外，需要管理员权限来配置 Windows To Go 启动选项。 如果你启用 Windows To Go 启动选项使用 pwlauncher 命令行工具或**更改 Windows To Go 启动选项**，计算机将尝试从其之前插入计算机的任何 USB 设备启动的应用已启动。

## <a name="BKMK_examples"></a>示例

下面的示例演示如何使用**pwlauncher**命令以启用从 USB 启动：
```
Pwlauncher /enable
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
---
title: gpupdate
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fd4e567-2ce1-4637-b611-c2f0895e5708
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 994e37ebd972d881e06bdb99d5256e75096ccd81
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375641"
---
# <a name="gpupdate"></a>gpupdate

更新组策略设置。 有关如何使用此命令的示例，请参阅[示例](#examples)。

## <a name="syntax"></a>语法

```
gpupdate [/target:{Computer | User}] [/force] [/wait:<VALUE>] [/logoff] [/boot] [/sync] [/?]
```

### <a name="parameters"></a>Parameters

|     参数     |                                                                                                                                                                                                                                                                                                                             描述                                                                                                                                                                                                                                                                                                                             |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /target： {Computer |                                                                                                                                                                                                                                                                                                                                用户                                                                                                                                                                                                                                                                                                                                |
|      /force       |                                                                                                                                                                                                                                                                                   重新应用所有策略设置。 默认情况下，仅应用已更改的策略设置。                                                                                                                                                                                                                                                                                    |
|  /wait： \<VALUE >   | 设置在返回到命令提示符之前等待策略处理完成的秒数。 超过时间限制时，将显示命令提示符，但会继续进行策略处理。 默认值为600秒。 值**0**表示不等待。 值 **-1**表示无限期等待。</br>在脚本中，通过将此命令与指定的时间限制一起使用，可以运行**gpupdate** ，并继续执行不依赖于**gpupdate**完成的命令。 或者，你可以使用此命令，但不指定时间限制，让**gpupdate**在依赖于它的其他命令运行之前运行。 |
|      /logoff      |                                                                                                                                   在更新组策略设置后导致注销。 这对于不在后台更新周期处理策略但在用户登录时执行进程策略的客户端扩展组策略是必需的。 示例包括用户目标的软件安装和文件夹重定向。 如果没有调用需要注销的扩展，此选项将不起作用。                                                                                                                                    |
|       /boot       |                                                                                                                                       在应用组策略设置后重新启动计算机。 对于不在后台更新循环上处理策略但在计算机启动时执行进程策略的客户端扩展组策略，这是必需的。 示例包括以计算机为目标的软件安装。 如果没有调用需要重新启动的扩展，此选项将不起作用。                                                                                                                                        |
|       /sync       |                                                                                                                                                                              导致同步完成下一个前台策略应用程序。 前台策略在计算机启动和用户登录时应用。 可以使用 **/target**参数为用户和/或计算机指定此项。 如果你指定了 **/force**和 **/wait**参数，则会将其忽略。                                                                                                                                                                               |
|        /?         |                                                                                                                                                                                                                                                                                                                在命令提示符下显示帮助。                                                                                                                                                                                                                                                                                                                 |

## <a name="remarks"></a>备注

-   **Gpupdate**命令在 windows Server 2008 R2、windows server 2008、Windows 7 旗舰版、Windows 7 专业版、Windows vista 旗舰版、Windows vista Enterprise 和 Windows Vista 企业版中可用。

## <a name="examples"></a>示例

强制对所有组策略设置进行后台更新，不管这些设置是否已更改。

```
gpupdate /force
```

#### <a name="additional-references"></a>其他参考

-   [组策略技术中心](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [命令行语法项](command-line-syntax-key.md)
---
title: waitfor
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a48ef70d-4d28-4035-b6b0-7d7b46ac2157
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21ced4a9ef0dd7dac5f6c4fc6f171d99fa516c07
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440311"
---
# <a name="waitfor"></a>waitfor



发送或等待一个信号的系统上。 **Waitfor**用来同步计算机的网络。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
waitfor [/s <Computer> [/u [<Domain>\]<User> [/p [<Password>]]]] /si <SignalName>
waitfor [/t <Timeout>] <SignalName>
```

## <a name="parameters"></a>Parameters

|       参数       |                                                                                         描述                                                                                          |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s\<计算机 >     | 指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。 此参数适用于所有文件和命令中指定的文件夹。 |
| /u [\<域 >\]<User> |                              运行脚本，使用指定的用户帐户的凭据。 默认情况下**waitfor**使用当前用户的凭据。                               |
|   /p [\<Password>]    |                                                    指定在指定的用户帐户的密码 **/u**参数。                                                     |
|          /si          |                                                                        将通过网络发送指定的信号。                                                                        |
|     /t \<Timeout>     |                                              指定的信号等待秒的数。 默认情况下**waitfor**将无限期等待。                                               |
|     \<SignalName>     |                                                指定信号的**waitfor**等待或发送。 *信号名称*不区分大小写。                                                 |
|          /?           |                                                                             在命令提示符下显示帮助。                                                                             |

## <a name="remarks"></a>备注

-   信号名称不能超过 225 个字符。 有效字符包括 a-z、 A-Z、 0-9 和 ASCII 扩展字符集 (128-255)。
-   如果不使用 **/s**，信号广播的域中的所有系统。 如果您使用 **/s**，仅对指定的系统发送信号。
-   可以运行的多个实例**waitfor**在一台计算机，但每个实例**waitfor**必须等待其他信号。 只有一个实例**waitfor**可以等待给定计算机上的给定信号。
-   您可以通过使用手动激活信号 **/si**命令行选项。
-   **Waitfor**运行仅在 Windows XP 和服务器运行 Windows Server 2003 操作系统的系统，但它可以将信号发送到运行 Windows 操作系统的任何计算机。
-   如果它们处于与发送信号的计算机位于同一域，计算机才可接收信号。
-   可以使用**waitfor**时测试软件版本。 例如，编译计算机可以将信号发送到多台计算机运行**waitfor**编译已成功完成后。 在收到信号，批处理文件，包括**waitfor**可以指示要立即开始安装软件或编译的版本上运行测试的计算机。

## <a name="BKMK_examples"></a>示例

若要等待，直到收到"espresso\build007"信号，请键入：
```
waitfor espresso\build007
```
默认情况下**waitfor**无限期地等待一个信号。

若要等待 10 秒作为"espresso\compile007"信号接收超时前，请键入：
```
waitfor /t 10 espresso\build007
```
若要手动激活"espresso\build007"信号，请键入：
```
waitfor /si espresso\build007
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
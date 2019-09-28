---
title: openfiles
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c3be561d-a11f-4bf1-9835-8e4e96fe98ec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38b1d27b86551c6d4cd9e6b1ad87bfc0e8dd221d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372499"
---
# <a name="openfiles"></a>openfiles



使管理员能够查询、显示或断开在系统上打开的文件和目录。 还启用或禁用 "系统维护对象列表" 全局标志。

本主题包括有关以下命令的信息：
-   [openfiles/disconnect](#BKMK_disconnect)
-   [openfiles/query](#BKMK_query)
-   [openfiles/local](#BKMK_local)

## <a name="BKMK_disconnect"></a>openfiles/disconnect

允许管理员断开通过共享文件夹远程打开的文件和文件夹。

### <a name="syntax"></a>语法

```
openfiles /disconnect [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/id <OpenFileID>] | [/a <AccessedBy>] | [/o {read | write | read/write}]} [/op <OpenFile>]
```

### <a name="parameters"></a>Parameters

|            参数             |                                                                                                                                 描述                                                                                                                                  |
|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s \<System >           | 指定要连接到的远程系统（按名称或 IP 地址）。 不要使用反斜杠。 如果不使用 **/s**选项，则默认情况下会在本地计算机上执行命令。 此参数适用于命令中指定的所有文件和文件夹。 |
|    /u [@no__t > \] @ no__t-2     |                                                          使用指定用户帐户的权限执行命令。 如果不使用 **/u**选项，则默认情况下会使用系统权限。                                                           |
|         /p [\<Password >]         |                                               指定在 **/u**选项中指定的用户帐户的密码。 如果不使用 **/p**选项，则在执行命令时，将出现密码提示。                                                |
|        /id \<OpenFileID >         |                                       断开打开的文件的指定文件 ID。 通配符（ **&#42;** ）可用于此参数。</br>注意:可以使用**openfiles/query**命令查找文件 ID。                                       |
|         /a \<AccessedBy >         |                                                断开与*AccessedBy*参数中指定的用户名关联的所有打开的文件。 通配符（ **&#42;** ）可用于此参数。                                                 |
| /o {读取 \| 写入 \| 读取/写入} |                                               断开所有打开的文件与指定的打开模式值的连接。 有效值为 "读取"、"写入" 或 "读/写"。 通配符（ **&#42;** ）可用于此参数。                                                |
|         /op \<OpenFile >          |                                                           断开由特定打开的文件名创建的所有打开的文件连接。 通配符（ **&#42;** ）可用于此参数。                                                           |
|                /?                |                                                                                                                     在命令提示符下显示帮助。                                                                                                                     |

### <a name="examples"></a>示例

若要断开所有打开的文件（ID 26843578），请键入：
```
openfiles /disconnect /id 26843578
```
要断开用户 "hiropln" 访问的所有打开的文件和目录的连接，请键入：
```
openfiles /disconnect /a hiropln
```
若要将所有打开的文件和目录与读/写模式断开连接，请键入：
```
openfiles /disconnect /o read/write
```
若要将目录与打开的文件名 "C:\TestShare @ no__t" 断开连接，而不考虑访问该目录的用户，请键入：
```
openfiles /disconnect /a * /op "c:\testshare\"
```
要断开用户 "hiropln" 正在访问的远程计算机 "srvmain" 上所有打开的文件的连接，请键入：
```
openfiles /disconnect /s srvmain /u maindom\hiropln /id *
```

## <a name="BKMK_query"></a>openfiles/query

查询并显示所有打开的文件。

### <a name="syntax"></a>语法

```
openfiles /query [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] [/fo {TABLE | LIST | CSV}] [/nh] [/v]
```

### <a name="parameters"></a>Parameters

|          参数           |                                                                                                                                 描述                                                                                                                                  |
|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         /s \<System >         | 指定要连接到的远程系统（按名称或 IP 地址）。 不要使用反斜杠。 如果不使用 **/s**选项，则默认情况下会在本地计算机上执行命令。 此参数适用于命令中指定的所有文件和文件夹。 |
|  /u [@no__t > \] @ no__t-2   |                                                          使用指定用户帐户的权限执行命令。 如果不使用 **/u**选项，则默认情况下会使用系统权限。                                                           |
|       /p [\<Password >]       |                                               指定在 **/u**选项中指定的用户帐户的密码。 如果不使用 **/p**选项，则在执行命令时，将出现密码提示。                                                |
| [/fo {TABLE \| LIST \| CSV}] |                             以指定的格式显示输出。 *格式*的有效值为：</br>数据表显示表中的输出。</br>成员列表在列表中显示输出。</br>.CSV以逗号分隔值格式显示输出。                              |
|             /nh              |                                                                                取消输出中的列标题。 仅在 **/fo**参数设置为**表**或**CSV**时有效。                                                                                 |
|              /v              |                                                                                                       指定详细信息显示在输出中。                                                                                                        |
|              /?              |                                                                                                                     在命令提示符下显示帮助。                                                                                                                     |

### <a name="examples"></a>示例

若要查询和显示所有打开的文件，请键入：
```
openfiles /query
```
若要查询和显示不带标头的所有打开的文件，请键入：
```
openfiles /query /fo table /nh
```
若要以列表格式查询和显示所有打开的文件以及详细信息，请键入：
```
openfiles /query /fo list /v
```
若要通过使用 "maindom" 域上用户 "hiropln" 的凭据在远程系统 "srvmain" 上查询和显示所有打开的文件，请键入：
```
openfiles /query /s srvmain /u maindom\hiropln /p p@ssW23
```

> [!NOTE]
> 在此示例中，在命令行上提供密码。 若要防止显示密码，请忽略 **/p**选项。 系统将提示你输入密码，该密码不会回显到屏幕。

## <a name="BKMK_local"></a>openfiles/local

启用或禁用系统 "维护对象列表" 全局标志。 如果在没有参数的情况下使用，则**openfiles/local**将显示 "维护对象列表" 全局标志的当前状态。

### <a name="syntax"></a>语法

```
openfiles /local [on | off]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[\| off]|启用或禁用系统 "维护对象列表" 全局标志，该标志跟踪本地文件句柄。|
|/?|在命令提示符下显示帮助。|

### <a name="remarks"></a>备注

-   启用 "维护对象列表" 全局标志可能会使系统变慢。
-   使用 "**打开**" 或 "**关闭**" 选项所做的更改在重新启动系统后才会生效。

### <a name="examples"></a>示例

若要检查 "维护对象列表" 全局标志的当前状态，请键入：
```
openfiles /local
```
默认情况下，"维护对象列表" 全局标志处于禁用状态，并且显示以下输出：
```
INFO: The system global flag 'maintain objects list' is currently disabled.
```
若要启用 "维护对象列表" 全局标志，请键入：
```
openfiles /local on
```
启用全局标志后，将显示以下消息：
```
SUCCESS: The system global flag 'maintain objects list' is enabled.
         This will take effect after the system is restarted.
```
若要禁用 "维护对象列表" 全局标志，请键入：
```
openfiles /local off
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
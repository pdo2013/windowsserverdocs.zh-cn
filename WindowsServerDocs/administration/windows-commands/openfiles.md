---
title: openfiles
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0bec8cf64a3c7f261c792a07da603cba4366e1a7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436435"
---
# <a name="openfiles"></a>openfiles



使管理员能够查询、 显示或断开连接的系统上已打开的文件和目录。 此外可启用或禁用系统维护的对象列表全局标志。

本主题包含有关以下命令的信息：
-   [openfiles /disconnect](#BKMK_disconnect)
-   [openfiles /query](#BKMK_query)
-   [openfiles /local](#BKMK_local)

## <a name="BKMK_disconnect"></a>openfiles /disconnect

使管理员能够断开连接的共享文件夹通过远程打开的文件和文件夹。

### <a name="syntax"></a>语法

```
openfiles /disconnect [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/id <OpenFileID>] | [/a <AccessedBy>] | [/o {read | write | read/write}]} [/op <OpenFile>]
```

### <a name="parameters"></a>Parameters

|            参数             |                                                                                                                                 描述                                                                                                                                  |
|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s\<系统 >           | 指定要连接到 （按名称或 IP 地址） 的远程系统。 不要使用反斜杠。 如果不使用 **/s**选项，该命令默认情况下，在本地计算机上执行。 此参数适用于所有文件和命令中指定的文件夹。 |
|    /u [\<域 >\]<UserName>     |                                                          通过使用指定的用户帐户的权限执行该命令。 如果不使用 **/u**选项，默认情况下使用权限的系统。                                                           |
|         /p [\<Password>]         |                                               指定在指定的用户帐户的密码 **/u**选项。 如果不使用 **/p** ，提示输入密码后将显示选项执行此命令。                                                |
|        /id \<OpenFileID>         |                                       断开连接打开的文件，通过指定的文件 id。 通配符字符 ( **&#42;** ) 可用于此参数。</br>注意：可以使用**openfiles /query**命令查找文件 id。                                       |
|         /a \<AccessedBy>         |                                                断开所有打开的文件中指定的用户名相关联*AccessedBy*参数。 通配符字符 ( **&#42;** ) 可用于此参数。                                                 |
| /o {读取\|编写\|读/写} |                                               断开所有打开的文件，使用指定的打开模式值。 有效值为读取、 写入或读/写。 通配符字符 ( **&#42;** ) 可用于此参数。                                                |
|         /op \<OpenFile>          |                                                           将断开所有打开的文件连接创建的特定打开的文件名称。 通配符字符 ( **&#42;** ) 可用于此参数。                                                           |
|                /?                |                                                                                                                     在命令提示符下显示帮助。                                                                                                                     |

### <a name="examples"></a>示例

若要断开连接的文件 ID 26843578 所有打开的文件，请键入：
```
openfiles /disconnect /id 26843578
```
若要断开所有打开的文件和目录访问的用户"hiropln"，键入：
```
openfiles /disconnect /a hiropln
```
若要断开所有打开的文件和目录具有读/写模式下，键入：
```
openfiles /disconnect /o read/write
```
若要断开连接打开的文件同名的目录"C:\TestShare\"，无论谁正在访问它，类型：
```
openfiles /disconnect /a * /op "c:\testshare\"
```
若要断开所有打开的文件"srvmain"在远程计算机上的正在访问的用户"hiropln，"而不考虑其 ID，请键入：
```
openfiles /disconnect /s srvmain /u maindom\hiropln /id *
```

## <a name="BKMK_query"></a>openfiles /query

查询并显示所有打开的文件。

### <a name="syntax"></a>语法

```
openfiles /query [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] [/fo {TABLE | LIST | CSV}] [/nh] [/v]
```

### <a name="parameters"></a>Parameters

|          参数           |                                                                                                                                 描述                                                                                                                                  |
|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         /s\<系统 >         | 指定要连接到 （按名称或 IP 地址） 的远程系统。 不要使用反斜杠。 如果不使用 **/s**选项，该命令默认情况下，在本地计算机上执行。 此参数适用于所有文件和命令中指定的文件夹。 |
|  /u [\<域 >\]<UserName>   |                                                          通过使用指定的用户帐户的权限执行该命令。 如果不使用 **/u**选项，默认情况下使用权限的系统。                                                           |
|       /p [\<Password>]       |                                               指定在指定的用户帐户的密码 **/u**选项。 如果不使用 **/p** ，提示输入密码后将显示选项执行此命令。                                                |
| [/fo {TABLE \| LIST \| CSV}] |                             指定的格式显示输出。 有效值*格式*是：</br>表：显示输出表中。</br>列表：显示输出列表中。</br>CSV:以逗号分隔值格式显示输出。                              |
|             /nh              |                                                                                禁止显示在输出中的列标题。 仅当 **/fo**参数设置为**表**或**CSV**。                                                                                 |
|              /v              |                                                                                                       指定在输出中显示的详细的信息。                                                                                                        |
|              /?              |                                                                                                                     在命令提示符下显示帮助。                                                                                                                     |

### <a name="examples"></a>示例

若要查询并显示所有打开的文件，请键入：
```
openfiles /query
```
若要查询并不带标头的表格格式显示所有打开的文件，请键入：
```
openfiles /query /fo table /nh
```
若要查询并以列表格式的详细信息，显示所有打开的文件，请键入：
```
openfiles /query /fo list /v
```
若要查询并通过使用"hiropln"上"maindom"域用户的凭据显示"srvmain"在远程系统上所有打开的文件，请键入：
```
openfiles /query /s srvmain /u maindom\hiropln /p p@ssW23
```

> [!NOTE]
> 在此示例中，在命令行上提供密码。 若要防止显示密码，省略 **/p**选项。 系统将提示您输入密码，将不回显到屏幕。

## <a name="BKMK_local"></a>openfiles /local

启用或禁用系统维护的对象列表全局标志。 如果使用不带参数， **openfiles /local**显示维护对象列表全局标志的当前状态。

### <a name="syntax"></a>语法

```
openfiles /local [on | off]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[上\|关闭]|启用或禁用系统维护的对象列表全局标志，跟踪本地文件的句柄。|
|/?|在命令提示符下显示帮助。|

### <a name="remarks"></a>备注

-   启用维护对象列表全局标志可能会减慢你的系统。
-   通过使用所做的更改**上**或**关闭**选项才会生效，直到重新启动系统。

### <a name="examples"></a>示例

若要检查维护对象列表全局标志的当前状态，请键入：
```
openfiles /local
```
默认情况下，维护的对象列表全局标志处于禁用状态，并显示以下输出：
```
INFO: The system global flag 'maintain objects list' is currently disabled.
```
若要启用维护对象列表全局标志，请键入：
```
openfiles /local on
```
启用全局标志时，会显示以下消息：
```
SUCCESS: The system global flag 'maintain objects list' is enabled.
         This will take effect after the system is restarted.
```
若要禁用维护对象列表全局标志，请键入：
```
openfiles /local off
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
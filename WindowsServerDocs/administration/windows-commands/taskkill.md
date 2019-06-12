---
title: taskkill
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b71e792-08b6-46d4-95a5-cb6336a79524
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13366307bbf685d1e4af8c0a58d5b9d6643111dc
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811046"
---
# <a name="taskkill"></a>taskkill

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

结束一个或多个任务或进程。 可以通过进程 ID 或图像名称结束进程。 **taskkill**取代**kill**工具。
有关如何使用此命令的示例，请参阅[示例](#examples)。

## <a name="syntax"></a>语法

```
taskkill [/s <computer> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/fi <Filter>] [...] [/pid <ProcessID> | /im <ImageName>]} [/f] [/t]
```

## <a name="parameters"></a>Parameters

|         参数         |                                                                                                                                        描述                                                                                                                                        |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /s \<computer>       |                                                                                    指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。                                                                                     |
| /u\<域 >\\\<用户名 > | 使用指定的用户帐户权限运行命令*用户名*或*域*\\*用户名*。 **/u**可以指定仅当 **/s**指定。 默认值为当前已登录到发出命令的计算机的用户的权限。 |
|      /p \<Password>       |                                                                                                   指定在指定的用户帐户的密码 **/u**参数。                                                                                                   |
|       /fi \<Filter>       |          应用筛选器选择的一组任务。 可以使用多个筛选器，也可以使用通配符字符 ( **\\** \*) 指定的所有任务或映像名称。 请参阅以下[的有效的筛选器名称表](#filter-names-operators-and-values)，运算符和值。           |
|     /pid \<ProcessID>     |                                                                                                                 指定要终止的进程的进程 ID。                                                                                                                 |
|     /im \<ImageName>      |                                                                                指定要终止的进程映像名称。 使用通配符字符 ( **\\** \*) 指定所有的映像名称。                                                                                |
|            /f             |                                                                    指定的进程将强制终止。 对远程进程; 忽略此参数强制终止所有远程进程。                                                                     |
|            /t             |                                                                                                          终止指定的进程和由其启动的任何子进程。                                                                                                          |

#### <a name="filter-names-operators-and-values"></a>筛选器名称、 运算符和值

| 筛选器名称 |    有效的运算符     |                                                                有效值                                                                |
|-------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|   STatUS    |         eq, ne         |                                                 RUNNING &#124; NOT RESPONDING &#124; UNKNOWN                                                 |
|  IMAGENAME  |         eq, ne         |                                                                  映像名称                                                                  |
|     PID     | eq, ne, gt, lt, ge, le |                                                                  PID 值                                                                   |
|   会话   | eq, ne, gt, lt, ge, le |                                                                会话号                                                                |
|   CPUtime   | eq, ne, gt, lt, ge, le | CPU 时间的格式<em>HH</em> **:** <em>MM</em> **:** <em>SS</em>，其中*MM*并*SS*介于 0 和 59 和*HH*是任何无符号数字 |
|  MEMUSAGE   | eq, ne, gt, lt, ge, le |                                                              以 kb 为单位的内存使用情况                                                              |
|  USERNAME   |         eq, ne         |                                               任何有效的用户名 (*用户*或*域*\\*用户*)                                               |
|  服务   |         eq, ne         |                                                                 服务名称                                                                 |
| WINDOWTITLE |         eq, ne         |                                                                 窗口标题                                                                 |
|   模块   |         eq, ne         |                                                                   DLL 名称                                                                   |

## <a name="remarks"></a>备注
* 指定远程系统时，不支持的 WINDOWTITLE 和状态筛选器。
* 通配符字符 ( **\\** <em>) 对于接受 * */im</em>* 选项只有在应用筛选器时。
* 终止远程进程始终执行强制，而不管是否 **/f**指定选项。
* 提供将计算机名称传递给主机名筛选器会导致关闭并停止所有进程。
* 可以使用**tasklist**来确定进程 ID (PID) 将被终止的进程。

## <a name="examples"></a>示例

若要结束的进程与进程 Id 1230，1241 和 1253，键入：

```
taskkill /pid 1230 /pid 1241 /pid 1253
```

若要强制结束进程"Notepad.exe"，如果它由系统启动，请键入：

```
taskkill /f /fi "USERNAME eq NT AUTHORITY\SYSTEM" /im notepad.exe
```

若要使用映像名称以"请注意，"开头的"Srvmain"在远程计算机上的所有进程都结束时使用的凭据的用户帐户 Hiropln，键入：

```
taskkill /s srvmain /u maindom\hiropln /p p@ssW23 /fi "IMAGENAME eq note*" /im *
```

若要结束进程 ID 2134 和任何子进程处理，它已启动，但仅当这些进程启动的管理员帐户中，键入：

```
taskkill /pid 2134 /t /fi "username eq administrator"
```

若要结束所有进程的进程 ID 大于或等于 1000，而不管其图像名称，键入：

```
taskkill /f /fi "PID ge 1000" /im *
```

#### <a name="additional-references"></a>其他参考
[命令行语法项](command-line-syntax-key.md)

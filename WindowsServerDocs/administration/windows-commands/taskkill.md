---
title: taskkill
description: '适用于 * * * * 的 Windows 命令主题 '
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
ms.openlocfilehash: 9050788a06b66e54be59c69dd8cc837092123b00
ms.sourcegitcommit: ee8e0b217be6f6b2532ee7265fb4be00c106e124
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878130"
---
# <a name="taskkill"></a>taskkill

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

结束一个或多个任务或进程。 可以通过进程 ID 或图像名称结束进程。 **taskkill**替换**kill**工具。
有关如何使用此命令的示例，请参阅[示例](#examples)。

## <a name="syntax"></a>语法

```
taskkill [/s <computer> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/fi <Filter>] [...] [/pid <ProcessID> | /im <ImageName>]} [/f] [/t]
```

## <a name="parameters"></a>Parameters

|         参数         |                                                                                                                                        描述                                                                                                                                        |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /s \<计算机 >       |                                                                                    指定远程计算机的名称或 IP 地址（不使用反斜杠）。 默认值为本地计算机。                                                                                     |
| /u \<域 >\\用户名>\< | 使用*用户名*或*域*\\*用户名*指定的用户的帐户权限运行命令。 只有指定 **/s**时才能指定 **/u** 。 默认值是当前登录到发出命令的计算机的用户的权限。 |
|      /p \<密码 >       |                                                                                                   指定在 **/u**参数中指定的用户帐户的密码。                                                                                                   |
|       /fi \<filter >       |          应用筛选器以选择一组任务。 可以使用多个筛选器，也可以使用通配符（ **\\** \*）来指定所有任务或映像名称。 [有关有效的筛选器名称](#filter-names-operators-and-values)、运算符和值，请参阅下表。           |
|     /pid \<ProcessID >     |                                                                                                                 指定要终止的进程的进程 ID。                                                                                                                 |
|     /im \<ImageName >      |                                                                                指定要终止的进程的映像名称。 使用通配符（ **\\** \*）指定所有映像名称。                                                                                |
|            /f             |                                                                    指定强制终止进程。 对于远程进程，此参数将被忽略。所有远程进程都被强制终止。                                                                     |
|            /t             |                                                                                                          终止指定的进程以及由该进程启动的任何子进程。                                                                                                          |

#### <a name="filter-names-operators-and-values"></a>筛选器名称、运算符和值

| 筛选器名称 |    有效的运算符     |                                                                有效值                                                                |
|-------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|   状态值    |         eq, ne         |                                                 正在&#124;运行未&#124;响应未知                                                 |
|  IMAGENAME  |         eq, ne         |                                                                  映像名称                                                                  |
|     PID     | eq, ne, gt, lt, ge, le |                                                                  PID 值                                                                   |
|   会议   | eq, ne, gt, lt, ge, le |                                                                会话编号                                                                |
|   CPUtime   | eq, ne, gt, lt, ge, le | 采用<em>HH</em> **：** <em>MM</em> **：** <em>SS</em>格式的 CPU 时间，其中*MM*和*SS*介于0到59之间， *HH*是任意无符号数字 |
|  MEMUSAGE   | eq, ne, gt, lt, ge, le |                                                              内存使用量（KB）                                                              |
|  用户名   |         eq, ne         |                                               任何有效的用户名（*用户*或*域*\\*用户*）                                               |
|  服务器   |         eq, ne         |                                                                 服务名称                                                                 |
| SYSTEM.WINDOWS.CONTROLS.PAGE.WINDOWTITLE |         eq, ne         |                                                                 窗口标题                                                                 |
|   模块   |         eq, ne         |                                                                   DLL 名称                                                                   |

## <a name="remarks"></a>备注
* 指定远程系统时，不支持 SYSTEM.WINDOWS.CONTROLS.PAGE.WINDOWTITLE 和 STATUS 筛选器。
* 仅当应用了 **\\** 筛选器时，才<em>接受 * */im</em>* 选项的通配符（）。
* 不管是否指定了 **/f**选项，远程进程的终止始终都是强制执行的。
* 向主机名筛选器提供计算机名称将导致关闭并停止所有进程。
* 您可以使用**tasklist**来确定要终止的进程的进程 ID （PID）。

## <a name="examples"></a>示例

若要结束进程 Id 为1230、1241和1253的进程，请键入：

```
taskkill /pid 1230 /pid 1241 /pid 1253
```

若要强行结束系统启动的进程 "Notepad.exe"，请键入：

```
taskkill /f /fi "USERNAME eq NT AUTHORITY\SYSTEM" /im notepad.exe
```

若要在远程计算机 "Srvmain" 上以 "note" 开头的映像名称结束所有进程，请键入：

```
taskkill /s srvmain /u maindom\hiropln /p p@ssW23 /fi "IMAGENAME eq note*" /im *
```

若要结束进程 ID 为2134的进程及其启动的任何子进程，但仅当这些进程已由管理员帐户启动时，请键入：

```
taskkill /pid 2134 /t /fi "username eq administrator"
```

若要结束所有进程 ID 大于或等于1000的进程，无论其映像名称如何，请键入：

```
taskkill /f /fi "PID ge 1000" /im *
```

#### <a name="additional-references"></a>其他参考
[命令行语法项](command-line-syntax-key.md)

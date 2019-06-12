---
title: Tasklist
description: 了解如何显示在本地或远程计算机上运行的进程的列表。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8dbe30ee-1484-46be-917b-5ca3ff4fdc9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2c933bb68c488d83311856958a56809f2f5b859
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440992"
---
# <a name="tasklist"></a>Tasklist

显示本地计算机或远程计算机上当前正在运行的进程列表。 **Tasklist**取代**tlist**工具。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
tasklist [/s <Computer> [/u [<Domain>\]<UserName> [/p <Password>]]] [{/m <Module> | /svc | /v}] [/fo {table | list | csv}] [/nh] [/fi <Filter> [/fi <Filter> [ ... ]]]
```

## <a name="parameters"></a>Parameters

|          参数           |                                                                                                                                            描述                                                                                                                                             |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        /s\<计算机 >        |                                                                                         指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。                                                                                         |
| /u [\<域 >\\\]\<用户名 > | 使用指定的用户帐户权限运行命令*用户名*或*域*\*用户名<em>。\* \*/u</em> \*才可以指定 **/s**指定。 默认值为当前已登录到发出命令的计算机的用户的权限。 |
|        /p \<Password>        |                                                                                                       指定在指定的用户帐户的密码 **/u**参数。                                                                                                        |
|         /m\<模块 >         |                                                               列出与加载的 DLL 模块与给定的模式名称匹配的所有任务。 如果未指定模块名称，此选项将显示加载的每个任务的所有模块。                                                                |
|             /svc             |                                                                                    列出每个进程而无需截断的所有服务信息。 时有效 **/fo**参数设置为**表**。                                                                                    |
|              /v              |                                                                                 在输出中显示详细任务的信息。 对于完整的详细输出，而无需截断，使用 **/v**并 **/svc**在一起。                                                                                 |
|  /fo {表\|列表\|csv}  |                                                                             指定要用于输出的格式。 有效的值为**表**，**列表**，并**csv**。 默认格式为输出**表**。                                                                             |
|             /nh              |                                                                                             禁止显示在输出中的列标题。 时有效 **/fo**参数设置为**表**或**csv**。                                                                                              |
|        /fi \<Filter>         |                                                                          指定要包含在中或从查询中排除进程的类型。 请参阅下表中的有效的筛选器名称、 运算符和值。                                                                          |
|              /?              |                                                                                                                                在命令提示符下显示帮助。                                                                                                                                |

### <a name="filter-names-operators-and-values"></a>筛选器名称、 运算符和值

| 筛选器名称 |    有效的运算符     |                                                                 有效值                                                                 |
|-------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|   状态    |         eq, ne         |                                                                   运行                                                                    |
|  IMAGENAME  |         eq, ne         |                                                                  映像名称                                                                  |
|     PID     | eq, ne, gt, lt, ge, le |                                                                  PID 值                                                                   |
|   会话   | eq, ne, gt, lt, ge, le |                                                                会话号                                                                |
| 会话名 |         eq, ne         |                                                                 会话名称                                                                 |
|   CPUTIME   | eq, ne, gt, lt, ge, le | CPU 时间的格式<em>HH</em> **:** <em>MM</em> **:** <em>SS</em>，其中*MM*并*SS*介于 0 和 59 和*HH*是任何无符号数字 |
|  MEMUSAGE   | eq, ne, gt, lt, ge, le |                                                              以 kb 为单位的内存使用情况                                                              |
|  USERNAME   |         eq, ne         |                                                             任何有效的用户名                                                              |
|  服务   |         eq, ne         |                                                                 服务名称                                                                 |
| WINDOWTITLE |         eq, ne         |                                                                 窗口标题                                                                 |
|   模块   |         eq, ne         |                                                                   DLL 名称                                                                   |

## <a name="remarks"></a>备注

指定远程系统时，不支持的 WINDOWTITLE 和状态筛选器。

## <a name="BKMK_examples"></a>示例

若要列出所有任务使用进程 ID 大于 1000，并将其以 CSV 格式显示，请键入：
```
tasklist /v /fi "PID gt 1000" /fo csv
```
若要列出当前正在运行的系统进程，请键入：
```
tasklist /fi "USERNAME ne NT AUTHORITY\SYSTEM" /fi "STATUS eq running"
```
若要列出的所有进程的当前正在运行的详细的信息，请键入：
```
tasklist /v /fi "STATUS eq running"
```
若要列出所有服务的进程的信息"Srvmain"具有以"ntdll"开头的 DLL 名称在远程计算机上，请键入：
```
tasklist /s srvmain /svc /fi "MODULES eq ntdll*"
```
若要列出"Srvmain，"使用当前登录的用户帐户的凭据在远程计算机上的进程类型：
```
tasklist /s srvmain 
```
若要列出"Srvmain，"使用的用户帐户 Hiropln，凭据在远程计算机上的进程类型：
```
tasklist /s srvmain /u maindom\hiropln /p p@ssW23
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
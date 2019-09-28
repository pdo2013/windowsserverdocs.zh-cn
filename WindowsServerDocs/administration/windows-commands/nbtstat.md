---
title: nbtstat
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1d2ea99e-72f1-471f-9525-d2c49bf3be82
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd9148c922ac97e83b3b21bcb8b6585775505bf2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373321"
---
# <a name="nbtstat"></a>nbtstat

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示 TCP/IP 上的 NetBIOS （NetBT）协议统计信息、本地计算机和远程计算机的 NetBIOS 名称表以及 NetBIOS 名称缓存。 **nbtstat**允许刷新 NetBIOS 名称缓存和在 Windows Internet 名称服务（WINS）中注册的名称。 在没有参数的情况下使用， **nbtstat**显示帮助。 

## <a name="syntax"></a>语法

```
nbtstat [/a <remoteName>] [/A <IPaddress>] [/c] [/n] [/r] [/R] [/RR] [/s] [/S] [<Interval>]
```

### <a name="parameters"></a>Parameters

|    参数    |                                                                                                                         描述                                                                                                                         |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /a <remoteName> |    显示远程计算机的 NetBIOS 名称表，其中*remoteName*是远程计算机的 netbios 计算机名称。 NetBIOS 名称表是对应于计算机上运行的 NetBIOS 应用程序的 NetBIOS 名称的列表。     |
| /A <IPaddress>  |                                                           显示远程计算机的 NetBIOS 名称表，由远程计算机的 IP 地址（采用点分十进制表示法）指定。                                                            |
|       /c        |                                                                        显示 NetBIOS 名称缓存的内容、NetBIOS 名称表及其解析的 IP 地址。                                                                         |
|       /n        |                                            显示本地计算机的 NetBIOS 名称表。 "**已注册**" 状态指示该名称是通过广播或 WINS 服务器注册的。                                             |
|       /r        |      显示 NetBIOS 名称解析统计信息。 在配置为使用 WINS 的运行 Windows XP 或 Windows Server 2003 的计算机上，此参数将返回使用广播和 WINS 解析和注册的名称数。       |
|       /R        |                                                                      清除 NetBIOS 名称缓存的内容，然后从**Lmhosts**文件重新加载 #PRE 标记的项。                                                                      |
|       /RR       |                                                                           释放并刷新向 WINS 服务器注册的本地计算机的 NetBIOS 名称。                                                                            |
|       /s        |                                                                          显示 NetBIOS 客户端和服务器会话，尝试将目标 IP 地址转换为名称。                                                                           |
|       /S        |                                                                          显示 NetBIOS 客户端和服务器会话，仅按目标 IP 地址列出远程计算机。                                                                          |
|   <Interval>    | 重新显示所选统计信息，并按每个显示的*间隔*暂停指定的秒数。 按 CTRL + C 停止重新中的统计信息。 如果省略此参数，则**nbtstat**仅打印当前配置信息一次。 |
|       /?        |                                                                                                            在命令提示符下显示帮助。                                                                                                             |

## <a name="remarks"></a>备注

-   **nbtstat**命令行参数区分大小写。

-   下表描述了**nbtstat**生成的列标题：

    |标题|描述|
    |------|--------|
    |Input|接收的字节数。|
    |Output|发送的字节数。|
    |输入/输出|连接是从计算机（出站）还是从另一台计算机连接到本地计算机（入站）。|
    |生命|名称表缓存条目在清除之前将处于活动的时间。|
    |本地名称|与连接关联的本地 NetBIOS 名称。|
    |远程主机|与远程计算机关联的名称或 IP 地址。|
    |< 03 >|NetBIOS 名称的最后一个字节转换为十六进制。 每个 NetBIOS 名称长度为16个字符。 最后一个字节通常具有特殊意义，因为同一名称可能在计算机上出现多次，只是在最后一个字节内有所不同。 例如，< 20 > 为 ASCII 文本中的一个空格。|
    |type|名称的类型。 名称可以为唯一名称或组名称。|
    |状态|远程计算机上的 NetBIOS 服务是否正在运行（注册）或重复的计算机名是否已注册了相同的服务（冲突）。|
    |状态|NetBIOS 连接的状态。|

-   下表描述了可能的 NetBIOS 连接状态：

    |状态|描述|
    |-----|--------|
    |已连接|已建立会话。|
    |相应|已创建连接端点并将其与 IP 地址相关联。|
    |收听|此终结点可用于入站连接。|
    |空闲|此终结点已打开，但无法接收连接。|
    |待|会话正在连接阶段，正在解析目标的名称到 IP 地址映射。|
    |接受|当前正在接受入站会话，不久将会连接。|
    |正在|会话正在尝试重新连接（第一次尝试时无法连接）。|
    |出站|会话正在连接阶段，当前正在创建 TCP 连接。|
    |入站|入站会话在连接阶段。|
    |开|会话正在断开连接。|
    |Disconnected|本地计算机发出断开连接，它正在等待来自远程系统的确认。|

-   仅当 Internet 协议（TCP/IP）协议安装为网络连接中的网络适配器属性中的组件时，此命令才可用。

## <a name="BKMK_Examples"></a>示例
若要显示计算机的 netbios 名称为 CORP07 的远程计算机的 NetBIOS 名称表，请键入：

```
nbtstat /a CORP07
```

若要显示分配有 10.0.0.99 IP 地址的远程计算机的 NetBIOS 名称表，请键入：

```
nbtstat /A 10.0.0.99
```

若要显示本地计算机的 NetBIOS 名称表，请键入：

```
nbtstat /n
```

若要显示本地计算机的 NetBIOS 名称缓存内容，请键入：

```
nbtstat /c
```

若要清除 NetBIOS 名称缓存并重载本地 Lmhosts 文件中的 #PRE 标记项，请键入：

```
nbtstat /R
```

若要释放注册到 WINS 服务器并重新注册的 NetBIOS 名称，请键入：

```
nbtstat /RR
```

若要每隔五秒按 IP 地址显示 NetBIOS 会话统计信息，请键入：

```
nbtstat /S 5
```

## <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)



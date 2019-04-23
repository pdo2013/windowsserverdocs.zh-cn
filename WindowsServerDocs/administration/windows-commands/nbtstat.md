---
title: nbtstat
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0193152674dc934aa4f2d3be4dec54afc3066951
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867698"
---
# <a name="nbtstat"></a>nbtstat

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示 NetBIOS over TCP/IP (NetBT) 协议统计信息、 本地计算机和远程计算机的 NetBIOS 名称表和 NetBIOS 命名缓存。 **nbtstat**允许 NetBIOS 名称缓存和注册与 Windows Internet 名称服务 (WINS) 名称的刷新。 使用不带参数， **nbtstat**显示帮助。 

## <a name="syntax"></a>语法

```
nbtstat [/a <remoteName>] [/A <IPaddress>] [/c] [/n] [/r] [/R] [/RR] [/s] [/S] [<Interval>]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|/a <remoteName>|显示远程计算机的 NetBIOS 名称表其中*remoteName*是远程计算机的 NetBIOS 计算机名。 NetBIOS 名称表是对应于该计算机上运行的 NetBIOS 应用程序的 NetBIOS 名称的列表。|
|/A <IPaddress>|显示远程计算机的 IP 地址 （采用点分十进制表示法） 由指定的远程计算机的 NetBIOS 名称表。|
|/c|显示内容上的 NetBIOS 命名缓存、 NetBIOS 名称的表和其解析的 IP 地址。|
|/n|显示本地计算机的 NetBIOS 名称表。 状态**注册**指示通过广播或与 WINS 服务器注册的名称。|
|/r|显示 NetBIOS 名称解析统计信息。 在运行 Windows XP 或 Windows Server 2003 配置为使用 WINS 的计算机，此参数返回的已解决的名称数和已注册使用广播和 WINS。|
|/R|清除 NetBIOS 名称缓存的内容，然后重新加载 # 预 tagged 中的条目**Lmhosts**文件。|
|/RR|释放，然后刷新通过 WINS 服务器注册的本地计算机的 NetBIOS 名称。|
|/s|显示 NetBIOS 客户端和服务器会话，尝试将目标 IP 地址转换为一个名称。|
|/S|显示 NetBIOS 客户端和服务器会话，按目标 IP 地址列出的远程计算机。|
|<Interval>|重新显示所选的统计信息，暂停中指定的秒数*间隔*之间每个显示器。 按 CTRL + C 停止重新显示统计信息。 如果省略此参数，则**nbtstat**打印一次的当前配置信息。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   **nbtstat**命令行参数不区分大小写。

-   下表描述了生成的列标题**nbtstat**:

    |标题|描述|
    |------|--------|
    |Input|接收的字节数。|
    |输出|发送的字节数。|
    |输入/输出|该连接是否从 （出站） 的计算机或从本地计算机到另一台计算机 （入站）。|
    |生命周期|它将清除之前将 live 名称表缓存项的剩余时间。|
    |本地名称|与连接关联的本地 NetBIOS 名称。|
    |远程主机|名称或 IP 地址与远程计算机相关联。|
    |<03>|NetBIOS 名称的最后一个字节转换为十六进制。 每个 NetBIOS 名称为 16 个字符。 由于最后一个字节通常有特殊的意义，因为可能仅在最后一个字节中不同的计算机上多次出现相同的名称。 例如，< 20 个 > 是 ASCII 文本中的空间。|
    |type|名称的类型。 名称可以是唯一的名称或组名。|
    |状态|是否在远程计算机上的 NetBIOS 服务正在运行 （注册） 或重复的计算机名称已注册相同的服务 （冲突）。|
    |状态|NetBIOS 连接的状态。|

-   下表描述了可能的 NetBIOS 连接状态：

    |状态|描述|
    |-----|--------|
    |已连接|已建立会话。|
    |关联|已创建并与 IP 地址相关联的连接终结点。|
    |侦听|此终结点是可用于入站连接。|
    |空闲|此终结点已打开，但不能接收连接。|
    |连接|会话是在连接的阶段和正在被解析的目标名称到 IP 地址映射。|
    |接受|入站的会话当前正在被接受，并将在短期内连接。|
    |重新连接|一个会话尝试重新连接 （该连接上第一次尝试失败）。|
    |出站|会话是在连接的阶段和当前正在创建 TCP 连接。|
    |入站|入站的会话是在连接的阶段。|
    |正在断开连接|会话是断开连接的过程中。|
    |Disconnected|在本地计算机已断开连接，它正在等待来自远程系统的确认。|

-   此命令是作为一个组件中的网络连接的网络适配器的属性在安装了 Internet 协议 (TCP/IP) 协议的情况下才可用。

## <a name="BKMK_Examples"></a>示例
若要显示具有 CORP07 NetBIOS 计算机名的远程计算机的 NetBIOS 名称表，请键入：

```
nbtstat /a CORP07
```

若要显示的分配 IP 地址为 10.0.0.99 的远程计算机的 NetBIOS 名称表，请键入：

```
nbtstat /A 10.0.0.99
```

若要显示在本地计算机的 NetBIOS 名称表，请键入：

```
nbtstat /n
```

若要显示本地计算机 NetBIOS 名称缓存的内容，请键入：

```
nbtstat /c
```

若要清除的 NetBIOS 名称缓存并重新加载 # 预 tagged 的条目在本地的 Lmhosts 文件中，键入：

```
nbtstat /R
```

若要释放的 WINS 服务器注册的 NetBIOS 名称并重新注册它们，请键入：

```
nbtstat /RR
```

若要通过 IP 地址每隔 5 秒显示 NetBIOS 会话统计信息，请键入：

```
nbtstat /S 5
```

## <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)



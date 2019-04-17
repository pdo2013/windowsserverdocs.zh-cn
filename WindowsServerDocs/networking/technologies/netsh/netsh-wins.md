---
title: 网络 Shell (Netsh) 的示例批量文件
description: 你可以使用本主题以了解如何创建执行使用在 Windows Server 2016 Netsh 的多个任务的批量文件。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0528cfaef201ba30e00e30f56a763be39a6b828
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="network-shell-netsh-example-batch-file"></a>网络 Shell \(Netsh\) 示例批量文件

适用于：Windows Server 2016

你可以使用本主题以了解如何创建执行 Netsh 通过在 Windows Server 2016 的多个任务的批量文件。 在此示例批文件，**netsh wins**使用上下文。

## <a name="example-batch-file-overview"></a>示例批量文件概述

可以使用 Windows Internet 名称服务 \(WINS\) 批量文件中的和其他脚本 Netsh 命令自动执行任务。 下面的批量文件示例演示如何使用 Netsh WINS 的命令执行各种相关的任务。

在此示例批文件，WINS\ A WINS 服务器 192.168.125.30 的 IP 地址，并且 WINS\ B WINS 服务器 192.168.0.189 的 IP 地址。

示例批量文件来完成以下的任务。

- 添加一种具有 IP 地址 192.168.0.205 动态名称记录、MY\_RECORD \[04h\]，多 WINS\ A
- 作为推拉/复制合作伙伴的 WINS\ A 设置 WINS\ B
- 连接到 WINS\ B，然后将设置为推拉/复制伙伴 WINS\ B 的 WINS\ A
- 启动从 WINS\ A 推送复制到 WINS\ B
- 连接到 WINS\-B 以确认新的记录 MY\_RECORD，是否已成功复制

## <a name="netsh-example-batch-file"></a>Netsh 示例批量文件

在下面的示例批量文件，包含评论行为标记前面以"rem，"。 Netsh 将忽略这些注释。

    rem: Begin example batch file.
    
    rem two WINS servers:
    
    rem (WINS-A) 192.168.125.30
    
    rem (WINS-B) 192.168.0.189
    
    rem 1. Connect to (WINS-A), and add the dynamic name MY\_RECORD \[04h\] to the (WINS-A) database.
    
    netsh wins server 192.168.125.30 add name Name=MY\_RECORD EndChar=04 IP={192.168.0.205}
    
    rem 2. Connect to (WINS-A), and set (WINS-B) as a push/pull replication partner of (WINS-A).
    
    netsh wins server 192.168.125.30 add partner Server=192.168.0.189 Type=2
    
    rem 3. Connect to (WINS-B), and set (WINS-A) as a push/pull replication partner of (WINS-B).
    
    netsh wins server 192.168.0.189 add partner Server=192.168.125.30 Type=2
    
    rem 4. Connect back to (WINS-A), and initiate a push replication to (WINS-B).
    
    netsh wins server 192.168.125.30 init push Server=192.168.0.189 PropReq=0
    
    rem 5. Connect to (WINS-B), and check that the record MY\_RECORD \[04h\] was replicated successfully.
    
    netsh wins server 192.168.0.189 show name Name=MY\_RECORD EndChar=04
    
    rem 6. End example batch file.

## <a name="netsh-wins-commands-used-in-the-example-batch-file"></a>示例批量文件中使用 Netsh WINS 的命令

以下部分列表**netsh wins**在此示例过程中使用的命令。

- **服务器**。 将当前 WINS command\ 行上下文切换到其名称或 IP 地址指定的服务器。
- **添加名字**。 注册 WINS 服务器上的名称。
- **添加合作伙伴**。 将复制合作伙伴添加 WINS 服务器上。
- **初始化推送**。 启动并 WINS 服务器发送一个推送触发器。
- **显示名称**。 显示 WINS server 数据库中的某一特定记录的详细的信息。  

有关详细信息，请参阅[网络 Shell (Netsh)](netsh.md)。

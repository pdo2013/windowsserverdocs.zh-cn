---
title: Network Shell (Netsh) 示例批处理文件
description: 可以使用本主题，了解如何创建多个任务在 Windows Server 2016 中使用 Netsh 执行一个批处理文件。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0528cfaef201ba30e00e30f56a763be39a6b828
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880168"
---
# <a name="network-shell-netsh-example-batch-file"></a>网络外壳\(Netsh\)示例批处理文件

适用于：Windows Server 2016

可以使用本主题，了解如何创建 Windows Server 2016 中使用 Netsh 执行多个任务的批处理文件。 在此示例批处理文件中， **netsh wins**使用上下文。

## <a name="example-batch-file-overview"></a>示例批处理文件概述

为 Windows Internet 名称服务，可以使用 Netsh 命令\(WINS\)在批处理文件和其他脚本来自动执行任务。 下面的批处理文件示例演示如何使用 WINS 的 Netsh 命令来执行各种相关任务。

在此示例批处理文件中，WINS\-A 是 WINS 服务器具有 IP 地址 192.168.125.30 和 WINS\-B 是 WINS 服务器的 IP 地址 192.168.0.189。

示例批处理文件来完成以下任务。

- 添加动态名称记录 IP 地址为 192.168.0.205，MY\_记录\[04h\]，到 WINS\-A
- 设置 WINS\-B 作为推/拉复制伙伴的 WINS\-A
- 连接到 WINS\-B，然后设置 WINS\-WINS 的推/拉复制伙伴作为一个\-B
- 启动从 WINS 推送复制\-A 到 WINS\-B
- 连接到 WINS\-B 以验证新记录，MY\_记录，已成功复制

## <a name="netsh-example-batch-file"></a>Netsh 示例批处理文件

在下面的示例批处理文件中包含注释的行前面通过"rem"，为标记。 Netsh 将忽略注释。

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

## <a name="netsh-wins-commands-used-in-the-example-batch-file"></a>Netsh WINS 命令示例批处理文件中使用

以下部分列出了**netsh wins** ，在此示例过程使用的命令。

- **服务器**。 将当前的 WINS 命令\-到指定其名称或 IP 地址的服务器的行上下文。
- **将名称添加**。 注册上的 WINS 服务器的名称。
- **添加合作伙伴**。 WINS 服务器上添加复制伙伴。
- **init 推送**。 启动触发器，并发送推送到 WINS 服务器。
- **显示名称**。 显示详细的 WINS 服务器数据库中的特定记录的信息。  

有关详细信息，请参阅[Network Shell (Netsh)](netsh.md)。

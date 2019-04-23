---
title: 查询
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 675c5128-f3cf-4e8f-8a3f-b29ab2a8b6de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ebe10bb78a6a901871a75e8533b3389c38060666
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863148"
---
# <a name="query"></a>查询

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示有关进程、 会话和远程桌面会话主机 （RD 会话主机） 服务器的信息。

> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。

## <a name="syntax"></a>语法
```
query process
query session
query termserver
query user
```

## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[查询进程](query-process.md)|显示有关 rd 会话主机服务器运行的进程的信息。|
|[查询会话](query-session.md)|Rd 会话主机服务器上将显示有关会话的信息。|
|[查询 termserver](query-termserver.md)|显示在网络上的所有 rd 会话主机服务器上的列表。|
|[查询用户](query-user.md)|Rd 会话主机服务器上将显示有关用户会话的信息。|

#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[远程桌面服务 & #40;终端服务和 #41;命令参考](remote-desktop-services-terminal-services-command-reference.md)

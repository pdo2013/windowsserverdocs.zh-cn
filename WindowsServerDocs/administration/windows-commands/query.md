---
title: query
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d0aee400a3fae38cce73a34b55aa92f266082b19
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371836"
---
# <a name="query"></a>query

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示有关进程、会话和远程桌面会话主机（RD 会话主机）服务器的信息。

> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解最新版本中的新增功能，请参阅 Windows server TechNet 库中的[Windows server 2012 远程桌面服务中的新增功能](https://technet.microsoft.com/library/hh831527)。

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
|[查询进程](query-process.md)|显示有关在 rd 会话主机服务器上运行的进程的信息。|
|[查询会话](query-session.md)|显示有关 rd 会话主机服务器上的会话的信息。|
|[查询 termserver](query-termserver.md)|显示网络上所有 rd 会话主机服务器的列表。|
|[查询用户](query-user.md)|显示有关 rd 会话主机服务器上的用户会话的信息。|

#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[远程桌面服务&#40;终端服务和&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)

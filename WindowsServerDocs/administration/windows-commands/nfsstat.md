---
title: nfsstat
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f9db119596b5602f18acfa10af6aa1b7cbbc9b2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373178"
---
# <a name="nfsstat"></a>nfsstat



你可以使用**nfsstat**来显示或重置对 NFS 服务器的调用计数。

## <a name="syntax"></a>语法

```
nfsstat [-z]
```

## <a name="description"></a>描述

当未使用 **-z**选项时， **nfsstat**命令行实用工具将显示对服务器所做的 NFS V2、Nfs v3 和 Mount V3 调用数，因为计数器设置为0（在服务启动时或者计数器使用**重置时）nfsstat-z**。
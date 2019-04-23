---
title: nfsstat
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9db8b903d4c3681b2b3bae3424f8af83696ae2c7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853288"
---
# <a name="nfsstat"></a>nfsstat



可以使用**nfsstat**显示或重置为 NFS 服务器发出调用的计数。

## <a name="syntax"></a>语法

```
nfsstat [-z]
```

## <a name="description"></a>描述

如果不使用**z**选项， **nfsstat**命令行实用程序会显示 NFS V2、 NFS V3 和装入 V3 计数器设置为 0，因为向服务器发出的调用的数量时服务启动或重置计数器使用时**nfsstat z**。
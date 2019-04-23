---
title: diskperf
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a99f18b56c9295e902a3c89e2e89b36c9c1b6c89
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852578"
---
# <a name="diskperf"></a>diskperf



在 Windows 2000 中，默认情况下不启用物理和逻辑磁盘性能计数器。

**Diskperf**包含在 Windows XP、 Windows Server 2003、 Windows Server 2008、 Windows Vista、 Windows Server 2008 R2 和 Windows 7，以便可以用于远程启用或禁用运行的计算机上的物理或逻辑磁盘性能计数器Windows 2000。

## <a name="syntax"></a>语法

```
diskperf [-Y[D|V] | -N[D|V]] [\\computername]
```

## <a name="options"></a>选项

|Option|描述|
|------|-----------|
|-?|显示上下文相关帮助。|
|-Y|计算机重新启动时启动所有磁盘的性能计数器。|
|-YD|计算机重新启动时启用对物理驱动器的磁盘性能计数器。|
|-YV|计算机重新启动时启用逻辑驱动器或存储卷的磁盘性能计数器。|
|-N|计算机重新启动时禁用所有磁盘的性能计数器。|
|-ND|计算机重新启动时禁用对物理驱动器的磁盘性能计数器。|
|-NV|计算机重新启动时禁用逻辑驱动器或存储卷的磁盘性能的计数器。|
|\\\\*\<computername>*|指定你想要启用或禁用磁盘性能计数器的计算机的名称。|
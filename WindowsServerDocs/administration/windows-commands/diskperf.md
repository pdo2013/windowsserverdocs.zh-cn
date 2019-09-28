---
title: diskperf
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 829f0284d761e6a5134011fa1dff99646d55fc13
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377808"
---
# <a name="diskperf"></a>diskperf



在 Windows 2000 中，默认情况下不启用物理磁盘和逻辑磁盘性能计数器。

**Diskperf**包含在 windows XP、windows server 2003、windows server 2008、windows Vista、windows Server 2008 R2 和 windows 7 中，因此可用于在运行的计算机上远程启用或禁用物理或逻辑磁盘性能计数器Windows 2000。

## <a name="syntax"></a>语法

```
diskperf [-Y[D|V] | -N[D|V]] [\\computername]
```

## <a name="options"></a>选项

|Option|描述|
|------|-----------|
|-?|显示上下文相关的帮助。|
|-Y|计算机重新启动时，启动所有磁盘性能计数器。|
|-YD|计算机重新启动时，为物理驱动器启用磁盘性能计数器。|
|-YV|计算机重新启动时，为逻辑驱动器或存储卷启用磁盘性能计数器。|
|-N|在计算机重新启动时禁用所有磁盘性能计数器。|
|-ND|计算机重新启动时，禁用物理驱动器的磁盘性能计数器。|
|-NV|计算机重新启动时，禁用逻辑驱动器或存储卷的磁盘性能计数器。|
|\\ @ no__t-1 *\<computername >*|指定要在其中启用或禁用磁盘性能计数器的计算机的名称。|
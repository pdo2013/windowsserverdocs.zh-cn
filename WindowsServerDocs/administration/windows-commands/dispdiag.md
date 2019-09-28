---
title: dispdiag
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b640883a207648d2ef6c9a7d6e5366cd0bb384c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377765"
---
# <a name="dispdiag"></a>dispdiag



日志向文件显示信息。

## <a name="syntax"></a>语法

```
dispdiag [-testacpi] [-d] [-delay <Seconds>] [-out <FilePath>]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|- testacpi|运行热键诊断测试。 显示测试期间按下的任何键的键名称、代码和扫描代码。|
|-d.ddd...e|生成包含测试结果的转储文件。|
|-delay \<Seconds >|按指定时间 *（以秒为单位）* 延迟收集数据。|
|-out \<FilePath >|指定用于保存所收集数据的路径和文件名。 这必须是最后一个参数。|
|-?|显示可用的命令参数，并提供使用它们的帮助。|
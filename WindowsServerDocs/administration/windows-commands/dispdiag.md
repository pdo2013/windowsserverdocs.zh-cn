---
title: dispdiag
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9c96c70aac1b3329e050fa8b02743e61fed44d15
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831458"
---
# <a name="dispdiag"></a>dispdiag



日志到文件中显示的信息。

## <a name="syntax"></a>语法

```
dispdiag [-testacpi] [-d] [-delay <Seconds>] [-out <FilePath>]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|- testacpi|在运行热键诊断测试。 显示密钥名称，在测试期间按下任何键代码和扫描代码。|
|-d|生成测试结果的转储文件。|
|-延迟\<秒 >|延迟的数据中的指定时间的回收*秒*。|
|-out \<FilePath>|指定路径和文件名来保存收集的数据。 这必须是最后一个参数。|
|-?|显示可用的命令参数并使用这些提供帮助。|
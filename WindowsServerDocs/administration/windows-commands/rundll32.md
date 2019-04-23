---
title: rundll32
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 46d9cd64-8186-4cd4-a500-44700340fe81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b1f288d21a1dcac25ecc00f685ea179d8a6542f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835028"
---
# <a name="rundll32"></a>rundll32



加载并运行 32 位动态链接库 (Dll)。 没有为 Rundll32 可配置设置。 为特定 DLL 使用运行提供的帮助信息**rundll32**命令。

您必须运行**rundll32**命令从提升的命令提示符。 若要打开提升的命令提示符，请单击**启动**，右键单击**命令提示符**，然后单击**以管理员身份运行**。

## <a name="syntax"></a>语法

```
Rundll32 <DLLname>
```

## <a name="commands"></a>命令

|参数|描述|
|---------|-----------|
|[Rundll32 printui.dll,PrintUIEntry](rundll32-printui.md)|显示打印机用户界面|

## <a name="remarks"></a>备注

Rundll32 仅可以从显式编写 Rundll32 要调用的 DLL 调用函数。 详细了解 Rundll32 要求，请参阅[一文 164787](https://go.microsoft.com/fwlink/?LinkID=165773)在 Microsoft 知识库文章 (https://go.microsoft.com/fwlink/?LinkID=165773)。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
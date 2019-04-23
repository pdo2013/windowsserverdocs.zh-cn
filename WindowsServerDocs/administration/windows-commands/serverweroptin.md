---
title: serverweroptin
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f3c0b0af-cafb-4f09-8b36-5a357ddf392d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29545be99b14042d16a6f3a4118e0746f18b14ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869638"
---
# <a name="serverweroptin"></a>serverweroptin

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

使用此选项可启用错误报告。
## <a name="syntax"></a>语法
```
serverweroptin [/query] [/detailed] [/summary]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/query|验证当前设置。|
|/ 详细|自动发送详细的报告。|
|/summary|自动发送摘要报告。|
|/?|在命令提示符下显示帮助。|
## <a name="BKMK_Examples"></a>示例
若要验证当前设置，请键入：
```
serverweroptin /query
```
若要自动发送详细的报告，请键入：
```
serverweroptin /detailed
```
若要自动发送摘要报告，请键入
```
serverweroptin /summary
```
## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)


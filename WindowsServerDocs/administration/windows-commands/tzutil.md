---
title: tzutil
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bcf6e007-c9b6-4df5-83c5-ed7b4b1b5913
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 347254bd5a00a8bfb4a80f20d518f1e0e8b593bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392305"
---
# <a name="tzutil"></a>tzutil

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示 Windows 时区实用程序。 
## <a name="syntax"></a>语法
```
tzutil [/?] [/g] [/s <timeZoneID>[_dstoff]] [/l]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/?|在命令提示符下显示帮助。|
|/g|显示当前时区 ID。|
|/s \<timeZoneID > [_dstoff]|使用指定的时区 ID 设置当前时区。 **_Dstoff**后缀为时区禁用夏令时调整（如果适用）。|
|/l|列出所有有效的时区 Id 和显示名称。 输出将为：<br /><br />-    @ no__t-1display name ><br />-    @ no__t-1time zone ID >|

## <a name="remarks"></a>备注
退出代码为**0**指示已成功完成命令。

## <a name="BKMK_Examples"></a>示例
若要显示当前时区 ID，请键入：
```
tzutil /g
```
若要将当前时区设置为太平洋标准时间，请键入：
```
tzutil /s Pacific Standard time
```
若要将当前时区设置为太平洋标准时间并禁用夏令时调整，请键入：
```
tzutil /s Pacific Standard time_dstoff
```
## <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)


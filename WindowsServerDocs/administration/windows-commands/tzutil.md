---
title: tzutil
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 41a46ea7974b67cc557973484428480e7beb5484
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876798"
---
# <a name="tzutil"></a>tzutil

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示 Windows 时间区域实用程序。 
## <a name="syntax"></a>语法
```
tzutil [/?] [/g] [/s <timeZoneID>[_dstoff]] [/l]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/?|在命令提示符下显示帮助。|
|/g|显示当前时区 id。|
|/s \<timeZoneID>[_dstoff]|设置当前时区使用指定的时区 id。 **_Dstoff**后缀禁用时区的夏时制调整 （如果适用）。|
|/l|列出所有有效的时间区域 Id 和显示名称。 输出将是：<br /><br />-   \<显示名称 ><br />-   \<时区 ID >|

## <a name="remarks"></a>备注
退出代码**0**指示命令已成功完成。

## <a name="BKMK_Examples"></a>示例
若要显示的当前时区 ID，请键入：
```
tzutil /g
```
若要设置当前所在的时区为太平洋标准时间，请键入：
```
tzutil /s Pacific Standard time
```
若要设置当前所在的时区为太平洋标准时间，并禁用夏时制调整，请键入：
```
tzutil /s Pacific Standard time_dstoff
```
## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)


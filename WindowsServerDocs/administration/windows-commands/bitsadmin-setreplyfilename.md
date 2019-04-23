---
title: bitsadmin setreplyfilename
description: Windows 命令主题**bitsadmin setreplyfilename** -指定包含服务器回复的文件的路径。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c26d3342-0533-40b1-a13e-e09678232b25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b86d4137f661e9953d6d397b2fbc890393bbd8a0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852868"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

指定包含服务器回复的文件的路径。

**1.2 及更早版本的 BITS**: 不支持。

## <a name="syntax"></a>语法

```
bitsadmin /SetReplyFileName <Job> <Path>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|路径|服务器回复的放置位置|

## <a name="remarks"></a>备注

仅对上载-答复作业有效。

## <a name="BKMK_examples"></a>示例

下面的示例名为的作业设置答复文件名 pathfor *myDownloadJob*。
```
C:\>bitsadmin /SetReplyFileName myDownloadJob c:\reply
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
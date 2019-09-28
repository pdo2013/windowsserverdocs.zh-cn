---
title: bitsadmin setreplyfilename
description: 适用于**bitsadmin setreplyfilename**的 Windows 命令主题-指定包含服务器回复的文件的路径。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a490b5bc565549d096b6f43f42758f77570fcb26
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380425"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

指定包含服务器答复的文件的路径。

**BITS 1.2 及更早版本**： 不受支持。

## <a name="syntax"></a>语法

```
bitsadmin /SetReplyFileName <Job> <Path>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|Path|服务器答复位置|

## <a name="remarks"></a>备注

仅对上传-答复作业有效。

## <a name="BKMK_examples"></a>示例

下面的示例设置 pathfor 名为*myDownloadJob*的作业的答复文件名。
```
C:\>bitsadmin /SetReplyFileName myDownloadJob c:\reply
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
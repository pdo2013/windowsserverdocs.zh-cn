---
title: bitsadmin nowrap
description: Windows 命令主题**bitsadmin nowrap** -截断输出文本扩展的最右侧的命令窗口之外的任何行。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4130f606a6b1874e1ea31952160de44d6e09c6b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822918"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

将截断输出文本扩展的最右侧的命令窗口之外的任何行。

## <a name="syntax"></a>语法

```
bitsadmin /NoWrap
```

## <a name="remarks"></a>备注

默认情况下，所有开关除外**监视器**开关，输出包装。 指定**NoWrap**切换之前其他开关。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业状态*myDownloadJob*和输出不包装
```
C:\>bitsadmin /NoWrap /GetState myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
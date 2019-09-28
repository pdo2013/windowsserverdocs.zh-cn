---
title: bitsadmin nowrap
description: 适用于**bitsadmin nowrap**的 Windows 命令主题-截断超出命令窗口最右边边缘的任何输出文本行。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 3806ec51161eeae498e3c9b367b2aacf0bd32c99
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381051"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

截断超出命令窗口最右边边缘的任何输出文本行。

## <a name="syntax"></a>语法

```
bitsadmin /NoWrap
```

## <a name="remarks"></a>备注

默认情况下，除**监视器**开关外，所有交换机都将输出换行。 在其他交换机之前指定**NoWrap**开关。

## <a name="BKMK_examples"></a>示例

下面的示例将检索名为*myDownloadJob*的作业的状态，并且不会包装输出
```
C:\>bitsadmin /NoWrap /GetState myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
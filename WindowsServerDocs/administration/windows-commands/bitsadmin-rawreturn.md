---
title: bitsadmin rawreturn
description: 适用于**bitsadmin rawreturn**的 Windows 命令主题-返回适用于分析的数据。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 86d769de460538acda696194348980de5752d6d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380885"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

返回适合解析的数据。

## <a name="syntax"></a>语法

```
bitsadmin /RawReturn
```

## <a name="remarks"></a>备注

从输出中去除换行符和格式设置。

通常，将此命令与**Create**和**Get @ no__t*** 开关一起使用以仅接收值。 必须在其他交换机之前指定此开关。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为*myDownloadJob*的作业的状态的原始数据。
```
C:\>bitsadmin /RawReturn /GetState myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
---
title: bitsadmin rawreturn
description: Windows 命令主题**bitsadmin rawreturn** -返回适用于分析数据。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4e12c8e621021d35ac618b4592515fe38c36be0e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434889"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

返回适用于分析数据。

## <a name="syntax"></a>语法

```
bitsadmin /RawReturn
```

## <a name="remarks"></a>备注

条带换行符和从输出格式设置。

通常情况下，将此命令与结合**创建**并**获取\\** * 交换机，以接收仅值。 必须指定此开关之前其他开关。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的状态的原始数据*myDownloadJob*。
```
C:\>bitsadmin /RawReturn /GetState myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
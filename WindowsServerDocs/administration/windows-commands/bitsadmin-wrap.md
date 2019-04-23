---
title: bitsadmin wrap
description: Windows 命令主题**bitsadmin 包装**-包装输出文本扩展到下一行命令窗口的最右侧边缘之外的任何行。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4834a8a17c72394b6ee8f051ec76919af9880124
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881668"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

包装输出无法容纳在命令窗口中。

## <a name="syntax"></a>语法

```
bitsadmin /Wrap Job
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

在其他开关之前指定。 默认情况下，所有开关除外[bitsadmin 监视器](bitsadmin-monitor.md)开关，输出包装。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的信息*myDownloadJob*和包装的输出。

```
C:\>bitsadmin /Wrap /Info myDownloadJob /verbose
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

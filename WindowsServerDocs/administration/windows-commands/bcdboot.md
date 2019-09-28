---
title: bcdboot
description: Windows 命令主题**bcdboot** -快速设置系统分区，或修复位于系统分区上的启动环境。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e62a250e-08e9-47f6-89d1-e6002560ab57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1c0f505180a503617335cc9575fea3d346bbe02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383436"
---
# <a name="bcdboot"></a>bcdboot



使你能够快速设置系统分区，或修复位于系统分区上的启动环境。 系统分区是通过将一组简单的引导配置数据（BCD）文件复制到现有的空分区来设置的。

有关 BCDboot 的详细信息，包括有关在何处查找 BCDboot 的信息以及如何使用此命令的示例，请参阅[BCDboot 命令行选项](https://technet.microsoft.com/library/hh824874.aspx)主题。

## <a name="syntax"></a>语法

```
bcdboot <source> [/l] [/s]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|源程序|指定用作复制启动环境文件的源的 Windows 目录的位置。|
|/l|指定区域设置。 默认区域设置为美国英语。|
|/s|指定系统分区的卷号。 默认值为固件标识的系统分区。|

## <a name="BKMK_examples"></a>示例

有关如何使用此命令的更多示例，请参阅[BCDboot 命令行选项](https://technet.microsoft.com/library/hh824874.aspx)主题。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
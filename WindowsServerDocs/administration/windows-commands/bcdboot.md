---
title: bcdboot
description: Windows 命令主题**bcdboot** -快速设置系统分区，或修复系统分区上的启动环境。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 78838dd6567ad886948df8ac21425a8f9b596d5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825878"
---
# <a name="bcdboot"></a>bcdboot



可以快速设置一个系统分区，或修复系统分区上的启动环境。 系统分区是通过将一组简单的启动配置数据 (BCD) 文件复制到现有的空分区设置。

有关 BCDboot，其中包括有关在何处可以找到 BCDboot 和如何使用此命令的示例的详细信息请参阅[BCDboot 命令行选项](https://technet.microsoft.com/library/hh824874.aspx)主题。

## <a name="syntax"></a>语法

```
bcdboot <source> [/l] [/s]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|源|指定要用作源，可将复制启动环境的 Windows 目录的位置。|
|/l|指定的区域设置。 默认区域设置为美国英语。|
|/s|指定系统分区的卷号。 默认值是系统分区由固件。|

## <a name="BKMK_examples"></a>示例

有关如何使用此命令的更多示例，请参阅[BCDboot 命令行选项](https://technet.microsoft.com/library/hh824874.aspx)主题。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
---
title: bitsadmin 对等缓存和 setconfigurationflags
description: 用于 bitsadmin 对等互连**和 setconfigurationflags**的 Windows 命令主题：设置用于确定计算机是否可以向对等机提供内容以及能否从对等方下载内容的配置标志。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a65d54bcaa2bce26eb2b7c98250837ab09c7a423
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381115"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>bitsadmin 对等缓存和 setconfigurationflags



设置配置标志，这些标志确定计算机是否可以向对等方提供内容以及是否可以从对等方下载内容。

## <a name="syntax"></a>语法

```
bitsadmin /PeerCaching /SetConfigurationFlags <Job> <Value>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|ReplTest1|该值是一个无符号整数，它对二进制表示形式中的位进行了以下解释：</br>-允许从对等方下载作业的数据：设置最小有效位</br>-允许将作业的数据提供给对等方：从右侧设置第二个位。|

## <a name="BKMK_examples"></a>示例

下面的示例指定要从对等方为名为*myJob*的作业下载的作业数据。
```
C:\> Bitsadmin /PeerCaching /SetConfigurationFlags myJob 1
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
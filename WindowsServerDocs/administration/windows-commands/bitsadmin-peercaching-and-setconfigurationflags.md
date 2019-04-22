---
title: bitsadmin 对等缓存和 setconfigurationflags
description: Windows 命令主题**bitsadmin 对等缓存和 setconfigurationflags** -设置确定是否计算机可以提供到对等节点的内容，并可以从对等方下载内容配置标志。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 22408d4aab7f5ea374511bc16751d911a84644f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813328"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>bitsadmin 对等缓存和 setconfigurationflags



设置配置标志，以确定是否计算机可以提供到对等节点的内容和可以从对等方下载内容。

## <a name="syntax"></a>语法

```
bitsadmin /PeerCaching /SetConfigurationFlags <Job> <Value>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|ReplTest1|值为无符号的整数，位组中的二进制表示形式的以下解释：</br>-允许来自对等方下载作业的数据：设置最低有效位</br>-允许作业的数据提供给对等方：从右侧设置第 2 个字节。|

## <a name="BKMK_examples"></a>示例

下面的示例指定要下载来自对等名为的作业的作业的数据*myJob*。
```
C:\> Bitsadmin /PeerCaching /SetConfigurationFlags myJob 1
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
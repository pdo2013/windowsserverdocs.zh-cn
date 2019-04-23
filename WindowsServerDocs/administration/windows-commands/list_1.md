---
title: 列表
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 69b105a1-9710-4a06-8102-38cc9e475ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef9262a04f469f54e43cf3a83efe30fac7ad8580
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854668"
---
# <a name="list"></a>列表



显示的磁盘、 磁盘中的分区、 将磁盘中的卷的或虚拟硬盘 (Vhd) 的列表。

## <a name="syntax"></a>语法

```
list { disk | partition | volume | vdisk }
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|磁盘|显示磁盘及其相关信息，如大小、 可用空间、 磁盘是基本或动态磁盘和磁盘使用的主启动记录 (MBR) 或 GUID 分区表 (GPT) 分区形式的列表。|
|分区|显示当前磁盘的分区表中列出的分区。|
|音量|显示所有磁盘上的基本卷和动态卷的列表。|
|虚拟磁盘|显示已附加和/或所选的 Vhd 的列表。 此命令将列出已分离的 Vhd，如果当前所选;但是，磁盘类型设置为未知之前附加 VHD。 VHD 标有星号 （*） 具有焦点。</br>注意：此命令才适用于 Windows 7 和 Windows Server 2008 R2。|

## <a name="remarks"></a>备注

-   列出时动态磁盘上的分区，分区可能与磁盘上的动态卷不对应。 动态磁盘包含系统卷或启动卷 （如果存在磁盘上） 的分区表中的项会发生这种差异。 它们还包含为了保留动态卷使用的空间占用的磁盘的其余部分的分区。
-   标有星号 （*） 的对象具有焦点。
-   列出磁盘，如果磁盘丢失，作为使用 M 前缀其磁盘编号。例如，编号的第一个缺少磁盘 M0。

## <a name="BKMK_examples"></a>示例

```
list disk
list partition
list volume
list vdisk
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)


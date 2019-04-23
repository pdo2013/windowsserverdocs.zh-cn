---
title: pushd
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 396bc545-0f41-473e-b0ac-76fbbb74d390
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 548f39921c1f6aa3837e6443e396922396eb84f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887458"
---
# <a name="pushd"></a>pushd



将存储在当前目录以供**popd**命令，然后更改到指定的目录。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
pushd [<Path>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Path>|指定要将当前目录的目录。 此命令支持相对路径。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   每次使用**pushd**命令，供你使用存储在单个目录。 但是，通过使用可存储多个目录**pushd**命令多次。

    目录按顺序存储在一个虚拟堆栈中。 如果您使用**pushd**命令后，在其中使用该命令的目录放置在堆栈的底部。 如果再次使用该命令，第二个目录位于顶部的第一个。 每次使用该过程将重复**pushd**命令。

    可以使用**popd**命令将当前目录更改为最新的存储的目录**pushd**命令。 如果您使用**popd**命令时，从堆栈中移除位于堆栈顶部的目录，并在当前目录更改为该目录。 如果您使用**popd**同样，命令在堆栈上的下一步目录中删除。
-   如果启用了命令扩展， **pushd**命令接受网络路径或本地驱动器号和路径。
-   如果指定网络路径， **pushd**命令暂时将分配的最大的未使用的驱动器号 （以 z:）为指定的网络资源。 该命令然后更改当前驱动器和目录，为新分配的驱动器上的指定目录。 如果您使用**popd**命令，在启用了命令扩展**popd**命令将删除使用创建的驱动器号分配**pushd**。

## <a name="BKMK_examples"></a>示例

下面的示例演示如何使用**pushd**命令和**popd**命令批处理程序若要从批处理程序运行的然后再改回来更改当前目录中：
```
@echo off
rem This batch file deletes all .txt files in a specified directory
pushd %1
del *.txt
popd
cls
echo All text files deleted in the %1 directory
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

[Popd](popd.md)
---
title: popd
description: 了解如何将目录更改为 pushd 命令最近存储的目录。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a4c52d5-9fd1-4eac-9c0c-5767b25728ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 8a9e0a301a5f8b46e1907a4f43c5ed9247b85f77
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372227"
---
# <a name="popd"></a>popd

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

将当前目录更改为**pushd**命令最近存储的目录。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法
```
popd
```

### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   每次使用**pushd**命令时，将存储一个目录供你使用。 但是，可以多次使用**pushd**命令来存储多个目录。
    目录按顺序存储在虚拟堆栈中。 如果使用**pushd**命令一次，则使用命令的目录将置于堆栈的底部。 如果再次使用该命令，第二个目录将置于第一个目录的顶部。 每次使用**pushd**命令时都会重复此过程。
    可以使用**popd**命令将当前目录更改为**pushd**命令最近存储的目录。 如果使用**popd**命令，堆栈顶部的目录将从堆栈中删除，当前目录将更改为该目录。 如果再次使用**popd**命令，将删除堆栈上的下一个目录。
-   启用命令扩展后， **popd**命令将删除**pushd**创建的任何驱动器号 assignations。

## <a name="BKMK_examples"></a>示例
下面的示例演示如何在批处理程序中使用**pushd**命令和**popd**命令，以更改运行批处理程序的目录，然后将其更改回：

```
@echo off
rem This batch file deletes all .txt files in a specified directory
pushd %1
del *.txt
popd
cls
echo All text files deleted in the %1 directory
```

## <a name="additional-references"></a>其他参考
-   [pushd](pushd.md)
-   [命令行语法项](command-line-syntax-key.md)


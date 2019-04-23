---
title: popd
description: 了解如何将目录更改为最新存储 pushd 命令的目录。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6da6dc9d1fc2d8965f8a081831cb1150375209a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827458"
---
# <a name="popd"></a>popd

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将当前目录更改到最新存储的目录**pushd**命令。
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
-   每次使用**pushd**命令，供你使用存储在单个目录。 但是，通过使用可存储多个目录**pushd**命令多次。
    目录按顺序存储在一个虚拟堆栈中。 如果您使用**pushd**命令后，在其中使用该命令的目录放置在堆栈的底部。 如果再次使用该命令，第二个目录位于顶部的第一个。 每次使用该过程将重复**pushd**命令。
    可以使用**popd**命令将当前目录更改为最新的存储的目录**pushd**命令。 如果您使用**popd**命令时，从堆栈中移除位于堆栈顶部的目录，并在当前目录更改为该目录。 如果您使用**popd**同样，命令在堆栈上的下一步目录中删除。
-   如果启用了命令扩展， **popd**命令删除创建的任何驱动器号分配**pushd**。

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

## <a name="additional-references"></a>其他参考
-   [pushd](pushd.md)
-   [命令行语法解答](command-line-syntax-key.md)


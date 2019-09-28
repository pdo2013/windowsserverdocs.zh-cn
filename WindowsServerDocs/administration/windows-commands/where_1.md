---
title: （其中
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0b3486a5-896b-4d92-84b8-e463a0b76487
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: abebe5799075653d2ace1af4eadbdd5d477d97a6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362159"
---
# <a name="where"></a>（其中



显示与给定的搜索模式匹配的文件的位置。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
where [/r <Dir>] [/q] [/f] [/t] [$<ENV>:|<Path>:]<Pattern>[ ...] 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/r \<Dir >|指示从指定目录开始的递归搜索。|
|/q|返回退出代码（**0**表示成功， **1**表示失败），而不显示匹配文件的列表。|
|/f|用引号显示**where**命令的结果。|
|/t|显示文件大小以及每个匹配文件的上次修改日期和时间。|
|[$ \<ENV >： \| @ no__t-2Path >：] \<Pattern > [...]|指定要匹配的文件的搜索模式。 至少需要一个模式，并且模式可以包含通配符（ **&#42;** 和 **？** ）。 默认**情况下，搜索**当前目录和 PATH 环境变量中指定的路径。 您可以使用 "$*ENV*：*pattern* " 格式指定要搜索的其他路径（其中，" *env* " 是包含一个或多个路径的现有环境变量）或使用 "格式*路径*：*模式*" （其中*path*是要搜索的目录路径。 这些可选格式不应与 **/r**命令行选项一起使用。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   如果不指定文件扩展名，则默认情况下，PATHEXT 环境变量中列出的扩展名将追加到该模式。
-   **其中**，可以运行递归搜索，显示文件信息（如日期或大小），并接受环境变量来代替本地计算机上的路径。

## <a name="BKMK_examples"></a>示例

若要在当前计算机及其子目录的驱动器 C 中查找名为 Test 的所有文件，请键入：
```
where /r c:\ test 
```
若要列出公共目录中的所有文件，请键入：
```
where $public:*.*
```
若要在远程计算机、Computer1 及其子目录的驱动器 C 中查找名为 Notepad 的所有文件，请键入：
```
where /r \\computer1\c notepad.*
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
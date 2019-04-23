---
title: （其中
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ff50405dd53ee383abc8e13f67befecf73e37c1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832448"
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
|/r \<Dir>|指示使用指定的目录开始递归搜索。|
|/q|返回退出代码 (**0**成功后，对于**1**失败) 而不会显示匹配的文件的列表。|
|/f|显示的结果**其中**命令在引号中。|
|/t|显示文件大小和上次修改的日期和时间的每个匹配的文件。|
|[$\<ENV >:\|\<路径 >:]\<模式 > [...]|指定要匹配的文件的搜索模式。 至少一种模式是必需的并且该模式可以包括通配符 (**&#42;** 并 **？**)。 默认情况下**其中**会搜索当前目录和路径环境变量中指定的路径。 可以指定不同的路径以使用 $ 格式搜索*ENV*:*模式*(其中*ENV*是包含一个或多个路径的现有环境变量) 或通过使用格式*路径*:*模式*(其中*路径*是你想要搜索的目录路径)。 这些可选的格式不应该用于 **/r**命令行选项。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   如果不指定文件扩展名，默认情况下 PATHEXT 环境变量中列出的扩展追加到模式中。
-   **其中**可以运行递归搜索，显示文件的信息，如日期或大小，并接受而不是在本地计算机上的路径的环境变量。

## <a name="BKMK_examples"></a>示例

若要查找当前的计算机和及其子目录的驱动器 C 中名为 Test 的所有文件，请键入：
```
where /r c:\ test 
```
若要列出公共目录中的所有文件，请键入：
```
where $public:*.*
```
若要查找名为驱动器 C 的远程计算机，Computer1 和及其子目录中的记事本的所有文件，请键入：
```
where /r \\computer1\c notepad.*
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
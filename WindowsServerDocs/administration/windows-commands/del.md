---
title: del
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 346eede2-2085-44f5-9936-6877b5d5a833
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e569443a56646862c7a2c9fbd2c599cede941a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378700"
---
# <a name="del"></a>del



删除一个或多个文件。 此命令与**erase**命令相同。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
del [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
erase [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Names >|指定一个或多个文件或目录的列表。 通配符可用于删除多个文件。 如果指定了目录，则会删除该目录中的所有文件。|
|/p|删除指定文件之前提示确认。|
|/f|强制删除只读文件。|
|/s|删除当前目录和所有子目录中的指定文件。 显示要删除的文件的名称。|
|/q|指定安静模式。 不会提示您确认删除。|
|/a [：] \<Attributes >|基于以下文件属性删除文件：</br>**r**只读文件</br>**h**隐藏文件</br>**我**不是内容索引文件</br>**s**系统文件</br>准备好存档**的文件**</br>**l**重新分析点</br>-前缀表示 "not"|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

> [!CAUTION]
> 如果使用**del**从磁盘中删除文件，则无法检索该文件。
> -   如果使用 **/p**， **del**将显示文件的名称并发送以下消息：

    `FileName, Delete (Y/N)?`

    To confirm the deletion, press Y. To cancel the deletion and display the next file name (that is, if you specified a group of files), press N. To stop the **del** command, press CTRL+C.
- 如果禁用命令扩展， **/s**将显示找不到的任何文件的名称，而不是显示要删除的文件的名称（即，该行为是相反的）。
- 如果在 "*名称*" 中指定一个文件夹，则会删除该文件夹中的所有文件。 例如，以下命令将删除 \Work 文件夹中的所有文件：  
  ```
  del \work
  ```  
- 您可以使用通配符（ **&#42;** 和 **？** ）一次删除多个文件。 但是，若要避免无意中删除文件，应谨慎使用**del**命令。 例如，如果键入以下命令：  
  ```
  del *.*
  ```  
  **Del**命令显示以下提示：

  `Are you sure (Y/N)?`

  若要删除当前目录中的所有文件，请按 Y，然后按 ENTER。 若要取消删除，请按 N，然后按 ENTER。

> [!NOTE]
> 在将通配符用于**del**命令之前，请使用与**dir**命令相同的通配符来列出所有要删除的文件。
> -   可从恢复控制台获取带有不同参数的**del**命令。

## <a name="BKMK_examples"></a>示例

若要删除驱动器 C 上名为 Test 的文件夹中的所有文件，请键入下列内容之一：
```
del c:\test
del c:\test\*.*
```
若要从当前目录中删除文件扩展名为 .bat 的所有文件，请键入：
```
del *.bat
```
若要删除当前目录中的所有只读文件，请键入：
```
del /a:r *.*
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

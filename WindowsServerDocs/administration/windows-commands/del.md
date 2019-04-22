---
title: del
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a1c2756e53d9f047160ddd037b3868e47d6e3181
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822988"
---
# <a name="del"></a>del



删除一个或多个文件。 此命令等同于**擦除**命令。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
del [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
erase [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<名称 >|指定一个或多个文件或目录的列表。 若要删除多个文件，可以使用通配符。 如果指定了目录，将删除该目录中的所有文件。|
|/p|删除指定的文件前显示确认提示。|
|/f|强制删除只读文件。|
|/s|删除指定的当前目录及所有子目录中的文件。 删除它们时显示的文件的名称。|
|/q|指定安静模式。 系统不会提示确认删除。|
|/a[:]\<Attributes>|删除基于以下文件属性的文件：</br>**r**只读文件</br>**h**隐藏文件</br>**我**不内容索引的文件</br>**s**系统文件</br>文件可以用于存档</br>**l**重新分析点</br>-前缀 not 这意味着|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

> [!CAUTION]
> 如果您使用**del**若要从磁盘中删除的文件，不能检索它。
-   如果您使用 **/p**， **del**显示文件的名称，并将发送以下消息：

    `FileName, Delete (Y/N)?`

    若要确认删除，请按 Y。若要取消删除和显示的下一步的文件的名称 （即，如果指定的一组文件），按 n。若要停止**del**命令，请按 CTRL + C。
-   如果禁用命令扩展 **/s**显示而不是显示要删除的文件的名称未找到任何文件的名称 （即，反转行为）。
-   如果指定的文件夹中*名称*，会删除所有文件夹中的文件。 例如，以下命令将删除所有 \Work 文件夹中的文件：  
    ```
    del \work
    ```  
-   你可以使用通配符 (**&#42;** 并 **？**) 若要一次删除多个文件。 但是，若要避免无意中删除文件，应使用谨慎使用通配符**del**命令。 例如，如果键入以下命令：  
    ```
    del *.*
    ```  
    **Del**命令将显示以下提示：

    `Are you sure (Y/N)?`

    若要删除所有当前目录中的文件，请按 Y，然后按 ENTER。 若要取消删除操作，按 N 键然后按 enter 键。

> [!NOTE]
> 使用带有通配符字符之前**del**命令，使用具有相同的通配符字符**dir**命令以列出所有文件将被删除。
-   **Del**命令，使用不同的参数，可从恢复控制台。

## <a name="BKMK_examples"></a>示例

若要删除驱动器 C 上名为 Test 的文件夹中的所有文件，请键入以下任一：
```
del c:\test
del c:\test\*.*
```
若要从当前目录中删除具有.bat 文件扩展名的所有文件，请键入：
```
del *.bak
```
若要删除当前目录中的所有只读文件，请键入：
```
del /a:r *.*
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
---
title: label
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bbae8bdd-97d4-4566-9118-7c95aa07645f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0c68fbbf3ea776bbf6cd49fc4fa446d5dd46542
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437908"
---
# <a name="label"></a>label



创建、 更改或删除磁盘的卷标 （名称）。 如果使用不带参数，**标签**命令更改当前的卷标签或删除现有标签。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
label [/mp] [<Volume>] [<Label>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/mp|指定卷应视为装入点或卷名称。|
|\<卷 >|指定 （后跟一个冒号） 的驱动器号，装入点或卷名称。 如果指定卷名称，则 **/mp**则不需要参数。|
|\<Label>|指定卷的标签。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

- Windows 显示的卷标和序列号 （如果有的话） 作为目录列表的一部分。
- NTFS 卷标签可以最多 32 个字符的长度，包括空格。 NTFS 卷标签将保留并显示已创建标签时使用这种情况。
- 如果未指定的值**标签**参数，**标签**命令采用以下格式显示输出：  
  ```
  Volume in drive C: xxxxxxxxxxx 
  Volume Serial Number is xxxx-xxxx 
  Volume label (32 characters, ENTER for none)?
  ```  
  可以键入新的卷标签或按 ENTER 以保留当前标签。 如果按 enter 键，并且卷上当前有一个标签**标签**命令将提示你并显示以下消息：  
  ```
  Delete current volume label (Y/N)?
  ```  
  按 Y 以删除标签，或按 N 若要保留标签。

## <a name="BKMK_examples"></a>示例

若要标记包含年 7 月的销售信息的驱动器 A 中的磁盘，请键入：
```
label a:sales-july
```
若要删除的驱动器 C 的当前标签，请按照下列步骤：
1. 在命令提示符处，键入：  
   ```
   Label
   ```  
   应显示类似于以下输出：  
   ```
   Volume in drive C: is Main Disk
   Volume Serial Number is 6789-ABCD
   Volume label (32 characters, ENTER for none)?
   ```  
2. 按 Enter。 应显示以下提示：  
   ```
   Delete current volume label (Y/N)?
   ```  
3. 按 Y 以删除当前标签。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
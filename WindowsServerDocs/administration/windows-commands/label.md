---
title: label
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e66a2d9a7d28462b287084e3f8b129ffc03800bd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374789"
---
# <a name="label"></a>label



创建、更改或删除磁盘的卷标（即名称）。 如果在没有参数的情况下使用，则 "**标签**" 命令将更改当前卷标签或删除现有标签。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
label [/mp] [<Volume>] [<Label>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/mp|指定应将卷视为装入点或卷名。|
|\<Volume >|指定驱动器号（后跟冒号）、装入点或卷名。 如果指定了卷名称，则不需要 **/mp**参数。|
|\<Label >|指定卷的标签。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

- Windows 在目录列表中显示卷标和序列号（如果有的话）。
- NTFS 卷标长度最多可包含32个字符，包括空格。 NTFS 卷标签保留并显示创建标签时使用的大小写。
- 如果未指定**标签**参数的值，"**标签**" 命令将按以下格式显示输出：  
  ```
  Volume in drive C: xxxxxxxxxxx 
  Volume Serial Number is xxxx-xxxx 
  Volume label (32 characters, ENTER for none)?
  ```  
  您可以键入新的卷标签，或按 ENTER 来保留当前标签。 如果按 ENTER 并且卷当前具有标签，则**标签**命令会提示你输入以下消息：  
  ```
  Delete current volume label (Y/N)?
  ```  
  按 Y 删除标签，或按 N 以保留标签。

## <a name="BKMK_examples"></a>示例

若要在驱动器 A 中为包含7月销售信息的磁盘添加标签，请键入：
```
label a:sales-july
```
若要删除驱动器 C 的当前标签，请执行以下步骤：
1. 在命令提示符处，键入：  
   ```
   Label
   ```  
   应显示类似于以下内容的输出：  
   ```
   Volume in drive C: is Main Disk
   Volume Serial Number is 6789-ABCD
   Volume label (32 characters, ENTER for none)?
   ```  
2. 按 Enter。 应显示以下提示：  
   ```
   Delete current volume label (Y/N)?
   ```  
3. 按 Y 删除当前标签。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
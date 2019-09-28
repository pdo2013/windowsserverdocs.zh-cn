---
title: 注册导入
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e1c7920a64469717c30cfcddda7b8002db5ba10
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384730"
---
# <a name="reg-import"></a>注册导入



将包含导出的注册表子项、项和值的文件的内容复制到本地计算机的注册表中。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
Reg import FileName
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<文件名 >|指定包含要复制到本地计算机的注册表中的内容的文件的名称和路径。 必须使用**reg export**提前创建此文件。|
|/?|在命令提示符下显示**reg import**的帮助。|

## <a name="remarks"></a>备注

下表列出了**reg import**操作的返回值。

|ReplTest1|Description|
|-----|-----------|
|0|成功|
|1|失败|

## <a name="BKMK_examples"></a>示例

若要从名为 AppBkUp 的文件中导入注册表项，请键入：
```
reg import AppBkUp.reg
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
---
title: bitsadmin getowner
description: 适用于**bitsadmin getowner**的 Windows 命令主题-检索指定作业的所有者。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab8151ba8b1379c101aa037504ae2021ff0df62f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381447"
---
# <a name="bitsadmin-getowner"></a>bitsadmin getowner

显示指定作业的所有者的显示名称或 GUID。

## <a name="syntax"></a>语法

```
bitsadmin /GetOwner <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例显示名为的作业的所有者 *myDownloadJob*。
```
C:\>bitsadmin /GetOwner myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
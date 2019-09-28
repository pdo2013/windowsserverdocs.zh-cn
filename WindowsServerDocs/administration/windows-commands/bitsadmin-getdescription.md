---
title: bitsadmin getdescription
description: 适用于**bitsadmin getdescription**的 Windows 命令主题-检索指定作业的说明。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f3974603-ebbe-4d31-8217-040fe2d90c85
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 02ab91ad9b6d1d6d1ef67465bb5c982fbddc1bb4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381655"
---
# <a name="bitsadmin-getdescription"></a>bitsadmin getdescription



检索指定作业的说明。

## <a name="syntax"></a>语法

```
bitsadmin /GetDescription <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例将检索名为*myDownloadJob*的作业的说明。
```
C:\>bitsadmin /GetDescription myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
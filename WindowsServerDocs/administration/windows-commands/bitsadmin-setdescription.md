---
title: bitsadmin setdescription
description: 适用于**bitsadmin setdescription**的 Windows 命令主题-设置指定作业的说明。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d140ee9d575828a1a4d536073e468c9b4e56799f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380926"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription



设置指定作业的说明。

## <a name="syntax"></a>语法

```
bitsadmin /SetDescription <Job> <Description>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|描述|用于描述作业的文本。|

## <a name="BKMK_examples"></a>示例

下面的示例将检索名为*myDownloadJob*的作业的说明。
```
C:\>bitsadmin /SetDescription myDownloadJob "Music Downloads"
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
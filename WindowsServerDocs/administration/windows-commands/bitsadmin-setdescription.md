---
title: bitsadmin setdescription
description: Windows 命令主题**bitsadmin setdescription** -设置指定的作业的说明。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8e3323c20eebc8ba633ccfd478daa0753e506f46
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830748"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription



设置指定的作业的说明。

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

下面的示例检索名为的作业的说明*myDownloadJob*。
```
C:\>bitsadmin /SetDescription myDownloadJob "Music Downloads"
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
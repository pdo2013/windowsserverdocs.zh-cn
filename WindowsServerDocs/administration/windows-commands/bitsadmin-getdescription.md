---
title: bitsadmin getdescription
description: Windows 命令主题**bitsadmin getdescription** -检索指定的作业的说明。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ee20dd808cdbc8b76f44b7b14c9fd65b313a74e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813128"
---
# <a name="bitsadmin-getdescription"></a>bitsadmin getdescription



检索指定的作业的说明。

## <a name="syntax"></a>语法

```
bitsadmin /GetDescription <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的说明*myDownloadJob*。
```
C:\>bitsadmin /GetDescription myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
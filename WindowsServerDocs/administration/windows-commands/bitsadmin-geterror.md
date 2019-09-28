---
title: bitsadmin geterror
description: 适用于**bitsadmin geterror**的 Windows 命令主题-检索指定作业的详细错误信息。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cbe5bca1-d2dd-4ce6-903f-f85de4a2ec6a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f9bd607886d00ede4e1da91ed73eff2794db6ce
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381641"
---
# <a name="bitsadmin-geterror"></a>bitsadmin geterror



检索错误的详细信息将指定的作业。

## <a name="syntax"></a>语法

```
bitsadmin /GetError <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的错误信息 *myDownloadJob*。
```
C:\>bitsadmin /GetError myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
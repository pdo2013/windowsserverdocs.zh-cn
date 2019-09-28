---
title: bitsadmin info
description: Windows 命令主题**显示有关指定作业的摘要信息。** -bitsadmin 信息
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b6710d73860315fcd13670669871cd310ffb41c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381077"
---
# <a name="bitsadmin-info"></a>bitsadmin info



显示有关指定作业的摘要信息。

## <a name="syntax"></a>语法

```
bitsadmin /Info <Job> [/verbose]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

使用/verbose 参数提供有关作业的详细信息。

## <a name="BKMK_examples"></a>示例

以下示例检索有关名为*myDownloadJob*的作业的信息。
```
C:\>bitsadmin /Info myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
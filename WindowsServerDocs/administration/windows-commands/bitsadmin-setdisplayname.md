---
title: bitsadmin setdisplayname
description: 适用于**bitsadmin setdisplayname**的 Windows 命令主题-设置指定作业的显示名称。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 10a5607eb26f8199ec415a4cec17d03015a26bcd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380630"
---
# <a name="bitsadmin-setdisplayname"></a>bitsadmin setdisplayname



设置指定作业的显示名称。

## <a name="syntax"></a>语法

```
bitsadmin /SetDisplayName <Job> <DisplayName>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|DisplayName|用于指定作业的显示名称的文本。|

## <a name="BKMK_examples"></a>示例

下面的示例将名为*myDownloadJob*的作业的显示名称设置为*myDownloadJob2*。
```
C:\>bitsadmin /SetDisplayName myDownloadJob "Download Music Job"
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
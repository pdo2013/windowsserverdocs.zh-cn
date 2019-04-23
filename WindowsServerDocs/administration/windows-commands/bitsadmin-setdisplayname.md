---
title: bitsadmin setdisplayname
description: Windows 命令主题**bitsadmin setdisplayname** -设置指定的作业的显示名称。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d50cd2785e42b554cee340abc97fe4e4b53adcfc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843668"
---
# <a name="bitsadmin-setdisplayname"></a>bitsadmin setdisplayname



设置指定的作业的显示名称。

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

下面的示例设置名为的作业的显示名称*myDownloadJob*到*myDownloadJob2*。
```
C:\>bitsadmin /SetDisplayName myDownloadJob "Download Music Job"
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
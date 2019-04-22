---
title: bitsadmin getcustomheaders
description: Windows 命令主题**bitsadmin getcustomheaders** -从作业检索自定义 HTTP 标头。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f5959541f0e3190e26bbb298a9cd7c63ab32cae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812088"
---
# <a name="bitsadmin-getcustomheaders"></a>bitsadmin getcustomheaders



从作业检索自定义 HTTP 标头。

## <a name="syntax"></a>语法

```
bitsadmin /GetCustomHeaders <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例获取名为的作业的自定义标头*myDownloadJob*。
```
C:\>bitsadmin /GetCustomHeaders myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
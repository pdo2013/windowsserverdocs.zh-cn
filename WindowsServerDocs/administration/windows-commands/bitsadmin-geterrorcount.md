---
title: bitsadmin geterrorcount
description: Windows 命令主题**bitsadmin geterrorcount** -检索指定的作业生成的暂时性错误的次数的计数。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91045372931efec0e3189132a275eeacab584de4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818368"
---
# <a name="bitsadmin-geterrorcount"></a>bitsadmin geterrorcount



检索指定的作业生成的暂时性错误的次数计数。

## <a name="syntax"></a>语法

```
bitsadmin /GetErrorCount <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的错误计数信息 *myDownloadJob*。
```
C:\>bitsadmin /GetErrorCount myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
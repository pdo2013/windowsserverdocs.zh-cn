---
title: bitsadmin complete
description: Bitsadmin 的 Windows 命令主题**完成**-完成该作业。 使用此开关时，将下载的文件才可供您。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5a1dc5dbbf2d5b3207b5423f338e0caf4412599
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381825"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

完成作业。 使用此开关时，将下载的文件才可供您。 作业转到已转移的状态后，请使用此开关。 否则，只有那些已成功传输的文件是可用的。

## <a name="syntax"></a>语法

```
bitsadmin /complete <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

传输作业的状态，则 BITS 已成功传输作业中的所有文件。 但是，这些文件之前将不可用您使用 **/ 完成** 切换。 如果多个作业使用 *myDownloadJob* 作为其名称，则必须替换 *myDownloadJob* 来唯一地标识该作业的作业的 guid。
```
C:\>bitsadmin /complete myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
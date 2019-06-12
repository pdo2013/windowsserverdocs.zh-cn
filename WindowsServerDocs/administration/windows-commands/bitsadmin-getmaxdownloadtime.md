---
title: bitsadmin getmaxdownloadtime
description: Windows 命令主题**bitsadmin getmaxdownloadtime** -以秒为单位检索下载超时。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cdce64f6-7125-489d-be3c-4af1dfc8c46a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d067d6a0821d9af4784c02c6a332e8eddd2352c0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434942"
---
# <a name="bitsadmin-getmaxdownloadtime"></a>bitsadmin getmaxdownloadtime

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

以秒为单位检索下载超时。

## <a name="syntax"></a>语法

```
bitsadmin /GetMaxDownloadtime <Job> 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

-   N\/A

## <a name="BKMK_examples"></a>示例
下面的示例获取名为的作业的最大下载时间 *myDownloadJob* 以秒为单位。

```
C:\>bitsadmin /GetMaxDownloadtime myDownloadJob
```

## <a name="additional-references"></a>其他参考
[命令行语法项](command-line-syntax-key.md)



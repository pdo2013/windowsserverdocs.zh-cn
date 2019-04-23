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
ms.openlocfilehash: 868f7ac58a69c067681bf0597524fbaa5a25984f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842418"
---
#<a name="bitsadmin-getmaxdownloadtime"></a>bitsadmin getmaxdownloadtime

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
[命令行语法解答](command-line-syntax-key.md)



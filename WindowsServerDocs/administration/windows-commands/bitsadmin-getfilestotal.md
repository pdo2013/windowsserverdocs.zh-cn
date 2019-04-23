---
title: bitsadmin getfilestotal
description: Windows 命令主题**bitsadmin getfilestotal** -检索文件中指定的作业数。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21240cfa1f0fa1f8e8fc3d2acf83f5df80812816
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857458"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal



检索指定的作业中的文件数。

## <a name="syntax"></a>语法

```
bitsadmin /GetFilesTotal <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索名为作业中包含的文件数 *myDownloadJob*。
```
C:\>bitsadmin /GetFilesTotal myDownloadJob
```

##

[命令行语法解答](command-line-syntax-key.md)另请参阅
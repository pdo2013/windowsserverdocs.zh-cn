---
title: bitsadmin addfile
description: 适用于**bitsadmin addfile**的 Windows 命令主题-将文件添加到指定的作业。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8dfddda92e506dbfca2a47394a310edf16fe78aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382040"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

将文件添加到指定的作业。

## <a name="syntax"></a>语法

```
bitsadmin /AddFile <Job> <RemoteURL> <LocalName>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|RemoteURL|服务器上的文件的 URL。|
|LocalName|本地计算机上的文件的名称。 *LocalName*必须包含文件的绝对路径。|

## <a name="BKMK_examples"></a>示例

将文件添加到作业。 对要添加的每个文件重复此调用。 如果多个作业使用 *myDownloadJob* 作为其名称，则必须替换 *myDownloadJob* 来唯一地标识该作业的作业的 guid。
```
C:\>bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
---
title: bitsadmin getpeercachingflags
description: Windows 命令主题**bitsadmin getpeercachingflags** -检索标志确定是否缓存该作业的文件并将它们提供给对等的如果位可以从对等方下载作业的内容。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eabcc5cacdcc5f7f4de7178b5afeff2acc89d7a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862808"
---
#<a name="bitsadmin-getpeercachingflags"></a>bitsadmin getpeercachingflags

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

检索标志确定是否缓存该作业的文件并将它们提供给对等的如果位可以从对等方下载作业的内容。

## <a name="syntax"></a>语法

```
bitsadmin /GetPeerCachingFlags <Job> 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例
下面的示例检索名为的作业的标志*myJob*。

```
C:\>bitsadmin /GetPeerCachingFlags myJob
```

## <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)



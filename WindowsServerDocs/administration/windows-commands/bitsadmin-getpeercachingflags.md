---
title: bitsadmin getpeercachingflags
description: Windows 命令主题 for **bitsadmin getpeercachingflags** -检索标志，该标志确定是否可以缓存作业的文件并将其提供给对等方，以及 BITS 是否可以从对等方下载作业的内容。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6b86214b5289a59e8db2ecff065ab3b8cd17007e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381445"
---
# <a name="bitsadmin-getpeercachingflags"></a>bitsadmin getpeercachingflags

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

检索标志，该标志确定是否可以缓存作业的文件并将其提供给对等方，以及 BITS 是否可以从对等方下载作业的内容。

## <a name="syntax"></a>语法

```
bitsadmin /GetPeerCachingFlags <Job> 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例
下面的示例检索名为*myJob*的作业的标志。

```
C:\>bitsadmin /GetPeerCachingFlags myJob
```

## <a name="additional-references"></a>其他参考
[命令行语法项](command-line-syntax-key.md)



---
title: bitsadmin setpeercachingflags
description: Windows 命令主题**bitsadmin setpeercachingflags** -设置确定如果缓存该作业的文件并将它们提供给对等方，并且该作业可以从对等方下载内容的标志。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d50a6ccd83a6251808ca3d66437e52f641c60a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814248"
---
# <a name="bitsadmin-setpeercachingflags"></a>bitsadmin setpeercachingflags



设置标志，以确定如果缓存该作业的文件并将它们提供给对等方和下载该作业可以从对等机内容。

## <a name="syntax"></a>语法

```
bitsadmin /SetPeerCachingFlags <Job> <value> 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|ReplTest1|值为无符号的整数，位组中的二进制表示形式的以下解释。</br>1-作业可以从对等方下载内容。</br>2-可以缓存并提供给对等方作业的文件。|

## <a name="BKMK_examples"></a>示例

下面的示例设置名为的作业的标志*myJob*这使得它能够从对等方下载内容。
```
C:\>bitsadmin / SetPeerCachingFlags myJob 1 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
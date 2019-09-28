---
title: bitsadmin setpeercachingflags
description: Windows 命令主题 for **bitsadmin setpeercachingflags** -设置标志，这些标志确定是否可以缓存作业的文件并将其提供给对等方，以及作业是否可以从对等方下载内容。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 147f28268f1b4dd6dfb40cff85f073feabbc35a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380460"
---
# <a name="bitsadmin-setpeercachingflags"></a>bitsadmin setpeercachingflags



设置标志，该标志确定是否可以缓存作业的文件并将其提供给对等方以及作业是否可以从对等方下载内容。

## <a name="syntax"></a>语法

```
bitsadmin /SetPeerCachingFlags <Job> <value> 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|ReplTest1|该值是一个无符号整数，它对二进制表示形式中的位进行了以下解释。</br>1-作业可以从对等方下载内容。</br>2-作业的文件可以缓存并提供给对等方。|

## <a name="BKMK_examples"></a>示例

下面的示例设置名为*myJob*的作业的标志，它允许其从对等方下载内容。
```
C:\>bitsadmin / SetPeerCachingFlags myJob 1 
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
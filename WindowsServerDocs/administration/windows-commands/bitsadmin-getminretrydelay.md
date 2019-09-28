---
title: bitsadmin getminretrydelay
description: Windows 命令主题 for **bitsadmin getminretrydelay** -检索在尝试传输文件之前服务遇到暂时性错误后等待的时间长度（以秒为单位）。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a2bde6340034e48b97b4c86f48a3b2ef72560a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381548"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay



检索时间 （秒），该服务在尝试传输文件之前遇到暂时性错误后等待的长度。

## <a name="syntax"></a>语法

```
bitsadmin /GetMinRetryDelay <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的最小重试延迟 *myDownloadJob*。
```
C:\>bitsadmin /GetMinRetryDelay myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
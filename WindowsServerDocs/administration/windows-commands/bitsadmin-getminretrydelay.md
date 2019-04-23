---
title: bitsadmin getminretrydelay
description: Windows 命令主题**bitsadmin getminretrydelay** -检索时间 （秒），该服务在尝试传输文件之前遇到暂时性错误后等待的长度。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2a6df9faab8340994ad9219a863ad8e50186ccd1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832198"
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

[命令行语法解答](command-line-syntax-key.md)
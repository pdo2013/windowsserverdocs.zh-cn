---
title: bitsadmin setminretrydelay
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 640492cf690a934e3e3b8d0ecf8ca7a0d6a7dc2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813078"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

设置以秒为单位，BITS 在尝试传输文件之前遇到暂时性错误后等待的时间，最小长度。

## <a name="syntax"></a>语法

```
bitsadmin /SetMinRetryDelay <Job> <RetryDelay>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|RetryDelay|一个以秒为单位表示的数字。|

## <a name="BKMK_examples"></a>示例

下面的示例设置名为的作业的最小重试延迟*myDownloadJob*到 35 秒。
```
C:\>bitsadmin /SetMinRetryDelay myDownloadJob 35
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
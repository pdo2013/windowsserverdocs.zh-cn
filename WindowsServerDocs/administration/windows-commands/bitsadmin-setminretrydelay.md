---
title: bitsadmin setminretrydelay
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 379dfa8bfdc48969f268fd1c9544d3bee8bbe646
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380510"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

设置在尝试传输文件之前，在遇到暂时性错误后 BITS 等待的最小时间长度（以秒为单位）。

## <a name="syntax"></a>语法

```
bitsadmin /SetMinRetryDelay <Job> <RetryDelay>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|RetryDelay|以秒表示的数字。|

## <a name="BKMK_examples"></a>示例

下面的示例将名为*myDownloadJob*的作业的最小重试延迟设置为35秒。
```
C:\>bitsadmin /SetMinRetryDelay myDownloadJob 35
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
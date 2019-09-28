---
title: bitsadmin resume
description: '**Bitsadmin 恢复**的 Windows 命令主题-在传输队列中激活新的或挂起的作业。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1393e959980b72de09c546ced763a506d334b56c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380769"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume



激活传输队列中的新作业或挂起的作业。

## <a name="syntax"></a>语法

```
bitsadmin /Resume <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例恢复名为*myDownloadJob*的作业。
```
C:\>bitsadmin /Resume myDownloadJob
```
其他参考

[命令行语法项](command-line-syntax-key.md)
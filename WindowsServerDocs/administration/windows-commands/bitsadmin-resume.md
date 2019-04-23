---
title: bitsadmin resume
description: Windows 命令主题**bitsadmin 恢复**-激活新的或挂起作业的传输队列中。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 76027ac927f8a9bb2558e3ce6d75e4f6692e56e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842028"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume



激活新的或挂起作业的传输队列中。

## <a name="syntax"></a>语法

```
bitsadmin /Resume <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例将名为的作业继续执行*myDownloadJob*。
```
C:\>bitsadmin /Resume myDownloadJob
```
其他参考

[命令行语法解答](command-line-syntax-key.md)
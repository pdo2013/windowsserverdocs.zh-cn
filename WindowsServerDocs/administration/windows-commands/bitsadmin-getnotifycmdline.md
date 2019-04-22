---
title: bitsadmin getnotifycmdline
description: Windows 命令主题**bitsadmin getnotifycmdline** -检索在作业完成传输数据时运行的命令行命令。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90fa33e6-aca5-4a23-82bd-19a9f13f8416
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ca7b2e67c0b5672733a25465fba89d1bd69d07a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817288"
---
# <a name="bitsadmin-getnotifycmdline"></a>bitsadmin getnotifycmdline

检索在作业完成传输数据时要执行的命令行命令。

**1.2 及更早版本的 BITS**: 不支持。

## <a name="syntax"></a>语法

```
bitsadmin /GetNotifyCmdLine <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索时的作业名为服务使用的命令行命令 *myDownloadJob* 完成。
```
C:\>bitsadmin /GetNotifyCmdLine myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
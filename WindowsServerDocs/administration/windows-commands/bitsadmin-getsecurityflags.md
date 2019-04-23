---
title: bitsadmin getsecurityflags
description: Windows 命令主题**bitsadmin getsecurityflags** -报告 URL 重定向的 HTTP 安全标志并在传输期间检查服务器证书上执行。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 97d889b54c4b0de0230c50e1d7c8d21617ea881a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835848"
---
#<a name="bitsadmin-getsecurityflags"></a>bitsadmin getsecurityflags

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在传输期间，服务器证书上执行 URL 重定向和检查报告 HTTP 安全标志。

## <a name="syntax"></a>语法

```
bitsadmin /GetSecurityFlags <Job> 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例
下面的示例从名为的作业检索 securitly 标志*myJob*。

```
C:\>bitsadmin /GetSecurityFlags myJob 
```

## <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)



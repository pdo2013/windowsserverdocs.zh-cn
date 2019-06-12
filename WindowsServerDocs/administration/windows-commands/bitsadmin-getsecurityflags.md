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
ms.openlocfilehash: 6e1db167b12d47afccb8842da617f1e9fe72acff
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434960"
---
# <a name="bitsadmin-getsecurityflags"></a>bitsadmin getsecurityflags

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
[命令行语法项](command-line-syntax-key.md)



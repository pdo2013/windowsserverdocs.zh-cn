---
title: bitsadmin getsecurityflags
description: 适用于**bitsadmin getsecurityflags**的 Windows 命令主题-报告在传输过程中对服务器证书执行的 URL 重定向和检查的 HTTP 安全标志。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fb53664a6366b411ae1eb9b0fe7c93392d60b542
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381457"
---
# <a name="bitsadmin-getsecurityflags"></a>bitsadmin getsecurityflags

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

报告在传输过程中对服务器证书执行的 URL 重定向和检查的 HTTP 安全标志。

## <a name="syntax"></a>语法

```
bitsadmin /GetSecurityFlags <Job> 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例
下面的示例从名为*myJob*的作业中检索 securitly 标志。

```
C:\>bitsadmin /GetSecurityFlags myJob 
```

## <a name="additional-references"></a>其他参考
[命令行语法项](command-line-syntax-key.md)



---
title: bitsadmin sethelpertoken
description: 适用于**bitsadmin sethelpertoken**的 Windows 命令主题-将当前命令提示符的主要令牌（或任意本地用户帐户的令牌，如果指定）设置为 BITS 传输作业的帮助令牌。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 91c03366998168dad9ab4530ef36a5020b8ad6ec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380566"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

将当前命令提示符的主要令牌（或任意本地用户帐户的令牌，如果指定）设置为 BITS 传输作业的 [帮助令牌](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs)。

**BITS 3.0 及更早版本**： 不受支持。

## <a name="syntax"></a>语法

```
bitsadmin /SetHelperToken <Job> [\<username@domain\> \<password\>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|作业的显示名称或 GUID。|
|\< @ no__t-1 @ no__t-2 \<password @ no__t-4|可选 @ no__t-0The 要使用其令牌的本地用户帐户的凭据。|

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

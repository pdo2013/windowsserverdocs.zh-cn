---
title: bitsadmin sethelpertoken
description: Windows 命令主题**bitsadmin sethelpertoken** -设置的 BITS 传输作业帮助器标记为当前命令提示符的主令牌 （或任意本地用户帐户的令牌中，如果指定）。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 558a1aca66a7b3ec447136ceff9237d13efe4ede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852998"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

将当前命令提示符的主令牌 （或任意本地用户帐户的令牌中，如果指定） 设置的 BITS 传输作业 [帮助器令牌](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs)。

**BITS 3.0 及更早版本**: 不支持。

## <a name="syntax"></a>语法

```
bitsadmin /SetHelperToken <Job> [\<username@domain\> \<password\>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|作业的显示名称或 GUID。|
|\<username@domain\> \<password\>|可选&mdash;帐户使用的令牌的本地用户的凭据。|

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

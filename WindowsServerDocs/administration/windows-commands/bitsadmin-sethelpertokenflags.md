---
title: bitsadmin sethelpertokenflags
description: 适用于**bitsadmin sethelpertokenflags**的 Windows 命令主题-设置与 BITS 传输作业关联的帮助程序令牌的用法标志。
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
ms.openlocfilehash: 6047c63677fac3311634ababb675be5301b7f3b5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380582"
---
# <a name="bitsadmin-sethelpertokenflags"></a>bitsadmin sethelpertokenflags

为 [帮助令牌](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs)设置使用标志 @no__t 与 BITS 传输作业关联的1。

**BITS 3.0 及更早版本**： 不受支持。

## <a name="syntax"></a>语法

```
bitsadmin /SetHelperTokenFlags <Job> <Flags>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|作业的显示名称或 GUID。|
|Flags|可能的值包括以下各项。 0x0001 @ no__t-0The 帮助程序令牌用于打开上传作业的本地文件，创建或重命名下载作业的临时文件，或创建或重命名上传答复作业的答复文件。 0x0002 @ no__t-0The 帮助程序令牌用于打开服务器消息块（SMB）上传或下载作业的远程文件，或响应隐式 NTLM 或 Kerberos 凭据的 HTTP 服务器或代理质询。 必须调用 @ no__t-0 @ no__t-1to 允许通过 HTTP 发送凭据。|

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

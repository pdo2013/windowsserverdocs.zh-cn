---
title: bitsadmin sethelpertokenflags
description: Windows 命令主题**bitsadmin sethelpertokenflags** -设置与 BITS 传输作业相关联的帮助程序令牌使用情况标记。
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
ms.openlocfilehash: cc9652afe73476041aa42e64671885bfc1af9628
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813858"
---
# <a name="bitsadmin-sethelpertokenflags"></a>bitsadmin sethelpertokenflags

设置的使用情况标志 [帮助器标记](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs) 与 BITS 传输作业相关联。

**BITS 3.0 及更早版本**: 不支持。

## <a name="syntax"></a>语法

```
bitsadmin /SetHelperTokenFlags <Job> <Flags>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|作业的显示名称或 GUID。|
|Flags|可能的值包括以下内容。 0x0001&mdash;帮助程序令牌使用打开一个上载作业的本地文件，以创建或重命名临时文件的下载作业，或者要创建或重命名上载-答复作业答复文件。 0x0002&mdash;帮助程序令牌用来打开远程文件的服务器消息块 (SMB) 上传或下载作业，或在响应 HTTP 服务器或代理服务器质询的隐式 NTLM 或 Kerberos 凭据。 必须调用 `/SetCredentialsJob TargetScheme NULL NULL` 以允许通过 HTTP 发送的凭据。|

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

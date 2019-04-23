---
title: bitsadmin setcredentials
description: Windows 命令主题**bitsadmin setcredentials** -将凭据添加到作业。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cd099a4-9e85-46d8-8527-edb6dfab7f97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 923dcff7d268d40b72db3254e2a97c808c7c7253
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877388"
---
# <a name="bitsadmin-setcredentials"></a>bitsadmin setcredentials

将凭据添加到作业。

**1.2 及更早版本的 BITS**: 不支持。

## <a name="syntax"></a>语法

```
bitsadmin /SetCredentials <Job> <Target> <Scheme> <Username> <Password>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|目标|服务器或代理|
|方案|下列情况之一：</br>-基本-在其中的用户名和密码发送到服务器或代理的明文形式的身份验证方案。</br>-摘要-使用服务器指定的数据字符串用于质询质询-响应身份验证方案。</br>-NTLM，使用 Windows 网络环境中进行身份验证的用户的凭据质询响应身份验证方案。</br>-NEGOTIATE — 也称为简单和受保护的协商协议 (Snego) 是与服务器或代理服务器以确定要用于身份验证的方案协商一种质询响应身份验证方案。 例如，Kerberos 协议和 NTLM。</br>-PASSPORT — 由 Microsoft 提供了单一登录为成员站点提供的集中式身份验证服务。|
|Username|所提供的凭据的名称|
|密码|与所提供关联的密码*用户名*|

## <a name="BKMK_examples"></a>示例

名为的作业的以下示例 Adds 凭据*myDownloadJob*。
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC Edward Password20
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
---
title: bitsadmin removecredentials
description: 适用于**bitsadmin removecredentials**的 Windows 命令主题-从作业中删除凭据。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a78ce9a-1feb-4811-a000-cce81287b22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 34a5bae9304a9db9f47f437276270ca06b1ebeee
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380825"
---
# <a name="bitsadmin-removecredentials"></a>bitsadmin removecredentials

从作业中删除凭据。

**BITS 1.2 及更早版本**： 不受支持。

## <a name="syntax"></a>语法

```
bitsadmin /RemoveCredentials <Job> <Target> <Scheme>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|目标|服务器或代理|
|机制|下列情况之一：</br>-基本-身份验证方案，在此方案中，用户名和密码以明文形式发送到服务器或代理。</br>-摘要-一种质询-响应身份验证方案，该方案将服务器指定的数据字符串用于质询。</br>-NTLM-一种质询-响应身份验证方案，该方案使用用户的凭据在 Windows 网络环境中进行身份验证。</br>-NEGOTIATE，也称为简单和受保护的协商协议（Snego）是一种质询响应身份验证方案，该方案与服务器或代理协商以确定要用于身份验证的方案。 例如，Kerberos 协议和 NTLM。</br>-PASSPORT — Microsoft 提供的一种集中式身份验证服务，可为成员站点提供单一登录。|

## <a name="BKMK_examples"></a>示例

下面的示例从名为*myDownloadJob*的作业中删除凭据。
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
---
title: ksetup:addkpasswd
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3196995-1b38-48ff-ba08-911cfab77317
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a85eb6dfe30c33126504744a7659fe2cc573087
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856818"
---
# <a name="ksetupaddkpasswd"></a>ksetup:addkpasswd



将添加一个领域 Kerberos 密码 (Kpasswd) 服务器地址。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /addkpasswd <RealmName> [<KpasswdName>]
```

### <a name="parameters"></a>Parameters

如果 Kerberos 领域，工作站将进行身份验证支持 Kerberos 更改密码协议，则可以配置运行 Windows 操作系统为使用 Kerberos 密码服务器的客户端计算机。 在领域端配置此设置。

|参数|描述|
|---------|-----------|
|\<RealmName>|领域名称表述为大写的 DNS 名称，例如 corp.CONTOSO.COM，并列出默认值为领域 = 当**ksetup**运行。|
|\<KpasswdName>|要用作 Kerberos 密码服务器的 KDC 名称表示为一个不区分大小写完全限定的域名，例如 mitkdc.microsoft.com。 如果省略 KDC 名称时，可能会使用 DNS 来查找 Kdc。|

## <a name="remarks"></a>备注

如果 Kerberos 领域，工作站将进行身份验证支持 Kerberos 更改密码协议，则可以配置运行 Windows 操作系统为使用 Kerberos 密码服务器的客户端计算机。

运行命令**ksetup**以验证 KDC 名称。 如果**kpasswd =** 不会出现在输出中，映射未配置。

可以一次添加其他 KDC 姓名。

## <a name="BKMK_Examples"></a>示例

配置 CORP.的领域，CONTOSO.COM，以便其使用的非 Windows KDC 服务器，mitkdc.contoso.com，为密码服务器：
```
ksetup /addkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
这会导致控制进行身份验证领域之间的所有密码的非 Windows Kerberos 密码服务器。

#### <a name="additional-references"></a>其他参考

-   [Ksetup](ksetup.md)
-   [Ksetup:delkpasswd](ksetup-delkpasswd.md)
-   [命令行语法解答](command-line-syntax-key.md)
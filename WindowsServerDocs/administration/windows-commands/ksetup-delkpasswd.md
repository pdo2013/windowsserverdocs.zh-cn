---
title: ksetup:delkpasswd
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e487389e0f27f58aacaea2b81d573dc9a965e42f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889078"
---
# <a name="ksetupdelkpasswd"></a>ksetup:delkpasswd

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

删除领域 Kerberos 密码服务器 (Kpasswd)。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。
## <a name="syntax"></a>语法
```
ksetup /delkpasswd <RealmName> <KpasswdName>
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|<RealmName>|领域名称表述为大写的 DNS 名称，例如 corp.CONTOSO.COM，并列出默认值为领域 = 当**ksetup**运行。|
|<KpasswdName>|要用作 Kerberos 密码服务器的 KDC 名称表示为一个不区分大小写的完全限定域名，例如 mitkdc.contoso.com。 如果省略 KDC 名称时，可能会使用 DNS 来查找 Kdc。|
## <a name="remarks"></a>备注
运行命令**ksetup**以验证 KDC 名称。 如果**kpasswd =** 不出现在输出中，那么尚未配置映射。 将列出多个映射，如果设置。
## <a name="BKMK_Examples"></a>示例
验证领域 corp.CONTOSO.COM 使用非 Windows KDC 服务器 mitkdc.contoso.com 作为密码服务器：
```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
若要验证命令按预期方式正常工作，请运行**ksetup**要验证领域 CORP.的 Windows 计算机上CONTOSO.COM 未映射到 Kerberos 密码服务器 （KDC 名称）。
## <a name="additional-references"></a>其他参考
-   [ksetup](ksetup.md)
-   [ksetup:delkpasswd](ksetup-delkpasswd.md)
-   [命令行语法解答](command-line-syntax-key.md)

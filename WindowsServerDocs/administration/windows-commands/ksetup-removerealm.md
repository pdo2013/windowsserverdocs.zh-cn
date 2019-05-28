---
title: ksetup:removerealm
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 579b0772e4642389b90aa370dad80a3eebea9d34
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564718"
---
# <a name="ksetupremoverealm"></a>ksetup:removerealm



从注册表中删除指定的领域的所有信息。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /removerealm <RealmName>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<RealmName>|领域名称表述为大写的 DNS 名称，例如 corp.CONTOSO.COM，和它被列为的默认领域时**ksetup**运行。|

## <a name="remarks"></a>备注

领域名称存储在注册表中的两个位置中：**HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001**并 **\CurrentControlSet\Control\Lsa\Kerberos**。

无法从域控制器中删除的默认领域名称，因为这会重置其 DNS 信息，并将其删除可能会使域控制器不可用。

## <a name="BKMK_Examples"></a>示例

错误地设置领域名称的拼写错误".COM"本地计算机上为 corp.CONTOSO。CON
```
ksetup /setrealm CORP.CONTOSO.CON
```
从本地计算机中删除该错误的领域名称：
```
ksetup /removerealm CORP.CONTOSO.CON
```
通过运行验证删除**ksetup**并查看输出。

#### <a name="additional-references"></a>其他参考

-   [Ksetup](ksetup.md)
-   [Ksetup:setrealm](ksetup-setrealm.md)
-   [命令行语法项](command-line-syntax-key.md)
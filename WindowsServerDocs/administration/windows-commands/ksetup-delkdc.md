---
title: ksetup:delkdc
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7d6ec389-094c-4a7b-a78b-605497ddc289
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e2fa065ca60338b04cc8718e199805cc494c908
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814558"
---
# <a name="ksetupdelkdc"></a>ksetup:delkdc



删除实例的 Kerberos 领域的密钥分发中心 (KDC) 名称。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /delkdc <RealmName> <KDCName>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<RealmName>|领域名称表述为大写的 DNS 名称，例如 corp.CONTOSO.COM，和它被列为的默认领域时**ksetup**运行。 它是从其正在尝试删除其他 KDC 此领域。|
|\<KDCName>|KDC 名称表示为一个不区分大小写的完全限定域名，例如 mitkdc.contoso.com。|

## <a name="remarks"></a>备注

这些映射存储在注册表中**HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains**。 若要从多台计算机中删除领域配置数据，使用安全配置模板管理单元和策略分发，而不是使用**ksetup**显式各台计算机上。

在运行 Windows 2000 Server Service Pack 1 (SP1) 及更早版本的计算机，计算机必须重新启动之前将使用已更改的领域的设置配置。

若要验证计算机的默认领域名称或验证此命令按预期起作用，运行**ksetup**在命令提示符处，并验证已删除的 KDC 不存在列表中。

## <a name="BKMK_Examples"></a>示例

此计算机的安全要求已更改，因此必须删除 Windows 领域和非 Windows 领域之间的链接。 首先，确定哪些关联，以删除和生成现有的关联的输出：
```
ksetup
```
通过使用以下命令删除的关联：
```
Ksetup /delkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

#### <a name="additional-references"></a>其他参考

-   [Ksetup](ksetup.md)
-   [命令行语法解答](command-line-syntax-key.md)
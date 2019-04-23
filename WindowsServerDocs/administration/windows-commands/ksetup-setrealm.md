---
title: ksetup:setrealm
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aa6b2a21904ec4dae1e60def5bd36647291b1af6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877398"
---
# <a name="ksetupsetrealm"></a>ksetup:setrealm



设置 Kerberos 领域的名称。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /setrealm <DNSDomainName>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<DNSDomainName>|完全限定的域名或域名简单的形式可以是 DNS 域名。|

## <a name="remarks"></a>备注

应以大写字母输入 DNS 域名参数。 否则为**ksetup**命令将要求提供验证，以继续。

不支持在域控制器上设置 Kerberos 领域。 尝试这样做将导致警告和命令失败。

## <a name="BKMK_Examples"></a>示例

为此计算机添加到特定的域名来限制由非域控制器只需对 CONTOSO Kerberos 领域的访问设置的领域：
```
ksetup /setrealm CONTOSO
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)
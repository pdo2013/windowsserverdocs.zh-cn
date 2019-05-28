---
title: ksetup:addkdc
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98bfc23a-14c4-401c-bcb3-9903c5cdde64
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0466bee0b357e896bd971152a56da57612472672
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564730"
---
# <a name="ksetupaddkdc"></a>ksetup:addkdc



添加给定的 Kerberos 领域的密钥分发中心 (KDC) 地址。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /addkdc <RealmName> [<KDCName>] 
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<RealmName>|领域名称表述为大写的 DNS 名称，例如 corp.CONTOSO.COM，和它被列为的默认领域时**ksetup**运行。 它是您尝试添加其他 KDC 此领域。|
|\<KDCName>|KDC 名称表示为一个不区分大小写完全限定的域名，例如 mitkdc.microsoft.com。 如果省略 KDC 名称时，DNS 将找到 Kdc。|

## <a name="remarks"></a>备注

这些映射存储在注册表中**HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains**。 若要将 Kerberos 领域配置数据部署到多台计算机，使用安全配置模板管理单元和策略分发，而不是使用**ksetup**显式各台计算机上。

必须重新启动计算机，然后将使用新的 realm 设置。

若要验证计算机的默认领域名称或验证此命令按预期起作用，运行**ksetup**在命令提示符处，并添加 kdc 验证输出。

## <a name="BKMK_Examples"></a>示例

配置 Windows KDC 服务器和工作站应使用的领域：
```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```
如前面的命令将本地计算机帐户密码设置为在同一台计算机的命令行处运行 Ksetup 工具"p@sswrd1%"。 然后重新启动计算机。
```
Ksetup /setcomputerpassword p@sswrd1%
```

#### <a name="additional-references"></a>其他参考

-   [Ksetup](ksetup.md)
-   [Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)
-   [命令行语法项](command-line-syntax-key.md)
---
title: ksetup:delrealmflags
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 22053041-1eb4-47f5-bed9-3d5681bcde7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd2a3897a07a2eda4c05526b0ae8c55dda35e1e9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437991"
---
# <a name="ksetupdelrealmflags"></a>ksetup:delrealmflags



从指定的领域中删除领域标志。  有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /delrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<RealmName>|领域名称表述为大写的 DNS 名称，例如 corp.CONTOSO.COM，并且被列为的默认领域时**Ksetup**运行。|

## <a name="remarks"></a>备注

领域标志指定 Kerberos 领域不基于 Windows Server 操作系统的其他功能。 运行 Windows Server 2003、 Windows Server 2008 或 Windows Server 2008 R2 的计算机可以使用 Kerberos 服务器来管理身份验证而不是使用域运行 Windows Server 操作系统，并且这些系统参与Kerberos 领域。 此项建立的领域的功能。 下表描述了每个。

|ReplTest1|领域标志|描述|
|-----|----------|-----------|
|0xF|全部|设置所有领域标志。|
|0x00|无|未设置任何领域标志，并启用任何其他功能。|
|0x01|SendAddress|IP 地址将是包含在票证-授予的票证。|
|0x02|TcpSupported|在此领域中支持传输控制协议 (TCP) 和用户数据报协议 (UDP)。|
|0x04|委派|在此领域中的每个人都是受信任可以进行委派。|
|0x08|NcSupported|此领域支持名称规范化，允许对 DNS 和领域命名标准。|
|0x80|RC4|此领域支持 RC4 加密来启用跨领域信任，可以利用 TLS。|

在注册表中存储领域标志**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\\** <em>RealmName</em>。 默认情况下，注册表中不存在此项。 可以使用[Ksetup:addrealmflags](ksetup-addrealmflags.md)命令来填充注册表。

您可以看到哪些领域标志是可用，并且已设置通过查看的输出**ksetup**或**ksetup /dumpstate**。

## <a name="BKMK_Examples"></a>示例

列出 CONTOSO 的领域的可用领域标志：
```
Ksetup /listrealmflags
```
删除当前位于集中的两个标志：
```
ksetup /delrealmflags CONTOSO ncsupported delegate
```
运行**ksetup**命令来验证是否通过查看输出并查找设置领域标志**领域标志 =** 。

#### <a name="additional-references"></a>其他参考

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [命令行语法项](command-line-syntax-key.md)
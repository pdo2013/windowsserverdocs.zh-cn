---
title: ksetup:addrealmflags
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 80ca1e16-8871-494b-b9be-6bc9d63de860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbc878bd0ee25ad92c640710ab6b46bbc0eaf62a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827678"
---
# <a name="ksetupaddrealmflags"></a>ksetup:addrealmflags



将其他领域标志添加到指定的领域。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /addrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|领域名称|领域名称表述为大写的 DNS 名称，例如 corp.CONTOSO.COM。|

## <a name="remarks"></a>备注

领域标志指定 Kerberos 领域不基于 Windows Server 操作系统的其他功能。 运行 Windows Server 2003、 Windows Server 2008 或 Windows Server 2008 R2 的计算机可以使用 Kerberos 服务器来管理身份验证而不是使用域运行 Windows Server 操作系统，并且这些系统参与Kerberos 领域。 此项建立的领域的功能。 下表描述了每个。

|值|领域标志|描述|
|-----|----------|-----------|
|0xF|全部|设置所有领域标志。|
|0x00|无|未设置任何领域标志，并启用任何其他功能。|
|0x01|SendAddress|将包含在票证授予票证 IP 地址。|
|0x02|TcpSupported|在此领域中支持传输控制协议 (TCP) 和用户数据报协议 (UDP)。|
|0x04|委派|在此领域中的每个人都是受信任可以进行委派。|
|0x08|NcSupported|此领域支持名称规范化，允许对 DNS 和领域命名标准。|
|0x80|RC4|此领域支持 RC4 加密来启用跨领域信任，可以利用 TLS。|

注册表中存储领域标志**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\*** 领域-名称 *。 默认情况下，注册表中不存在此项。 可以使用[Ksetup:addrealmflags](ksetup-addrealmflags.md)命令来填充注册表。

您可以看到哪些领域标志是可用，并且已设置通过查看 ksetup 或 ksetup /dumpstate 的输出。

## <a name="BKMK_Examples"></a>示例

列出 CONTOSO 的领域的可用领域标志：
```
Ksetup /listrealmflags
```
将两个标志设置为 CONTOSO 领域：
```
ksetup /setrealmflags CONTOSO ncsupported delegate
```
添加一个当前未在集中的多个标志：
```
ksetup /addrealmflags CONTOSO SendAddress
```
运行**ksetup**命令来验证是否通过查看输出并查找设置领域标志**领域标志 =**。

#### <a name="additional-references"></a>其他参考

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   [命令行语法解答](command-line-syntax-key.md)
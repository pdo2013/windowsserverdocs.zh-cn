---
title: ksetup:listrealmflags
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bc4be8be747c31d60d75c90ad3aa831dd8dff93
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838298"
---
# <a name="ksetuplistrealmflags"></a>ksetup:listrealmflags



列出可以通过报告的可用领域标志**ksetup**。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /listrealmflags
```

### <a name="parameters"></a>Parameters

无

## <a name="remarks"></a>备注

领域标志指定非基于 Windows 的 Kerberos 领域的其他功能。 运行 Windows Server 2003、 Windows Server 2008 或 Windows Server 2008 R2 的计算机可以使用非基于 Windows 的 Kerberos 服务器来管理身份验证而不是使用运行 Windows Server 操作系统的域。 这些系统加入 Kerberos 领域，而不是 Windows 域。 此项建立的领域的功能。 下表描述了每个。

|值|领域标志|描述|
|-----|----------|-----------|
|0xF|全部|设置所有领域标志。|
|0x00|无|未设置任何领域标志，并启用任何其他功能。|
|0x01|SendAddress|将包含在票证授予票证 IP 地址。|
|0x02|TcpSupported|在此领域中支持传输控制协议 (TCP) 和用户数据报协议 (UDP)。|
|0x04|委派|在此领域中的每个人都是受信任可以进行委派。|
|0x08|NcSupported|此领域支持名称规范化，允许对 DNS 和领域命名标准。|
|0x80|RC4|此领域支持 RC4 加密来启用跨领域信任，可以利用 TLS。|

在注册表中存储领域标志**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\*** 领域-名称 *。 默认情况下，注册表中不存在此项。 可以使用[Ksetup:addrealmflags](ksetup-addrealmflags.md)命令来填充注册表。

## <a name="BKMK_Examples"></a>示例

列出此计算机上的已知的领域标志：
```
ksetup /listrealmflags
```
设置可用领域标志**Ksetup**不知道通过在命令行键入以下命令之一：
```
ksetup /setrealmflags CORP.CONTOSO.COM sendaddress tcpsupported delete ncsupported
```
```
ksetup /setrealmflags CORP.CONTOSO.COM 0xF
```

#### <a name="additional-references"></a>其他参考

-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   [Ksetup](ksetup.md)
-   [命令行语法解答](command-line-syntax-key.md)
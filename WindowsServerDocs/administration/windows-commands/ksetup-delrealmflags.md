---
title: ksetup： delrealmflags
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8e2e67d7af4fdd31b79ad633c9df844483bb1ea3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375101"
---
# <a name="ksetupdelrealmflags"></a>ksetup： delrealmflags



删除指定领域中的领域标志。  有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /delrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<RealmName >|领域名称被声明为大写的 DNS 名称，例如 CORP。CONTOSO.COM，在**Ksetup**运行时，将作为默认领域列出。|

## <a name="remarks"></a>备注

领域标志指定了不基于 Windows Server 操作系统的 Kerberos 领域的其他功能。 运行 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 的计算机可以使用 Kerberos 服务器来管理身份验证，而不是使用运行 Windows Server 操作系统的域，这些系统参与Kerberos 领域。 此条目将建立领域的功能。 下表对每个进行了说明。

|ReplTest1|领域标志|描述|
|-----|----------|-----------|
|0xF|全部|设置所有领域标志。|
|0x00|无|未设置领域标志，并且未启用任何其他功能。|
|0x01|SendAddress|此 IP 地址将包含在票证授予票证中。|
|0x02|TcpSupported|此领域支持传输控制协议（TCP）和用户数据报协议（UDP）。|
|0x04|委派|此领域中的每个人都受信任，可用于委派。|
|0x08|NcSupported|此领域支持名称规范化，这允许 DNS 和领域的命名标准。|
|0x80|RC4|此领域支持 RC4 加密以启用跨领域信任，这允许使用 TLS。|

领域标志存储在注册表的**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains @ no__t-1**<em>RealmName</em>中。 默认情况下，注册表中不存在此项。 可以使用[Ksetup： addrealmflags](ksetup-addrealmflags.md)命令填充注册表。

可以通过查看**ksetup**或**ksetup/dumpstate**的输出来了解哪些领域标志可用和设置。

## <a name="BKMK_Examples"></a>示例

列出领域 CONTOSO 的可用领域标志：
```
Ksetup /listrealmflags
```
删除集中当前有两个标志：
```
ksetup /delrealmflags CONTOSO ncsupported delegate
```
运行**ksetup**命令，以验证是否已通过查看输出和查找**领域**标志来设置领域标志。

#### <a name="additional-references"></a>其他参考

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [命令行语法项](command-line-syntax-key.md)
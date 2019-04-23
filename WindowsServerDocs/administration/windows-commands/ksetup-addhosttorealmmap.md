---
title: ksetup:addhosttorealmmap
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 237742d5-fa68-466c-b97e-636f489248ea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25cf258309c94f0efde980018dd5dcf3c7df4d60
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837498"
---
# <a name="ksetupaddhosttorealmmap"></a>ksetup:addhosttorealmmap



添加一个服务主体名称 (SPN) 映射规定的主机和领域之间。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /addhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<HostName>|主机名是计算机名称，它可以表示为计算机的完全限定的域名。|
|\<RealmName>|领域名称表述为大写的 DNS 名称，例如 corp.CONTOSO.COM。|

## <a name="remarks"></a>备注

此命令可将主机或共享到领域相同的 DNS 后缀的多个主机映射。

映射记录在注册表中**HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**。

## <a name="BKMK_Examples"></a>示例

作为配置 CONTOSO 的领域的一部分，将主机计算机 IPops897 映射到领域：
```
ksetup /addhosttorealmmap IPops897 CONTOSO
```
验证在注册表中的映射是按预期方式。

#### <a name="additional-references"></a>其他参考

-   [Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [命令行语法解答](command-line-syntax-key.md)
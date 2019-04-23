---
title: ksetup:delhosttorealmmap
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cf01edc4932fd5ec1cf98043de04286b3a100a34
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882338"
---
# <a name="ksetupdelhosttorealmmap"></a>ksetup:delhosttorealmmap



删除规定的主机和领域之间的服务主体名称 (SPN) 映射。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /delhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<HostName>|主机名是计算机名称，它可以表示为计算机的完全限定的域名。|
|\<RealmName>|领域名称表述为大写的 DNS 名称，例如 corp.CONTOSO.COM。|

## <a name="remarks"></a>备注

如果存在领域 （或到领域的多个主机），它映射到主机，此命令将删除该映射。

映射记录在注册表中**HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**。 您应该验证注册表中使用此命令后的映射。

## <a name="BKMK_Examples"></a>示例

更改领域 CONTOSO 的配置，删除主计算机 IPops897 到领域的映射：
```
ksetup /delhosttorealmmap IPops897 CONTOSO
```
运行此命令后，你可以验证在注册表中的映射是按预期方式。

#### <a name="additional-references"></a>其他参考

-   [Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [命令行语法解答](command-line-syntax-key.md)
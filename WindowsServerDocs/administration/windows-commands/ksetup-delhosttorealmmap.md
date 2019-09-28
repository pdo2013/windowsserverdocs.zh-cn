---
title: ksetup： delhosttorealmmap
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 70b54aaebc0b7b46c34c6f52e45f6583afd6c477
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375148"
---
# <a name="ksetupdelhosttorealmmap"></a>ksetup： delhosttorealmmap



删除所述主机和领域之间的服务主体名称（SPN）映射。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /delhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<HostName >|主机名是计算机的名称，可将其声明为计算机的完全限定的域名。|
|\<RealmName >|领域名称被声明为大写的 DNS 名称，例如 CORP。CONTOSO.COM。|

## <a name="remarks"></a>备注

当存在到领域（或多个主机到领域）映射的情况下，此命令将删除该映射。

映射将记录在**HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**中的注册表中。 使用此命令后，应验证注册表中的映射。

## <a name="BKMK_Examples"></a>示例

更改领域 CONTOSO 的配置，删除主计算机 IPops897 到领域的映射：
```
ksetup /delhosttorealmmap IPops897 CONTOSO
```
运行此命令后，可以在注册表中验证映射是否按预期进行。

#### <a name="additional-references"></a>其他参考

-   [Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [命令行语法项](command-line-syntax-key.md)
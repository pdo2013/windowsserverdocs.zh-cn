---
title: ksetup： addhosttorealmmap
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e6a28c6001707fac245de7136b5fb5bd38495027
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375649"
---
# <a name="ksetupaddhosttorealmmap"></a>ksetup： addhosttorealmmap



在所述的主机和领域之间添加服务主体名称（SPN）映射。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /addhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<HostName >|主机名是计算机的名称，可将其声明为计算机的完全限定的域名。|
|\<RealmName >|领域名称被声明为大写的 DNS 名称，例如 CORP。CONTOSO.COM。|

## <a name="remarks"></a>备注

此命令允许你将共享同一 DNS 后缀的一个或多个主机映射到领域。

映射将记录在**HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**中的注册表中。

## <a name="BKMK_Examples"></a>示例

在配置领域 CONTOSO 的过程中，将主机计算机 IPops897 映射到领域：
```
ksetup /addhosttorealmmap IPops897 CONTOSO
```
在注册表中验证映射是否按预期进行。

#### <a name="additional-references"></a>其他参考

-   [Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [命令行语法项](command-line-syntax-key.md)
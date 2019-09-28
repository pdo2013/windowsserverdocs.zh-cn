---
title: ksetup： delenctypeattr
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fc25ef3-e271-4229-a712-72c507df55aa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e3810d83c06b9ea08766451e13390b02b1867c83
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375156"
---
# <a name="ksetupdelenctypeattr"></a>ksetup： delenctypeattr



删除域的 "加密类型" 属性。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /delenctypeattr <DomainName> 
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<DomainName >|要与之建立连接的域的名称。 使用完全限定的域名或名称的简单格式，如 corp.contoso.com 或 contoso。|

## <a name="remarks"></a>备注

若要查看 Kerberos 票证授予票证（TGT）的加密类型和会话密钥，请运行**klist**命令并查看输出。

成功或失败完成时，将显示一条状态消息。

若要设置要连接到并使用的域，请运行**ksetup/domain \<DomainName >** 命令。

## <a name="BKMK_Examples"></a>示例

确定在此计算机上设置的当前加密类型：
```
klist
```
将域设置为 mit.contoso.com：
```
ksetup /domain mit.contoso.com
```
验证域的加密类型属性：
```
ksetup /getenctypeattr mit.contoso.com
```
删除域 mit.contoso.com 的 "设置加密类型" 属性：
```
ksetup /delenctypeattr mit.contoso.com
```

#### <a name="additional-references"></a>其他参考

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [命令行语法项](command-line-syntax-key.md)
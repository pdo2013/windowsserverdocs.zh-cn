---
title: bitsadmin util 和 getieproxy
description: '**Bitsadmin util 和 getieproxy**的 Windows 命令主题-检索给定服务帐户的代理使用情况。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6936e088ddcf467b5a8f931bc8217ba9da4662c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380268"
---
# <a name="bitsadmin-util-and-getieproxy"></a>bitsadmin util 和 getieproxy

> 适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

检索给定服务帐户的代理使用情况。

## <a name="syntax"></a>语法

```
bitsadmin /Util /GetIEProxy <Account> [/Conn <ConnectionName>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|帐户|指定要检索其代理设置的服务帐户。 可能的值为：<br /><br />-LOCALSYSTEM<br />-NETWORKSERVICE<br />-LOCALSERVICE|
|ConnectionName|可选，与 **/Conn**参数一起使用，以指定要使用的调制解调器连接。 如果未指定 **/Conn**参数，则 BITS 将使用 LAN 连接。 指定紧跟 **/Conn**参数的调制解调器连接名称。|

## <a name="remarks"></a>备注

此开关显示每个代理使用的值，而不仅仅是为服务帐户指定的代理使用情况。 有关设置服务帐户代理使用情况的详细信息，请参阅[bitsadmin util and setieproxy](bitsadmin-util-and-setieproxy.md)开关。

## <a name="BKMK_examples"></a>示例

下面的示例显示了网络服务帐户的代理使用情况。

```
C:\>bitsadmin /Util /GetIEProxy NETWORKSERVICE
```

## <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

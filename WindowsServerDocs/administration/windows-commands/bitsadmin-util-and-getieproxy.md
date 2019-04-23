---
title: bitsadmin util 和 getieproxy
description: Windows 命令主题**bitsadmin util 和 getieproxy** -检索给定的服务帐户的代理使用情况。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: de4f86340b1163c4d8e3286d9c86c9df794a21c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876868"
---
# <a name="bitsadmin-util-and-getieproxy"></a>bitsadmin util 和 getieproxy

> 适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

检索给定的服务帐户的代理使用情况。

## <a name="syntax"></a>语法

```
bitsadmin /Util /GetIEProxy <Account> [/Conn <ConnectionName>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|帐户|指定要检索其代理设置的服务帐户。 可能的值为：<br /><br />-本地系统<br />-   NETWORKSERVICE<br />-LOCALSERVICE|
|ConnectionName|与一起使用的可选 **/conn**参数来指定要使用的调制解调器连接。 如果未指定 **/conn**参数，BITS 使用 LAN 连接。 指定调制解调器连接名称紧跟 **/conn**参数。|

## <a name="remarks"></a>备注

此开关显示的值为每个代理用途，而不仅仅是代理用法指定为服务帐户。 有关设置服务帐户的代理使用情况的详细信息，请参阅[bitsadmin util 和 setieproxy](bitsadmin-util-and-setieproxy.md)切换。

## <a name="BKMK_examples"></a>示例

以下示例显示在网络服务帐户的代理使用情况。

```
C:\>bitsadmin /Util /GetIEProxy NETWORKSERVICE
```

## <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

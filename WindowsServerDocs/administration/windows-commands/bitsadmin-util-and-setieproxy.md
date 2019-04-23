---
title: bitsadmin util 和 setieproxy
description: Windows 命令主题**bitsadmin util 和 setieproxy** -设置代理设置，以使用服务帐户传输文件时使用。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e9f31ba-3070-4ffd-a94c-388c8d78f688
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d4f6ab2e52284895d2e7918364c24bbb69f2b1c9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853508"
---
# <a name="bitsadmin-util-and-setieproxy"></a>bitsadmin util 和 setieproxy

设置使用的服务帐户传输文件时要使用的代理设置。

**BITSAdmin 1.5 和更早版本**: 不支持。

## <a name="syntax"></a>语法

```
bitsadmin /Util /SetIEProxy <Account> <Usage>[/Conn <ConnectionName>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|帐户|指定服务帐户的代理设置，你想要定义的类型。 可能的值为：</br>-本地系统</br>-   NETWORKSERVICE</br>-LOCALSERVICE|
|用法|指定代理服务器检测要使用窗的体。 可能的值为：</br>-NO_PROXY — 不使用代理服务器。</br>-自动检测功能-自动检测代理设置。</br>-MANUAL_PROXY — 使用显式代理列表和跳过列表。 指定代理服务器列表，并跳过紧跟的使用情况标记的列表。 例如，MANUAL_PROXY proxy1 proxy2 为 NULL。</br>    代理列表是代理服务器，以使用的以逗号分隔列表。</br>    -跳过列表是以空格分隔列表的主机名或 IP 地址，或两者，为其传输不会通过代理路由。 这可以是\<本地 > 来指代同一 LAN 上的所有服务器。 值为 NULL 或""可能用于空代理跳过列表。</br>-AUTOSCRIPT — 相同自动检测功能，但它也会执行脚本。 指定使用情况标记之后立即将脚本 URL。 例如，AUTOSCRIPT http://server/proxy.js。</br>-重置 — 相同 NO_PROXY，只不过它将移除的手动代理 Url （如果指定） 和 Url 发现使用自动检测。|
|ConnectionName|可选-用于 **/conn**参数来指定要使用的调制解调器连接。 如果未指定 **/conn**参数，BITS 使用 LAN 连接。 指定调制解调器连接名称紧跟 **/conn**参数。|

## <a name="remarks"></a>备注

使用此开关每个后续调用将替换以前指定的使用情况，但不是使用以前定义的参数。 例如，如果在单独的调用中指定 NO_PROXY、 自动检测功能和 MANUAL_PROXY，BITS 使用上次提供的使用，但保留从以前定义的使用情况的参数。

> [!IMPORTANT]
> 从提升的命令提示符才能成功完成，必须运行此命令。

## <a name="BKMK_examples"></a>示例

下面的示例设置的网络服务帐户的代理使用情况。

```
C:\>bitsadmin /Util /SetIEProxy localsystem AUTODETECT
```

以下是更多示例。

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1,proxy2,proxy3 NULL
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1:80 ""
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
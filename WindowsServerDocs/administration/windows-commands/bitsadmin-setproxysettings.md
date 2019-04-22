---
title: bitsadmin setproxysettings
description: Windows 命令主题**bitsadmin setproxysettings** -设置指定的作业的代理设置。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3503aab55f5650cb9283ce8a9f1a17359bfd48b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825588"
---
# <a name="bitsadmin-setproxysettings"></a>bitsadmin setproxysettings



设置指定的作业的代理设置。

## <a name="syntax"></a>语法

```
bitsadmin /SetProxySettings <Job> <Usage> [List] [Bypass]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|用法|以下值之一：</br>-预配置 — 使用所有者的 Internet Explorer 默认设置。</br>-NO_PROXY — 不使用代理服务器。</br>-替代 — 使用显式代理列表和跳过列表。 必须遵循的代理和代理跳过列表。</br>-自动检测功能-自动检测代理设置。|
|列表|使用时*使用情况*参数设置为替代-包含代理服务器，以使用的以逗号分隔列表。|
|绕过|使用时*使用情况*参数设置为替代 — 包含以空格分隔列表的主机名或 IP 地址，或两者，为其传输不会通过代理路由。 这可以是**\<本地 >** 来指代同一 LAN 上的所有服务器。 值为 NULL 或""可能用于空代理跳过列表。|

## <a name="BKMK_examples"></a>示例

下面的示例设置名为的作业的代理设置*myDownloadJob*。

```
C:\>bitsadmin /SetProxySettings myDownloadJob PRECONFIG
```

下面是一些其他示例。

```
bitsadmin /setproxysettings myDownloadJob NO_PROXY
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1:80 ""
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1,proxy2,proxy3 NULL
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
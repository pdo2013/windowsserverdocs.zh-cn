---
title: bitsadmin setproxysettings
description: 适用于**bitsadmin setproxysettings**的 Windows 命令主题-设置指定作业的代理设置。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 38987c88bcfc93ea9251583b7914f982cf79e057
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380466"
---
# <a name="bitsadmin-setproxysettings"></a>bitsadmin setproxysettings



设置指定作业的代理设置。

## <a name="syntax"></a>语法

```
bitsadmin /SetProxySettings <Job> <Usage> [List] [Bypass]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|用法|以下值之一：</br>-预配置-使用所有者的 Internet Explorer 默认值。</br>-NO_PROXY-不使用代理服务器。</br>-OVERRIDE-使用显式代理列表和绕过列表。 必须遵循代理和代理跳过列表。</br>-自动检测-自动检测代理设置。|
|List|当*Usage*参数设置为 OVERRIDE 时使用，其中包含要使用的代理服务器的逗号分隔列表。|
|开|当*Usage*参数设置为 OVERRIDE 时使用，其中包含以空格分隔的主机名或 IP 地址的列表，或者两者都不是通过代理路由传输的。 这可以 **\<local >** ，以指代同一 LAN 上的所有服务器。 NULL 或 "" 的值可以用于空代理绕过列表。|

## <a name="BKMK_examples"></a>示例

下面的示例设置名为*myDownloadJob*的作业的代理设置。

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

[命令行语法项](command-line-syntax-key.md)
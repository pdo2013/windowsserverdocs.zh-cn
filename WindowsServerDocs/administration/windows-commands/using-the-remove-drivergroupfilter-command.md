---
title: 使用删除 DriverGroupFilter 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 837bd5d4-c79d-4714-942d-9875bd8e61dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a546ead7220273955368c582ac1e3f9b3f61c191
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883428"
---
# <a name="using-the-remove-drivergroupfilter-command"></a>使用删除 DriverGroupFilter 命令



从服务器上的驱动程序组中删除筛选器规则。

## <a name="syntax"></a>语法

```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/ DriverGroup:\<组名称 >|指定的驱动程序组的名称。|
|[/ 服务器：\<服务器名称 >]|指定的服务器的名称。 这可以是 NetBIOS 名称或 FQDN。 如果未指定服务器名称，则使用本地服务器。|
|[/FilterType:\<FilterType>]|指定要从组中删除的筛选器的类型。 \<筛选器类型 > 可以是以下之一：</br>**BiosVendor**</br>**BiosVersion**</br>**ChassisType**</br>**Manufacturer**</br>**uuid**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**|

## <a name="BKMK_examples"></a>示例

若要删除筛选器，请键入以下项之一：
```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer
```
```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /FilterType:OSLanguage
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
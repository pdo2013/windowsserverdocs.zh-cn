---
title: 子命令集 DriverGroupFilter
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 829ab1f0-4514-421e-9cc0-767b238da69c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 339beda0d6e92c7632cb16566c8db7be5f1079af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835058"
---
# <a name="subcommand-set-drivergroupfilter"></a>Subcommand: set-DriverGroupFilter



添加或删除现有的驱动程序组筛选器驱动程序组中。

## <a name="syntax"></a>语法

```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> [/Policy:{Include | Exclude}] [/AddValue:<Value> [/AddValue:<Value> ...]] [/RemoveValue:<Value> [/RemoveValue:<Value> ...]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/ DriverGroup:\<组名称 >|指定的驱动程序组的名称。|
|[/ 服务器：\<服务器名称 >]|指定的服务器的名称。 这可以是 NetBIOS 名称或 FQDN。 如果未指定服务器名称，则使用本地服务器。|
|/FilterType:\<FilterType>|指定要添加或删除的驱动程序组筛选器的类型。 可以在单个命令中指定多个筛选器。 每个 **/FilterType**，可以添加或删除多个值使用 **/RemoveValue**并 **/AddValue**。 \<筛选器类型 > 可以是以下之一：</br>**BiosVendor**</br>**BiosVersion**</br>**ChassisType**</br>**Manufacturer**</br>**uuid**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**|
|[/ 策略: {包括 | Exclude}]|指定要设置筛选器的新策略。 如果 **/Policy**设置为**Include**，允许客户端计算机的筛选器匹配此组中安装的驱动程序。 如果 **/Policy**设置为**排除**，则客户端计算机符合筛选器不允许此组中安装的驱动程序。|
|[/ Addvalue:\<值 >]|指定要添加到筛选器的新客户端值。 可以指定单个筛选器类型的多个值。 请参阅以下的有效特性值列表**ChassisType**。 有关获取的值对于所有其他筛选器类型的信息，请参阅[驱动程序组筛选器](https://go.microsoft.com/fwlink/?LinkID=155158)(https://go.microsoft.com/fwlink/?LinkID=155158)。</br>**其他**</br>**UnknownChassis**</br>**桌面**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**塔**</br>**可移植**</br>**Laptop**</br>**笔记本**</br>**手持设备**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**SubChassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca**|
|[/RemoveValue:\<Value>]|指定要从使用指定的筛选器中删除的现有客户端值 **/AddValue**。|

## <a name="BKMK_examples"></a>示例

若要删除筛选器，请键入以下项之一：
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /AddValue:Name1 /RemoveValue:Name2
```
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /RemoveValue:Name1 /FilterType:ChassisType /Policy:Exclude /AddValue:Tower /AddValue:MiniTower
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
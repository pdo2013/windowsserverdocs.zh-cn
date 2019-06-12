---
title: 使用添加 DriverGroupFilter 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a66c5e68-99ea-4e47-b68d-8109633ae336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45525c01a6f2cdfbfd17000368356dda72ed29bc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440740"
---
# <a name="using-the-add-drivergroupfilter-command"></a>使用添加 DriverGroupFilter 命令



将筛选器添加到服务器上的驱动程序组。

## <a name="syntax"></a>语法

```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]
```

## <a name="parameters"></a>Parameters

|         参数          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| / DriverGroup:\<组名称 > |                                                                                                                                                                                                                                                                                                                                                                                                                                                              指定的驱动程序组的名称。                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|  [/ 服务器：\<服务器名称 >]  |                                                                                                                                                                                                                                                                                                                                                                                                               指定的服务器的名称。 这可以是 NetBIOS 名称或 FQDN。 如果指定没有服务器名称，则使用本地服务器。                                                                                                                                                                                                                                                                                                                                                                                                               |
| /FilterType:\<FilterType>  |                                                                                                                                                                                                   指定要添加到组的筛选器的类型。 可以在单个命令中指定多个筛选器类型。 每个筛选器类型必须后跟 **/Policy**并至少包含一个 **/值**。 \<筛选器类型 > 可以是**BiosVendor**， **BiosVersion**， **ChassisType**，**制造商**， **Uuid**， **OsVersion**， **OsEdition**，或**OsLanguage**。 有关获取的值对于所有其他筛选器类型的信息，请参阅[驱动程序组筛选器](https://go.microsoft.com/fwlink/?LinkID=155158)(<https://go.microsoft.com/fwlink/?LinkID=155158>)。                                                                                                                                                                                                    |
|     [/ 策略: {包括      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             Exclude}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|     [/ 值：\<值 >]      | 指定客户端值对应于 **/FilterType**。 可以指定多个单精度类型值。 请参阅以下列表了解的有效值**ChassisType**。 有关获取的值对于所有其他筛选器类型的信息，请参阅[驱动程序组筛选器](https://go.microsoft.com/fwlink/?LinkID=155158)(<https://go.microsoft.com/fwlink/?LinkID=155158>)。</br>**其他**</br>**UnknownChassis**</br>**桌面**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**塔**</br>**可移植**</br>**Laptop**</br>**笔记本**</br>**手持设备**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**SubChassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |

## <a name="BKMK_examples"></a>示例

若要将筛选器添加到驱动程序组中，键入以下项之一：
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /Value:Name2
```
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /FilterType:ChassisType /Policy:Exclude /Value:Tower /Value:MiniTower
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)


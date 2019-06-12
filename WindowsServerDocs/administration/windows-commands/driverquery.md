---
title: driverquery
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 92ca4b84-e4e2-405b-9f31-bf6db9f66839
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88a59f9da9927bb923418695bc760303c0fb00b0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439485"
---
# <a name="driverquery"></a>driverquery



使管理员能够显示已安装的设备驱动程序及其属性的列表。 如果使用不带参数， **driverquery**在本地计算机上运行。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
driverquery [/s <System> [/u [<Domain>\]<Username> [/p <Password>]]] [/fo {table | list | csv}] [/nh] [/v | /si]
```

## <a name="parameters"></a>Parameters

|         参数         |                                                                                                                                         描述                                                                                                                                          |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s\<系统 >        |                                                                                      指定的名称或远程计算机的 IP 地址。 不要使用反斜杠。 默认值为本地计算机。                                                                                       |
| /u [\<域 >\]<Username> | 使用由指定的用户帐户的凭据运行命令*用户*或*域*\*用户<em>。默认情况下\* \*/s</em> \*使用当前登录到发出命令的计算机的用户的凭据。 **/u**无法使用，除非 **/s**指定。 |
|      /p \<Password>       |                                                                           指定在指定的用户帐户的密码 **/u**参数。 **/ p**无法使用，除非 **/u**指定。                                                                            |
|        /fo {table         |                                                                                                                                             列表                                                                                                                                             |
|            /nh            |                                                                                      省略中显示的驱动程序信息的标头行。 如果不是有效 **/fo**参数设置为**列表**。                                                                                      |
|            /v             |                                                                                                               显示详细输出。 **/v**不能用于签名的驱动程序。                                                                                                               |
|            /si            |                                                                                                                          提供有关签名的驱动程序信息。                                                                                                                          |
|            /?             |                                                                                                                             在命令提示符下显示帮助。                                                                                                                             |

## <a name="BKMK_examples"></a>示例

若要显示在本地计算机上已安装的设备驱动程序的列表，请键入：
```
driverquery 
```
若要以逗号分隔值 (CSV) 格式显示输出，请键入：
```
driverquery /fo csv 
```
若要隐藏在输出中的标头行，请键入：
```
driverquery /nh 
```
若要使用**driverquery**命令在名为的远程服务器上**server1**在本地计算机上使用你当前的凭据，请键入：
```
driverquery /s server1
```
若要使用**driverquery**命令在名为的远程服务器上**server1**使用的凭据**user1**域上**maindom**，类型：
```
driverquery /s server1 /u maindom\user1 /p p@ssw3d
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
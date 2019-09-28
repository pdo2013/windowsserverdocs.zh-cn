---
title: driverquery
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5d44a1be300b7178bc2271187344c2fc4ab8815e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377653"
---
# <a name="driverquery"></a>driverquery



使管理员能够显示已安装设备驱动程序及其属性的列表。 如果在没有参数的情况下使用，则**driverquery**将在本地计算机上运行。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
driverquery [/s <System> [/u [<Domain>\]<Username> [/p <Password>]]] [/fo {table | list | csv}] [/nh] [/v | /si]
```

## <a name="parameters"></a>Parameters

|         参数         |                                                                                                                                         描述                                                                                                                                          |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s \<System >        |                                                                                      指定远程计算机的名称或 IP 地址。 不要使用反斜杠。 默认值为本地计算机。                                                                                       |
| /u [@no__t > \] @ no__t-2 | 使用用户或*域*\*User @ no__t）*指定的用户*帐户的凭据运行该命令。默认情况下，\* @ no__t-1/s @ no__t-2 @ no__t 使用当前登录到发出命令的计算机的用户的凭据。 除非指定 **/s** ，否则不能使用 **/u** 。 |
|      /p \<密码 >       |                                                                           指定在 **/u**参数中指定的用户帐户的密码。 除非指定 **/u** ，否则不能使用 **/p** 。                                                                            |
|        /fo {table         |                                                                                                                                             list                                                                                                                                             |
|            /nh            |                                                                                      省略显示的驱动程序信息中的标题行。 如果 **/fo**参数设置为**list**，则无效。                                                                                      |
|            /v             |                                                                                                               显示详细输出。 **/v**对于签名的驱动程序无效。                                                                                                               |
|            /si            |                                                                                                                          提供有关签名驱动程序的信息。                                                                                                                          |
|            /?             |                                                                                                                             在命令提示符下显示帮助。                                                                                                                             |

## <a name="BKMK_examples"></a>示例

若要在本地计算机上显示已安装设备驱动程序的列表，请键入：
```
driverquery 
```
若要以逗号分隔值（CSV）格式显示输出，请键入：
```
driverquery /fo csv 
```
若要在输出中隐藏标题行，请键入：
```
driverquery /nh 
```
若要在本地计算机上使用当前凭据在名为**server1**的远程服务器上使用**driverquery**命令，请键入：
```
driverquery /s server1
```
若要在名为**server1**的远程服务器上使用域**maindom**上**user1**的凭据的**driverquery**命令，请键入：
```
driverquery /s server1 /u maindom\user1 /p p@ssw3d
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
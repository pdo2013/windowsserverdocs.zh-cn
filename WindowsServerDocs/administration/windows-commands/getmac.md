---
title: getmac
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a749a348-7cd1-4336-9f33-bb42dd0e31e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c770f5da5159e0037af479f90fadb4cd83464c77
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375806"
---
# <a name="getmac"></a>getmac

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

返回与每台计算机的所有网卡的每个地址相关联的媒体访问控制（MAC）地址和网络协议列表，不管是在本地还是在网络上。 
## <a name="syntax"></a>语法
```
getmac[.exe][/s <computer> [/u <Domain\<User> [/p <Password>]]][/fo {TABLE | list | CSV}][/nh][/v]
```
### <a name="parameters"></a>Parameters

|             参数              |                                                                                          描述                                                                                          |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s <computer>            |                                      指定远程计算机的名称或 IP 地址（不使用反斜杠）。 默认值为本地计算机。                                       |
|        /u <Domain> @ no__t-1 @ no__t-2         | 使用用户或 Domain\user 指定的用户的帐户权限运行命令。 默认为发出命令的计算机上当前登录用户的权限。 |
|           /p <Password>            |                                                     指定在 **/u**参数中指定的用户帐户的密码。                                                     |
| /fo {表&#124;列表&#124; CSV} |                       指定用于查询输出的格式。 有效值为**TABLE**、 **list**和**CSV**。 输出的默认格式为**TABLE**。                        |
|                /nh                 |                                             隐藏输出中的列标题。 当 **/fo**参数设置为**表**或**CSV**时有效。                                              |
|                 /v                 |                                                                    指定输出显示详细信息。                                                                     |
|                 /?                 |                                                                                                                                                                                               |

## <a name="remarks"></a>备注
当你希望将 MAC 地址输入网络分析器时，或者需要知道计算机中每个网络适配器当前正在使用的协议时， **getmac**可能非常有用。
## <a name="BKMK_Examples"></a>示例
下面的示例演示如何使用**getmac**命令：
```
getmac /fo table /nh /v
```
```
getmac /s srvmain
```
```
getmac /s srvmain /u maindom\hiropln
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo list /v
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo table /nh
```
## <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)

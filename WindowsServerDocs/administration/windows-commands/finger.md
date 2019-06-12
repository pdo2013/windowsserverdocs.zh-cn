---
title: finger
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 907ea637-5c6c-4752-84c2-46bbf2a68a33
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 526363db3ecff4a9138c9cf13cbf330196e14ced
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439250"
---
# <a name="finger"></a>finger

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

手指服务或后台程序正在运行的指定远程计算机 （通常运行 UNIX 的计算机） 上显示有关用户或用户的信息。 在远程计算机指定的格式和显示用户信息的输出。 使用不带参数，**手指**显示帮助。 
## <a name="syntax"></a>语法
```
finger [-l] [<User>] [@<Host>] [...]
```
### <a name="parameters"></a>Parameters

| 参数 |                                                                            描述                                                                            |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    -l     |                                                          以长列表格式显示用户信息。                                                           |
|  <User>   | 指定要信息的用户。 如果省略*用户*参数，**手指**显示指定的计算机上所有用户的相关信息。 |
|  @<Host>  |        指定运行手指服务正在寻找用户信息的远程计算机。 您可以指定计算机名称或 IP 地址。        |
|    /?     |                                                               在命令提示符下显示帮助。                                                                |

## <a name="remarks"></a>备注
多个User@Host，可以指定参数。
必须前缀**手指**连字符 （-），而不是个斜杠 （/） 的参数。
此命令是作为一个组件中的网络连接的网络适配器的属性在安装了 Internet 协议 (TCP/IP) 协议的情况下才可用。
Windows Server 2003 不提供手指服务。
## <a name="BKMK_Examples"></a>示例
若要显示计算机 users.microsoft.com user1 的信息，请键入：
```
finger user1@users.microsoft.com
```
若要在计算机 users.microsoft.com 上显示的所有用户的信息，请键入：
```
finger @users.microsoft.com
```
## <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)

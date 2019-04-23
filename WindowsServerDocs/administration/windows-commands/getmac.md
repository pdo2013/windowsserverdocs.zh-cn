---
title: getmac
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e356354e63a057201582db0fb74933e1b3ef8d8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851038"
---
# <a name="getmac"></a>getmac

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

返回的媒体访问控制 (MAC) 地址和网络协议与每个地址关联的每台计算机中的所有网络卡或者本地或网络中的列表。 
## <a name="syntax"></a>语法
```
getmac[.exe][/s <computer> [/u <Domain\<User> [/p <Password>]]][/fo {TABLE | list | CSV}][/nh][/v]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/s <computer>|指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。|
|/u <Domain>\\<User>|使用由用户或域 \ 用户指定的用户帐户权限运行该命令。 默认值为当前登录的用户发出命令的计算机上的权限。|
|/p <Password>|指定在指定的用户帐户的密码 **/u**参数。|
|/fo { TABLE &#124; list&#124; CSV}|指定要用于将查询输出的格式。 有效的值为**表**，**列表**，并**CSV**。 默认格式为输出**表**。|
|/nh|禁止显示在输出中的列标题。 时有效 **/fo**参数设置为**表**或**CSV**。|
|/v|指定输出显示详细的信息。|
|/?||
## <a name="remarks"></a>备注
**getmac**你想要的 MAC 地址输入到一个网络分析器，或当您需要知道什么协议是当前正在使用的计算机中每个网络适配器上时很有用。
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
-   [命令行语法解答](command-line-syntax-key.md)

---
title: rpcinfo
description: 了解如何列出在远程计算机上的程序。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c342232-a8f0-42ff-8f11-d18c4981f5ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4aba1e57d5a61103310fbe7abcac391e543be5aa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826368"
---
# <a name="rpcinfo"></a>rpcinfo

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

列出了在远程计算机上的程序。 **Rpcinfo**命令行实用程序都会发出远程过程调用 (RPC) 到 RPC 服务器，并报告它找到。 

## <a name="syntax"></a>语法
```
rpcinfo [/p [<Node>]] [/b <Program version>] [/t <Node Program> [<version>]] [/u <Node Program> [<version>]]
```

### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/p [\<Node>]|列出了所有程序注册到指定主机上的端口映射器。 如果不指定节点 （计算机） 名称，该程序将查询本地主机上的端口映射器。|
|b </b\<程序版本 >|从具有指定的程序和端口映射器中注册的版本的所有网络节点请求响应。 必须指定程序名称或编号和版本号。|
|/t\<节点程序 > [\<版本 >]|使用 TCP 传输协议来调用指定的程序。 您必须指定一个节点 （计算机） 名称和程序名称。 如果不指定版本，程序将调用所有版本。|
|/u\<节点程序 > [\<版本 >]|使用 UDP 传输协议来调用指定的程序。 您必须指定一个节点 （计算机） 名称和程序名称。 如果不指定版本，程序将调用所有版本。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_Examples"></a>示例
若要列出的端口映射器中注册的所有程序，请键入：
```
rpcinfo /p [<Node>]
```
若要从具有指定的程序的网络节点请求响应，请键入：
```
rpcinfo /b <Program version>
```
若要使用传输控制协议 (TCP) 调用程序，请键入：
```
rpcinfo /t <Node Program> [<version>]
```
使用用户数据报协议 (UDP) 调用程序：
```
rpcinfo /u <Node Program> [<version>]
```

## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)

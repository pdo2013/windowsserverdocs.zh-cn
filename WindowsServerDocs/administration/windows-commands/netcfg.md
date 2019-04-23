---
title: netcfg
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e2daaab7-12db-4e36-b70c-db8906d084f7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aed535f843da6d735526ea97c07f94564dc00dc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871318"
---
# <a name="netcfg"></a>netcfg

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

安装 Windows 预安装环境 (WinPE)，Windows 用来部署工作站的轻量版本。   
## <a name="syntax"></a>语法  
```  
netcfg [/v] [/e] [/winpe] [/l ] /c /i  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|/v|在 verbose （详细） 模式下运行|  
|/e|在安装过程中使用服务的环境变量和卸载|  
|/winpe|将 TCP/IP、 NetBIOS 和 Microsoft 客户端安装的 Windows 预安装环境|  
|/l|提供 INF 的位置|  
|/c|提供要安装; 的组件的类协议、 服务或客户端|  
|i|提供的组件 ID|  
|/s|提供要显示组件的类型<br /><br />\ta = 适配器的支持，n = net 组件|  
|/?|在命令提示符下显示帮助。|  
## <a name="BKMK_Examples"></a>示例  
若要安装协议*示例*使用 c:\oemdir\example.inf:  
```  
netcfg /l c:\oemdir\example.inf /c p /i example  
```  
若要安装*MS_Server*服务：  
```  
netcfg /c s /i MS_Server  
```  
若要安装适用于 Windows 预安装环境 TCP/IP、 NetBIOS 和 Microsoft 客户端  
```  
netcfg /v /winpe  
```  
若要显示如果组件*MS_IPX*安装：  
```  
netcfg /q MS_IPX  
```  
若要卸载组件*MS_IPX*:  
```  
netcfg /u MS_IPX  
```  
若要显示所有安装网络组件：  
```  
netcfg /s n  
```  
以显示绑定包含的路径*MS_TCPIP*:  
```  
netcfg /b ms_tcpip  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  

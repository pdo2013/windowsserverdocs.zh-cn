---
title: ipxroute
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3a30304f-655e-43d2-a4ac-7568abf8975c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d995204eea0af776a2084a82411fa95542d1d77a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889088"
---
# <a name="ipxroute"></a>ipxroute

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示和修改使用 IPX 协议的路由表有关的信息。 使用不带参数， **ipxroute**显示发送到未知、 广播和多播地址的数据包的默认设置。   
## <a name="syntax"></a>语法  
```  
ipxroute servers [/type=X]  
ipxroute ripout <Network>  
ipxroute resolve {guid | name} {GUID | <AdapterName>}  
ipxroute board= N [def] [gbr] [mbr] [remove=xxxxxxxxxxxx]  
ipxroute config  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|servers[ /type=X]|显示指定的服务器类型的服务访问点 (SAP) 表。  **X**必须是整数。 例如， **/类型 = 4**显示所有文件服务器。 如果未指定 **/type**， **ipxroute 服务器**显示所有类型的服务器，服务器名称按列出它们。|  
|ripout 网络|如果将发现*网络*不可通过咨询 IPX 堆栈的路由表并发出 rip 请求，如有必要。  *网络*是 IPX 网络段数量。|  
|resolve{ GUID&#124; name} { GUID&#124; AdapterName}|将 GUID 的名称解析为其友好名称或为其 GUID 的友好名称。|  
|板 = *N*|指定要为其查询或设置参数的网络适配器。|  
|def|将数据包发送到所有路由广播。 如果数据包传输到不在源路由表，唯一的媒体访问卡 (MAC) 地址**ipxroute**会将数据包发送到单个路由广播的默认值。|  
|gbr|将数据包发送到所有路由广播。 如果为广播地址 (FFFFFFFFFFFF) 传输数据包**ipxroute**会将数据包发送到单个路由广播的默认值。|  
|mbr|将数据包发送到所有路由广播。 如果为多播地址 (C000xxxxxxxx)，则传输数据包**ipxroute**会将数据包发送到单个路由广播的默认值。|  
|remove= *xxxxxxxxxxxx*|从源路由表中删除给定的节点地址。|  
|配置|显示所有已配置 IPX 的绑定信息。|  
|/?|在命令提示符下显示帮助。|  
## <a name="BKMK_Examples"></a>示例  
若要显示工作站附加到的网络段、 工作站节点地址和所使用的框架类型，请键入：  
```  
ipxroute config  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  

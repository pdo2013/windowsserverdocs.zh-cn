---
title: bootcfg
description: 用于**bootcfg**的 Windows 命令主题-配置、查询或更改 boot.ini 文件设置。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3deb354c-5717-4066-bc79-b9323d559e44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d66296327a2221093e5434f69e15e7c55df1f6b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379855"
---
# <a name="bootcfg"></a>bootcfg

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

配置、查询或更改 Boot.ini 文件设置。  
## <a name="syntax"></a>语法  
```  
bootcfg <parameter> [arguments...]  
```  
## <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|[bootcfg addsw](bootcfg-addsw.md)|为指定的操作系统项添加操作系统加载选项。|  
|[bootcfg copy](bootcfg-copy.md)|创建现有启动项的副本，您可以将命令行选项添加到该副本中。|  
|[bootcfg dbg1394](bootcfg-dbg1394.md)|为指定的操作系统项配置1394端口调试。|  
|[bootcfg debug](bootcfg-debug.md)|添加或更改指定操作系统项的调试设置。|  
|[bootcfg default](bootcfg-default.md)|指定要指定为默认的操作系统项。|  
|[bootcfg delete](bootcfg-delete.md)|删除 Boot.ini 文件的 **[操作系统]** 部分中的操作系统项。|  
|[bootcfg ems](bootcfg-ems.md)|允许用户添加或更改用于将紧急管理服务控制台重定向到远程计算机的设置。|  
|[bootcfg query](bootcfg-query.md)|查询并显示 Boot.ini 中的 [启动加载程序] 和 **[操作系统]** 部分条目。|  
|[bootcfg raw](bootcfg-raw.md)|将指定为字符串的操作系统加载选项添加到 Boot.ini 文件的 **[操作系统]** 部分中的操作系统项。|  
|[bootcfg rmsw](bootcfg-rmsw.md)|删除指定操作系统项的操作系统加载选项。|  
|[bootcfg timeout](bootcfg-timeout.md)|更改操作系统超时值。|  

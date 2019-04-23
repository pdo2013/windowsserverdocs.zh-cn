---
title: bootcfg
description: Windows 命令主题**bootcfg** -配置、 查询，或更改 Boot.ini 文件设置。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 79a1c0e22a3b162ba9492c80d114b2d5b943c744
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867018"
---
# <a name="bootcfg"></a>bootcfg

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

配置、查询或更改 Boot.ini 文件设置。  
## <a name="syntax"></a>语法  
```  
bootcfg <parameter> [arguments...]  
```  
## <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|[bootcfg addsw](bootcfg-addsw.md)|添加用于指定的操作系统条目的操作系统负载选项。|  
|[bootcfg copy](bootcfg-copy.md)|创建一个现有的启动项目，可向其中添加命令行选项的副本。|  
|[bootcfg dbg1394](bootcfg-dbg1394.md)|配置指定的操作系统项的 1394年端口调试。|  
|[bootcfg debug](bootcfg-debug.md)|添加或更改指定的操作系统条目的调试设置。|  
|[bootcfg default](bootcfg-default.md)|指定要将指定为默认值的操作系统条目。|  
|[bootcfg delete](bootcfg-delete.md)|删除中的操作系统条目 **[操作系统]** Boot.ini 文件部分。|  
|[bootcfg ems](bootcfg-ems.md)|使用户能够添加或更改远程计算机的紧急管理服务控制台重定向的设置。|  
|[bootcfg 查询](bootcfg-query.md)|查询并显示 [引导加载程序] 和 **[操作系统]** 部分从 Boot.ini 的条目。|  
|[bootcfg raw](bootcfg-raw.md)|添加操作系统加载选项指定为一个字符串中的操作系统条目 **[操作系统]** Boot.ini 文件部分。|  
|[bootcfg rmsw](bootcfg-rmsw.md)|删除指定的操作系统条目的操作系统加载选项。|  
|[bootcfg timeout](bootcfg-timeout.md)|更改操作系统超时值。|  

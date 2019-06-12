---
title: ntfrsutl
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7721a19-5a87-4ab6-b816-65d2da2c811f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e94d2b0a9ca764a6e8a25a087817598e3f158581
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436400"
---
# <a name="ntfrsutl"></a>ntfrsutl

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

内部表、 线程和 NT 文件复制服务的内存信息转储\(NTFRS\)。 对本地和远程服务器运行此脚本。 NTFRS 服务控制管理器中的恢复设置\(SCM\)可以查找并在计算机上保留重要的日志事件的关键。 此工具提供了一种简便的方法来查看这些设置。   
  
## <a name="syntax"></a>语法  
  
```  
ntfrsutl[idtable|configtable|inlog|outlog][<computer>]  
ntfrsutl[memory|threads|stage][<computer>]  
ntfrsutl ds[<computer>]  
ntfrsutl [sets][<computer>]  
ntfrsutl [version][<computer>]  
ntfrsutl poll[/quickly[=[<N>]]][/slowly[=[<N>]]][/now][<computer>]  
```  
  
### <a name="parameters"></a>Parameters  
  
|  参数  |                                                                                                                                                                                                                                                                                                                                        描述                                                                                                                                                                                                                                                                                                                                         |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   拓扑   |                                                                                                                                                                                                                                                                                                                                          ID 表                                                                                                                                                                                                                                                                                                                                          |
| configtable |                                                                                                                                                                                                                                                                                                                                  FRS 配置表                                                                                                                                                                                                                                                                                                                                   |
|    inlog    |                                                                                                                                                                                                                                                                                                                                        入站的日志                                                                                                                                                                                                                                                                                                                                         |
|   出    |                                                                                                                                                                                                                                                                                                                                        出站日志                                                                                                                                                                                                                                                                                                                                        |
| <computer>  |                                                                                                                                                                                                                                                                                                                                  指定的计算机。                                                                                                                                                                                                                                                                                                                                   |
|   memory    |                                                                                                                                                                                                                                                                                                                                        内存使用率                                                                                                                                                                                                                                                                                                                                        |
|   线程   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|    区域    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|     ds      |                                                                                                                                                                                                                                                                                                                         列出的 DS NTFRS 服务的视图。                                                                                                                                                                                                                                                                                                                          |
|    集     |                                                                                                                                                                                                                                                                                                                             指定的活动副本集                                                                                                                                                                                                                                                                                                                              |
|   version   |                                                                                                                                                                                                                                                                                                                       指定的 API 和 NTFRS 服务版本。                                                                                                                                                                                                                                                                                                                        |
|    轮询     | 指定当前的轮询间隔。<br /><br />参数：<br /><br /><ul><li>**\/快速**\[ **\=** \[ <N> \] \]\(快速轮询\)<br /><br /><ul><li>**快速**\-快速轮询，直到稳定的配置是 rectrieved</li><li>**快速\=** \-快速轮询每隔默认分钟。</li><li>**快速\=** <N> \-快速轮询每个*N*分钟</li></ul></li><li>**\/缓慢**\[ **\=** \[ <N> \] \] \(缓慢轮询\)<br /><br /><ul><li>**缓慢**\-缓慢轮询，直到检索到稳定的配置</li><li>**缓慢\=** \-缓慢轮询每隔默认分钟</li><li>**缓慢\=** <N> \-快速轮询每个*N*分钟</li></ul></li><li>**\/现在**\(现在轮询\)</li></ul> |
|     \/？     |                                                                                                                                                                                                                                                                                                                            在命令提示符下显示帮助。                                                                                                                                                                                                                                                                                                                            |
  
## <a name="BKMK_Examples"></a>示例  
若要确定文件复制的轮询间隔：  
  
```  
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1  
```  
  
若要确定当前 NTFRS 应用程序接口\(API\)版本：  
  
```  
C:\Program Files\SupportTools>ntfrsutl version  
```  
  
## <a name="additional-references"></a>其他参考  
  
-   [命令行语法项](command-line-syntax-key.md)  
  
  
  


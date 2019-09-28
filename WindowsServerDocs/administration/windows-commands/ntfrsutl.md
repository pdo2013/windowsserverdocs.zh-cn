---
title: ntfrsutl
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f1301b6876698e9eb552ae0ef9e70ed278319a7c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372620"
---
# <a name="ntfrsutl"></a>ntfrsutl

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

转储 NT 文件复制服务的内部表、线程和内存信息，\(NTFRS @ no__t-1。 它针对本地和远程服务器运行。 在服务控制管理器中，NTFRS 的恢复设置 \(SCM @ no__t-1 对于在计算机上查找和保存重要日志事件至关重要。 此工具提供了一种方便的方法来查看这些设置。   
  
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
|   idtable   |                                                                                                                                                                                                                                                                                                                                          ID 表                                                                                                                                                                                                                                                                                                                                          |
| configtable |                                                                                                                                                                                                                                                                                                                                  FRS 配置表                                                                                                                                                                                                                                                                                                                                   |
|    inlog    |                                                                                                                                                                                                                                                                                                                                        入站日志                                                                                                                                                                                                                                                                                                                                         |
|   outlog    |                                                                                                                                                                                                                                                                                                                                        出站日志                                                                                                                                                                                                                                                                                                                                        |
| <computer>  |                                                                                                                                                                                                                                                                                                                                  指定计算机。                                                                                                                                                                                                                                                                                                                                   |
|   memory    |                                                                                                                                                                                                                                                                                                                                        内存使用率                                                                                                                                                                                                                                                                                                                                        |
|   线程   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|    区域    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|     Ds      |                                                                                                                                                                                                                                                                                                                         列出了 NTFRS 服务的 DS 视图。                                                                                                                                                                                                                                                                                                                          |
|    卷集     |                                                                                                                                                                                                                                                                                                                             指定活动的副本集                                                                                                                                                                                                                                                                                                                              |
|   version   |                                                                                                                                                                                                                                                                                                                       指定 API 和 NTFRS 服务版本。                                                                                                                                                                                                                                                                                                                        |
|    检测     | 指定当前的轮询间隔。<br /><br />参数：<br /><br /><ul><li>**\/quickly**\[ **\=** \[ <N> @ no__t @-8 @ no__t-9Polls<br /><br /><ul><li>**快速**\- 轮询，直到稳定的配置 rectrieved</li><li>**快速 no__t-1** \- 按默认时间间隔快速轮询。</li><li>**快速 no__t-1**<N> @no__t 3 每*N*分钟快速轮询</li></ul></li><li>**\/slowly**\[ **\=** \[ <N> @ no__t no__t @-8 \(Polls 缓慢 @ no__t-10<br /><br /><ul><li>**缓慢**\- 轮询缓慢，直到检索到稳定的配置</li><li>**缓慢 @ no__t-1** \- 轮询每默认分钟缓慢</li><li>**缓慢 @ no__t-1**<N> @no__t 3 每*N*分钟快速轮询</li></ul></li><li>**\/now** \(Polls now @ no__t-3</li></ul> |
|     \/？     |                                                                                                                                                                                                                                                                                                                            在命令提示符下显示帮助。                                                                                                                                                                                                                                                                                                                            |
  
## <a name="BKMK_Examples"></a>示例  
若要确定文件复制的轮询间隔，请执行以下操作：  
  
```  
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1  
```  
  
若要确定当前的 NTFRS 应用程序接口 \(API @ no__t 版本：  
  
```  
C:\Program Files\SupportTools>ntfrsutl version  
```  
  
## <a name="additional-references"></a>其他参考  
  
-   [命令行语法项](command-line-syntax-key.md)  
  
  
  


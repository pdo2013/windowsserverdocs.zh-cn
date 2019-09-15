---
title: sxstrace
description: 了解如何诊断并列问题。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbc8dad642e15dede1ce89105a501fd90224610b
ms.sourcegitcommit: feec5cbe983c8c5800ccd4fc214914084fcceaba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2019
ms.locfileid: "70975305"
---
# <a name="sxstrace"></a>sxstrace

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

诊断并列问题。    

## <a name="syntax"></a>语法  
```  
sxstrace [{[trace -logfile:<FileName> [-nostop]|[parse -logfile:<FileName> -outfile:<ParsedFile>  [-filter:<AppName>]}]  
```  

### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|轨迹|为 sxs 启用跟踪（并行）|  
|-logfile|指定原始日志文件。|  
|\<文件名 >|将跟踪日志保存到*文件名*。|  
|-nostop|指定不提示停止跟踪。|  
|分析|转换原始跟踪文件。|  
|-outfile|指定输出文件名。|  
|\<ParsedFile >|指定分析的文件的文件名。|  
|-filter|允许筛选输出。|  
|\<AppName >|指定应用程序的名称。|  
|stoptrace|如果之前未停止跟踪，则停止跟踪。|  
|-?|在命令提示符下显示帮助。|  

## <a name="BKMK_Examples"></a>示例  
启用跟踪并将跟踪文件保存到**sxstrace**：  
```  
sxstrace trace -logfile:sxstrace.etl  
```  
将原始跟踪文件转换为可读的格式，并将结果保存到**sxstrace**：  
```  
sxstrace parse -logfile:sxstrace.etl -outfile:sxstrace.txt  
```  

## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  
  

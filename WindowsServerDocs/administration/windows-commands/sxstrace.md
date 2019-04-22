---
title: sxstrace
description: 了解如何诊断通过并行问题。
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
ms.openlocfilehash: 396d06bf079c0cfa8ba4864f71333eec39f7b255
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814098"
---
# <a name="sxstrace"></a>sxstrace

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

对通过并行问题进行诊断。    

## <a name="syntax"></a>语法  
```  
sxstrace [{[trace /logfile:<FileName> [/nostop]|[parse /logfile:<FileName> /outfile:<ParsedFile>  [/filter:<AppName>]}]  
```  

### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|跟踪|启用跟踪的 sxs （并行）|  
|/logfile|指定的原始日志文件。|  
|\<FileName>|将保存到跟踪日志*文件名*。|  
|/nostop|指定无提示停止跟踪。|  
|分析|将转换为原始跟踪文件。|  
|/outfile|指定输出文件名。|  
|\<ParsedFile>|指定已分析的文件的文件名。|  
|/filter|允许筛选输出。|  
|\<AppName>|指定应用程序的名称。|  
|stoptrace|如果它未停止之前，停止跟踪。|  
|/?|在命令提示符下显示帮助。|  

## <a name="BKMK_Examples"></a>示例  
启用跟踪并保存到跟踪文件**sxstrace.etl**:  
```  
sxstrace trace /logfile:sxstrace.etl  
```  
将原始跟踪文件转换为人工可读的格式并保存到结果**sxstrace.txt**:  
```  
sxstrace parse /logfile:sxstrace.etl /outfile:sxstrace.txt  
```  

## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  
  

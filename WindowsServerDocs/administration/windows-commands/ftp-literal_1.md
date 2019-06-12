---
title: ftp literal_1
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 693916507488a9a480315a8e9299baa93a223b8a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438666"
---
# <a name="ftp-literal1"></a>ftp: literal_1

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows Server 2012 R2，Windows Server 2012 将发送到远程 ftp 服务器的逐字字符串自变量。 返回单个 ftp 回复代码。   

## <a name="syntax"></a>语法  
```  
literal <Argument> [ ]  
```  
### <a name="parameters"></a>Parameters  

| 参数  |                    描述                    |
|------------|---------------------------------------------------|
| <Argument> | 指定要发送到 ftp 服务器的自变量。 |

## <a name="remarks"></a>备注  
**文字**命令等同于**报价**命令。  
## <a name="BKMK_Examples"></a>示例  
发送**退出**命令到远程 ftp 服务器。  
```  
literal quit  
```  
## <a name="additional-references"></a>其他参考  
-   [ftp: quote](ftp-quote.md)  
-   [命令行语法项](command-line-syntax-key.md)  

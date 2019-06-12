---
title: ftp 引号
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f468bfc384673818dc53be303f82cd4803cb2eb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438490"
---
# <a name="ftp-quote"></a>ftp： 引号

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将逐字字符串参数发送到远程 ftp 服务器。 返回单个 ftp 回复代码。   
## <a name="syntax"></a>语法  
```  
quote <Argument>[ ]  
```  
### <a name="parameters"></a>Parameters  

| 参数  |                    描述                    |
|------------|---------------------------------------------------|
| <Argument> | 指定要发送到 ftp 服务器的自变量。 |

## <a name="remarks"></a>备注  
**报价**命令等同于**文字**命令。  
## <a name="BKMK_Examples"></a>示例  
发送**退出**命令到远程 ftp 服务器。  
```  
quote quit  
```  
## <a name="additional-references"></a>其他参考  
-   [ftp: literal_1](ftp-literal_1.md)  
-   [命令行语法项](command-line-syntax-key.md)  

---
title: exit
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 23d1044b-f5c1-4180-ae6d-f553b48da4d9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e599f84389b23e527e3718a620d5fdfefe24edb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439460"
---
# <a name="exit"></a>exit

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

退出 Cmd.exe 程序 （命令解释器） 或当前的批处理脚本。  
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。  
## <a name="syntax"></a>语法  
```  
exit [/b] [<exitCode>]  
```  
## <a name="parameters"></a>Parameters  

| 参数  |                                                                                         描述                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /b     |                                      退出当前的批处理脚本，而不是退出 Cmd.exe。 如果通过执行一个批处理脚本外，将退出 Cmd.exe。                                      |
| <exitCode> | 指定一个数字。 如果**b </b** ERRORLEVEL 环境变量设置为该数字的指定。 如果您退出**Cmd.exe**，进程退出代码设置为此数字。 |
|     /?     |                                                                             在命令提示符下显示帮助。                                                                             |

## <a name="BKMK_examples"></a>示例  
若要关闭命令解释器，Cmd.exe，键入：  
```  
exit  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  


---
title: exit
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 105bf572c1ebeb37ea59ff8bc5c04121d2442341
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377347"
---
# <a name="exit"></a>exit

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

退出 Cmd.exe 程序（命令解释器）或当前批处理脚本。  
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。  
## <a name="syntax"></a>语法  
```  
exit [/b] [<exitCode>]  
```  
## <a name="parameters"></a>Parameters  

| 参数  |                                                                                         描述                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /b     |                                      退出当前的批处理脚本，而不是退出 Cmd.exe。 如果从批处理脚本外部执行，则退出 Cmd.exe。                                      |
| <exitCode> | 指定数值。 如果指定了 **/b** ，则 ERRORLEVEL 环境变量设置为该数字。 如果要退出**cmd.exe**，进程退出代码将设置为该数字。 |
|     /?     |                                                                             在命令提示符下显示帮助。                                                                             |

## <a name="BKMK_examples"></a>示例  
若要关闭命令解释器 Cmd.exe，请键入：  
```  
exit  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  


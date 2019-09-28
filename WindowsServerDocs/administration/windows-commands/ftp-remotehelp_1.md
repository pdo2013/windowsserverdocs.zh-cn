---
title: ftp remotehelp_1
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bac6fbe4a55c3fed4caab4e30ba848ec9ea68e21
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376031"
---
# <a name="ftp-remotehelp_1"></a>ftp： remotehelp_1

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示远程命令的帮助。   
## <a name="syntax"></a>语法  
```  
remotehelp [<Command>]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|[<Command>]|指定需要帮助的命令的名称。 如果未指定*命令*， **ftp**将显示所有远程命令的列表。|  
## <a name="remarks"></a>备注  
可以使用**引号**或**文本**运行远程命令。  
## <a name="BKMK_Examples"></a>示例  
显示远程命令的列表。  
```  
remotehelp  
```  
显示 "**特征**" 远程命令的语法。  
```  
remotehelp feat  
```  
## <a name="additional-references"></a>其他参考  
-   [ftp：引用](ftp-quote.md)  
-   [命令行语法项](command-line-syntax-key.md)  

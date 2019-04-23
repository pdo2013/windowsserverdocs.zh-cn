---
title: ftp remotehelp_1
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bd64af157f7ce05330cdafe6e4db6787fa765859
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889588"
---
# <a name="ftp-remotehelp1"></a>ftp: remotehelp_1

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示远程命令的帮助。   
## <a name="syntax"></a>语法  
```  
remotehelp [<Command>]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|[<Command>]|指定需要帮助的命令的名称。 如果*命令*未指定，则**ftp**显示所有远程命令的列表。|  
## <a name="remarks"></a>备注  
你可以运行远程命令使用**报价**或**文字**。  
## <a name="BKMK_Examples"></a>示例  
显示远程命令的列表。  
```  
remotehelp  
```  
显示的语法**功能**远程命令。  
```  
remotehelp feat  
```  
## <a name="additional-references"></a>其他参考  
-   [ftp: quote](ftp-quote.md)  
-   [命令行语法解答](command-line-syntax-key.md)  

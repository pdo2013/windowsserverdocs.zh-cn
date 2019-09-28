---
title: ftp lcd
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47058cc62e4e87d1fcd951ec3b4a7885eeef2ae2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376306"
---
# <a name="ftp-lcd"></a>ftp： lcd

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

更改本地计算机上的工作目录。 默认情况下，工作目录是启动**ftp**的目录。   
## <a name="syntax"></a>语法  
```  
lcd [<directory>]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|[<directory>]|指定要更改的本地计算机上的目录。 如果未指定*目录*，则将当前工作目录更改为默认目录。|  
## <a name="BKMK_Examples"></a>示例  
将本地计算机上的工作目录更改为**C:\dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  

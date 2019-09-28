---
title: ftp 目录
description: Ftp 目录的 Windows 命令主题
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a29a92a5-7b79-4e6e-95cf-2ccb38bb6fb2 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c47971c52135d79ce62f935bfed981f6eefcecaa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376440"
---
# <a name="ftp-dir"></a>ftp： dir

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示远程计算机上的目录文件和子目录的列表。   
## <a name="syntax"></a>语法  
```  
dir [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|[<remotedirectory>]|指定要查看其列表的目录。 如果未指定目录，则使用远程计算机上的当前工作目录。|  
|[<LocalFile>]|指定要在其中存储目录列表的本地文件。 如果未指定本地文件，则结果将显示在屏幕上。|  
## <a name="BKMK_Examples"></a>示例  
在远程计算机上显示**dir1**的目录列表。  
```  
dir dir1  
```  
将远程计算机上的当前目录的列表保存在本地文件**dirlist**中。  
```  
dir . dirlist.txt  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  

---
title: ftp ls_1
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d183f6a014273b78befd14c8d3208508948ffc54
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376249"
---
# <a name="ftp-ls_1"></a>ftp： ls_1

> 适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012
> 
> 
> 适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示远程计算机上的文件和子目录的缩写列表。   
## <a name="syntax"></a>语法  
```  
ls [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>Parameters  

|      参数      |                                                                       描述                                                                        |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<remotedirectory>] | 指定要查看其列表的目录。 如果未指定目录，则使用远程计算机上的当前工作目录。 |
|    [<LocalFile>]    |               指定要在其中存储列表的本地文件。 如果未指定本地文件，则结果将显示在屏幕上。               |

## <a name="BKMK_Examples"></a>示例  
显示远程计算机上的文件和子目录的简短列表。  
```  
ls  
```  
获取远程计算机上的**dir1**的缩略目录列表，并将其保存到名为**dirlist**的本地文件中  
```  
ls dir1 dirlist.txt   
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  

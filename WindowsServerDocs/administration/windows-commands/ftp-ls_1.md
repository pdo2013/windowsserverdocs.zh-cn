---
title: ftp ls_1
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c45e26f6578510837f190ae20e3140e619dc59cb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841758"
---
# <a name="ftp-ls1"></a>ftp: ls_1

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012


>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示文件和子目录的远程计算机中的缩写的列表。   
## <a name="syntax"></a>语法  
```  
ls [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|[<remotedirectory>]|指定你想要查看列表的目录。 如果未不指定任何目录，使用远程计算机上的当前工作目录。|  
|[<LocalFile>]|指定要在其中存储列表的本地文件。 如果未指定本地文件，结果显示在屏幕上。|  
## <a name="BKMK_Examples"></a>示例  
显示文件和子目录从远程计算机的简化的列表。  
```  
ls  
```  
获取的缩写的目录列表**dir1**远程计算机上并将其本地文件中保存调用**dirlist.txt**  
```  
ls dir1 dirlist.txt   
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  

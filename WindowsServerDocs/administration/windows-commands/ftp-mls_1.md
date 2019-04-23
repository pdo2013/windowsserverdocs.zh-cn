---
title: ftp mls_1
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6cf81018fa590d38e55778d60b0cb0e849ab83de
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835698"
---
# <a name="ftp-mls1"></a>ftp: mls_1

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

远程目录中显示文件和子目录的简短的的列表。   
## <a name="syntax"></a>语法  
```  
mls <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|<remoteFile>|指定你想要查看列表的文件。|  
|<LocalFile>|指定要在其中存储列表的本地文件。|  
## <a name="remarks"></a>备注  
-   指定*remoteFiles*  
    键入一个连字符 (**-**) 若要在远程计算机上使用当前工作目录。  
-   指定*本地文件*  
    键入一个连字符 (**-**) 以在屏幕上显示该列表。  
## <a name="BKMK_Examples"></a>示例  
显示的文件和子目录的简化的列表**dir1**并**dir2**。  
```  
mls dir1 dir2 -  
```  
保存的文件和子目录的简化的列表**dir1**并**dir2**本地文件中**dirlist.txt**  
```  
mls dir1 dir2 dirlist.txt   
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  

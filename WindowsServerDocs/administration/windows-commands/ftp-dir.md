---
title: ftp 目录
description: Ftp 目录的 Windows 命令主题
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 78ac8ac5e9fc4894f55401bb234aa98de981adf7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835248"
---
# <a name="ftp-dir"></a>ftp: dir

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在远程计算机上显示的文件目录和子目录的列表。   
## <a name="syntax"></a>语法  
```  
dir [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|[<remotedirectory>]|指定你想要查看列表的目录。 如果未不指定任何目录，使用远程计算机上的当前工作目录。|  
|[<LocalFile>]|指定要在其中存储在目录列表的本地文件。 如果未指定本地文件，结果显示在屏幕上。|  
## <a name="BKMK_Examples"></a>示例  
显示的目录列表**dir1**远程计算机上。  
```  
dir dir1  
```  
将当前目录的列表保存在本地文件中的远程计算机上**dirlist.txt**。  
```  
dir . dirlist.txt  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  

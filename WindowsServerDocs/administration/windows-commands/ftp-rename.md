---
title: ftp 重命名
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80d1a15f038017444c7654a44748bfd22be8e487
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438379"
---
# <a name="ftp-rename"></a>ftp： 重命名

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

重命名远程文件。   
## <a name="syntax"></a>语法  
```  
rename <FileName> <NewFileName>  
```  
### <a name="parameters"></a>Parameters  

|   参数   |                 描述                 |
|---------------|---------------------------------------------|
|  <FileName>   | 指定你想要重命名的文件。 |
| <NewFileName> |        指定新文件名。         |

## <a name="BKMK_Examples"></a>示例  
远程文件重命名**example.txt**到**example1.txt**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  

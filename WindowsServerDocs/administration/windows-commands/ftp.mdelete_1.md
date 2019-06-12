---
title: ftp mdelete_1
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a80a8f5-e880-40a8-abc9-29a41836844f vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b342f9d728e91085d5edf2f8e1ece00b48bec8d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438325"
---
# <a name="ftp-mdelete1"></a>ftp: mdelete_1

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

删除远程计算机上的文件。   
## <a name="syntax"></a>语法  
```  
mdelete <remoteFile>[ ]  
```  
### <a name="parameters"></a>Parameters  

|  参数   |             描述              |
|--------------|--------------------------------------|
| <remoteFile> | 指定要删除的远程文件。 |

## <a name="BKMK_Examples"></a>示例  
删除远程文件**a.exe**并**b.exe**。  
```  
mdelete a.exe b.exe  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  

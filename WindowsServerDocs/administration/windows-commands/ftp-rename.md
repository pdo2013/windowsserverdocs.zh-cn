---
title: ftp 重命名
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 977baa042a6b0d9c23db7cb398bee997c2049227
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376022"
---
# <a name="ftp-rename"></a>ftp：重命名

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

重命名远程文件。   
## <a name="syntax"></a>语法  
```  
rename <FileName> <NewFileName>  
```  
### <a name="parameters"></a>Parameters  

|   参数   |                 描述                 |
|---------------|---------------------------------------------|
|  <FileName>   | 指定要重命名的文件。 |
| <NewFileName> |        指定新的文件名。         |

## <a name="BKMK_Examples"></a>示例  
将远程文件**示例 .txt**重命名为**示例 1**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  

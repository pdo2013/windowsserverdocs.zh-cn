---
title: expand
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 66de0488-a0c4-40c2-9b03-e40c107ba343
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5df078af23c77f54ccb2da83b1057c5d7042593a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825958"
---
# <a name="expand"></a>expand

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

展开一个或多个压缩的文件。 此命令可用于从分发磁盘中提取压缩的文件。  
## <a name="syntax"></a>语法  
```  
expand [/r] <source> <destination>  
expand /r <source> [<destination>]  
expand /i <source> [<destination>]  
expand /d <source>.cab [/f:<files>]  
expand <source>.cab /f:<files> <destination>  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|/r|重命名展开的文件。|  
|源|指定要展开的文件。 *源*可以包含驱动器号和冒号、 目录名称、 文件名或这些项的组合。 你可以使用通配符 (**\*** 或 **？**)。|  
|destination|指定扩展的文件的位置。<br /><br />如果*源*多个文件组成，而且不指定 **/r**，*目标*必须是一个目录。<br /><br />*目标*可以包含驱动器号和冒号、 目录名称、 文件名或这些项的组合。<br /><br />目标文件&#124;指定的路径。|  
|i|重命名展开的文件，但会忽略的目录结构。<br /><br />此参数适用于：Windows Server 2008 R2 和 Windows 7。|  
|/d|在源位置中显示的文件的列表。 不展开或解压缩文件。|  
|/f:|在您想要扩展的 cabinet (.cab) 文件中指定的文件。 你可以使用通配符 (**\*** 或 **？**)。|  
|/?|在命令提示符下显示帮助。|  
## <a name="remarks"></a>备注  
-   使用**展开**在恢复控制台  
    **展开**命令，使用不同的参数，可从恢复控制台。 有关恢复控制台的详细信息，请参阅[一文 314058](https://support.microsoft.com/kb/314058) Microsoft 知识库中。  
## <a name="additional-references"></a>其他参考  
[命令行语法解答](command-line-syntax-key.md)  

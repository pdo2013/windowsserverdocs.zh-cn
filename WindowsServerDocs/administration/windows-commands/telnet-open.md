---
title: telnet 打开
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4528b728c89bbdfc99de94c7fefebb18c8e1ad97
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383655"
---
# <a name="telnet-open"></a>telnet：打开

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

连接到 telnet 服务器。    
## <a name="syntax"></a>语法  
```  
o[pen] <hostname> [<Port>]  
```  
### <a name="parameters"></a>Parameters  

| 参数  |                                        描述                                         |
|------------|--------------------------------------------------------------------------------------------|
| <hostname> |                         指定计算机名称或 IP 地址。                         |
|  [<Port>]  | 指定 telnet 服务器正在侦听的 TCP 端口。 默认值为 TCP 端口23。 |

## <a name="BKMK_Examples"></a>示例  
在 telnet.microsoft.com 连接到 telnet 服务器。  
```  
o telnet.microsoft.com  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  

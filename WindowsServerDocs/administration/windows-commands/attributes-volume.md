---
title: 属性数量
description: '**属性 "卷**" 的 Windows 命令主题显示、设置或清除卷的属性。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e40e8284-3d57-4de8-a46c-e4ade34a0d53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 225a10307123763d1a024fcc08fbae536fd0b5df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382581"
---
# <a name="attributes-volume"></a>属性数量

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示、设置或清除卷的属性。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|设置|设置具有焦点的卷的指定属性。|  
|clear|清除具有焦点的卷的指定属性。|  
|只读|指定该卷为 read @ no__t-0only。|  
|消隐|指定卷处于隐藏状态。|  
|nodefaultdriveletter|指定卷默认情况下不接收驱动器号。|  
|影|指定卷是卷影副本卷。|  
|noerr|仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。|  
  
## <a name="remarks"></a>备注  
  
-   在基本主启动记录 \(MBR @ no__t 磁盘上，**隐藏**、 **readonly**和**nodefaultdriveletter**参数适用于磁盘上的所有卷。  
  
-   在基本 GUID 分区表 \(gpt @ no__t-1 磁盘上，在动态 MBR 和 gpt 磁盘上，**隐藏**、**只读**和**nodefaultdriveletter**参数仅适用于所选卷。  
  
-   必须选择一个卷，才能使 "**属性" 卷**命令成功。 使用 "**选择音量**" 命令选择卷并将焦点移动到该卷。  
  
## <a name="BKMK_examples"></a>示例  
若要在所选卷上显示当前属性，请键入：  
  
```  
attributes volume  
```  
  
若要将所选卷设置为 hidden 和 read @ no__t-0only，请键入：  
  
```  
attributes volume set hidden readonly  
```  
  
若要删除所选卷上的隐藏和读取 @ no__t 属性，请键入：  
  
```  
attributes volume clear hidden readonly  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  


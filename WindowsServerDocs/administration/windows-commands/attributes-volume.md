---
title: 属性卷
description: Windows 命令主题**特性卷**-显示、 设置或清除卷的属性。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 37af55ee2a041fbcf8068e0def72147732d3a687
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846578"
---
# <a name="attributes-volume"></a>属性卷

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示、 设置，或清除卷的属性。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|设置|设置具有焦点的卷的指定的特性。|  
|clear|清除具有焦点的卷的指定的属性。|  
|只读|指定该卷读取\-仅。|  
|隐藏|指定卷为隐藏状态。|  
|nodefaultdriveletter|指定默认情况下，该卷不接收驱动器号。|  
|卷影副本|指定该卷是卷影副本卷。|  
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|  
  
## <a name="remarks"></a>备注  
  
-   在基本主启动记录\(MBR\)磁盘**隐藏**， **readonly**，并**nodefaultdriveletter**参数将应用于所有卷上磁盘。  
  
-   基本 GUID 分区表上\(gpt\)磁盘和动态 MBR 和 gpt 磁盘上**隐藏**， **readonly**，以及**nodefaultdriveletter**参数仅适用于所选的卷。  
  
-   必须选择一个卷**特性卷**命令才会成功。 使用**选择卷**命令以选择一个卷并将焦点移到它。  
  
## <a name="BKMK_examples"></a>示例  
若要显示所选卷上的当前属性，请键入：  
  
```  
attributes volume  
```  
  
若要将所选的卷设置为隐藏和读取\-仅，键入：  
  
```  
attributes volume set hidden readonly  
```  
  
若要删除的隐藏和读取\-类型仅在所选的卷上的属性：  
  
```  
attributes volume clear hidden readonly  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法解答](command-line-syntax-key.md)  
  

  


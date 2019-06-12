---
title: select volume
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5d70d776-80ad-4f20-8288-a7997fb1df28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 98f42324dbd4c6b3add3333cf4687d1613b1f700
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441423"
---
# <a name="select-volume"></a>select volume

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

选择指定的卷并将焦点转移给它。 此外可以使用此命令显示当前所选磁盘中具有焦点的卷。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
select volume={<n>|<d>}  
```  
  
## <a name="parameters"></a>Parameters  
  
| 参数 |                                                                               描述                                                                                |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <n>    | 要接收焦点的卷数。 你可以通过使用当前选择的磁盘上查看所有卷的号**列出卷**DiskPart 命令。 |
|    <d>    |                                                 要接收焦点的卷驱动器号或装入点路径。                                                 |
  
## <a name="remarks"></a>备注  
  
-   如果未指定卷，此命令显示当前所选磁盘中具有焦点的卷。  
  
-   在基本磁盘上，选择某个卷还会将焦点移到相应的分区。  
  
-   如果具有一个对应的分区选择卷，则将自动选择该分区。  
  
-   如果使用相应的卷选择分区，则将自动选择该卷。  
  
## <a name="BKMK_examples"></a>示例  
若要将焦点移到卷 2 中，键入：  
  
```  
select volume=2  
```  
  
若要将焦点移到驱动器 C 中，键入：  
  
```  
select volume=c  
```  
  
若要将焦点移到名为"mountpath"的文件夹上装入的卷，请键入：  
  
```  
select volume=c:\mountpath  
```  
  
若要显示当前所选磁盘中具有焦点的卷，请键入：  
  
```  
select volume  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  


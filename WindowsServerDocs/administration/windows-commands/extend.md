---
title: extend
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2414e21d-fc0b-40e8-9e33-3e072f8ad76b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84047c690006bf727bc12855576960bbf67d1617
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857928"
---
# <a name="extend"></a>extend

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

扩展到可用的卷或分区具有焦点和其文件系统\(未分配\)磁盘上的空间。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
extend [size=<n>] [disk=<n>] [noerr]  
extend filesystem [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|size\=<n>|指定以兆字节为单位的空间量\(MB\)将添加到当前卷或分区。 如果没有给定大小，则使用所有可用的磁盘的连续可用空间。|  
|disk\=<n>|指定在其扩展卷或分区的磁盘。 如果指定没有磁盘，则会在当前磁盘上扩展卷或分区。|  
|文件系统|扩展具有焦点的卷的文件系统。 仅在其中的文件系统已扩展与该卷的磁盘上使用。|  
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|  
  
## <a name="remarks"></a>备注  
  
-   基本磁盘上的可用空间必须位于同一个磁盘作为卷或带有焦点的分区上。 它必须还紧跟卷或分区具有焦点\(也就是说，它必须启动下一步的扇区偏移量位置处\)。  
  
-   使用简单或跨区卷的动态磁盘上卷可以扩展到任意动态磁盘上任何可用空间。 你可以使用此命令，将跨区动态卷转换简单动态卷。 镜像，RAID\-5 和带区卷不能进行扩展。  
  
-   如果以前已使用 NTFS 文件系统格式化分区，文件系统自动扩展以填充较大的分区，将会丢失任何数据。  
  
-   如果以前已使用非 NTFS 文件系统格式化分区，命令将失败并且不会更改到的分区。  
  
-   如果以前未使用文件系统格式化分区，将仍扩展分区。  
  
-   分区必须具有关联的卷，然后可以对其进行扩展。  
  
## <a name="BKMK_examples"></a>示例  
若要通过 500 兆字节，3，磁盘上扩展卷或带有焦点的分区键入：  
  
```  
extend size=500 disk=3  
```  
  
若要扩展的文件系统的卷已扩展后，请键入：  
  
```  
extend filesystem  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法解答](command-line-syntax-key.md)  
  

  


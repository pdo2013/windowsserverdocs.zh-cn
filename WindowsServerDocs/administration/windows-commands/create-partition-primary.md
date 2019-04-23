---
title: 创建主分区
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d652d8e-3935-4a91-8ced-b17c0e7937be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbbb4b89540b7264fe907f79ca003117429da08e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881708"
---
# <a name="create-partition-primary"></a>创建主分区

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

具有焦点的基本磁盘上创建主分区。  
  
> [!CAUTION]  
  
  
  
## <a name="syntax"></a>语法  
  
```  
create partition primary [size=<n>] [offset=<n>] [id={ <byte> | <guid> }] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|size\=<n>|指定分区的大小以兆字节为单位\(MB\)。 如果没有给定大小，则分区将继续直至没有更多的未分配空间的当前区域中。|  
|offset\=<n>|以千字节的偏移量\(KB\)，在创建分区。 如果没有给定偏移量，分区将从大到足以保留它的最大的磁盘范围内的开头。|  
|align\=<n>|对齐到最接近的对齐边界的所有分区扩展盘区。 通常与硬件 RAID 逻辑单元号一起使用\(LUN\)数组以提高性能。 <n> 是的千字节数\(KB\)从一开始为最接近的对齐边界的磁盘。|  
|id\={ <byte> &#124; <guid> }|指定分区类型。 此参数旨在用于原始设备制造商\(OEM\)仅使用。 可以使用此参数指定任何分区类型字节或 GUID。 DiskPart 不检查有效性除若要确保它是一个字节，采用 GUID 或十六进制形式的分区类型。 **警告：** 使用此参数创建分区可能会导致计算机故障或无法启动。 除非您是 OEM 或具有丰富 gpt 磁盘经验的 IT 专业人员，否则不要使用此参数的 gpt 磁盘上创建分区。 请始终使用**创建分区 efi**命令创建 EFI 系统分区**创建分区 msr**命令以创建 Microsoft 保留分区和**创建主分区**命令\(而无需**id\={字节&#124;guid}** 参数\)gpt 磁盘上创建主分区。<br /><br />**主启动记录磁盘**<br /><br />对于主启动记录\(MBR\)磁盘，以十六进制格式，为分区指定分区类型字节。 如果 MBR 磁盘为未指定此参数，该命令将创建类型 0x06:sp，指定未安装文件系统中的一个分区。 示例包括：<br /><br />-LDM 数据分区：0x42<br />-恢复分区：0x27<br />-已识别的 OEM 分区：0x12, 0x84, 0xDE, 0xFE, 0xA0<br /><br />**GUID 分区表磁盘**<br /><br />对于 GUID 分区表\(gpt\)磁盘，可以指定你想要创建的分区的分区类型 GUID。 已识别的 Guid 包括：<br /><br />-   EFI system partition: c12a7328\-f81f\-11d2\-ba4b\-00a0c93ec93b<br />-   Microsoft Reserved partition: e3c9e316\-0b5c\-4db8\-817d\-f92df00215ae<br />-基本数据分区： ebd0a0a2\-b9e5\-4433\-87 c 0\-68b6b72699c7<br />的动态磁盘上 LDM 元数据分区：5808c8aa\-7e8f\-42e0\-85d2\-e1e90434cfb3<br />的动态磁盘上 LDM 数据分区： af9b60a0\-1431年\-4f62\-bc68\-3311714a69ad<br />-恢复分区： de94bba4\-06 d 1\-4d 40\-a16a\-bfd50179d6ac<br /><br />如果没有为 gpt 磁盘指定此参数，该命令将创建一个基本数据分区。|  
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有 noerr 参数中，错误会导致 DiskPart 退出，错误代码。|  
  
## <a name="remarks"></a>备注  
  
-   创建分区后，焦点就自动转移到新分区。  
  
-   分区不会接收驱动器号。 必须使用**分配**驱动器号分配给分区的 DiskPart 命令。  
  
-   若要成功执行此操作，必须选择基本磁盘。 使用**选择的磁盘**命令选择基本磁盘，并将焦点移到它。  
  
## <a name="BKMK_examples"></a>示例  
若要创建 1000 兆字节为单位的主分区的大小，请键入：  
  
```  
create partition primary size=1000  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法解答](command-line-syntax-key.md)  
  

  


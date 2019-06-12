---
title: 创建分区 msr
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04fba033-23cb-4521-bd5d-db96131f2e73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3fa9ba46418c3ed3b7999a734b4c0df40dce5027
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434172"
---
# <a name="create-partition-msr"></a>创建分区 msr

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

创建 Microsoft 保留\(MSR\)上 GUID 分区表分区\(gpt\)磁盘。  
  
> [!CAUTION]  
> 要使用此命令时请格外小心。 因为 gpt 磁盘要求特定的分区布局，则创建 Microsoft 保留分区可能会导致磁盘不可读。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
create partition msr [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|  参数  |                                                                                                                         描述                                                                                                                         |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size\=<n>  |               以兆字节表示的分区的大小\(MB\)。 分区是不少于以字节为单位指定的数量作为<n>。 如果没有给定大小，则分区将继续，直到当前区域中没有更多可用空间为止。               |
| offset\=<n> | 千字节为单位指定的偏移量\(KB\)，在创建分区。 偏移量将向上舍入以完全填充使用任何扇区大小。 如果没有给定偏移量，该分区放置在大小足以容纳它的第一个磁盘区域。 |
|    noerr    |                            仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。                             |
  
## <a name="remarks"></a>备注  
  
-   用于启动 Windows 操作系统，可扩展固件接口的 gpt 磁盘上\(EFI\)系统分区是跟 Microsoft 保留分区在磁盘上的第一个分区。 仅用于数据存储的 gpt 磁盘没有 EFI 系统分区，在其中用例 Microsoft 保留分区是第一个分区。  
  
-   Windows 不装入 Microsoft 保留分区。 不能在其中存储数据并且不能删除它们。  
  
-   Microsoft 保留分区是所需的每个 gpt 磁盘。 此分区的大小取决于 gpt 磁盘的总大小。 Gpt 磁盘的大小必须至少 32 MB，以创建 Microsoft 保留分区。  
  
-   若要成功执行此操作，必须选择基本 gpt 磁盘。 使用**选择的磁盘**命令选择基本 gpt 磁盘，并将焦点移到它。  
  
## <a name="BKMK_examples"></a>示例  
若要创建 Microsoft 保留分区 1000 兆字节为单位的大小，请键入：  
  
```  
create partition msr size=1000  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  


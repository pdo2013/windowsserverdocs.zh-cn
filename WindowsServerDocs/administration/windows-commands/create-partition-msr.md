---
title: 创建分区 msr
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 45cc215b097ce048b15f0e907f95f976e4941e28
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378897"
---
# <a name="create-partition-msr"></a>创建分区 msr

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

在 GUID 分区表 \(gpt @ no__t-3 磁盘上创建 Microsoft 保留 @no__t 0MSR @ no__t 分区。  
  
> [!CAUTION]  
> 使用此命令时要非常小心。 因为 gpt 磁盘需要特定分区布局，所以创建 Microsoft 保留分区可能会导致磁盘不可读。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
create partition msr [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|  参数  |                                                                                                                         描述                                                                                                                         |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size @ no__t-0 @ no__t-1  |               分区的大小（mb） @no__t-配置包括 @ no__t-1。 分区的最大字节数至少为 <n> 指定的数字。 如果没有给定大小，则分区会一直继续，直至当前区域中没有可用空间为止。               |
| offset @ no__t-0 @ no__t-1 | 指定在其中创建分区的偏移量（kb \(KB @ no__t）。 偏移量向上舍入，以完全填充所使用的任何扇区大小。 如果未给出偏移量，则会将该分区置于足够大的第一个磁盘范围中以容纳该分区。 |
|    noerr    |                            仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。                             |
  
## <a name="remarks"></a>备注  
  
-   在用于启动 Windows 操作系统的 gpt 磁盘上，可扩展固件接口 \(EFI @ no__t 系统分区是磁盘上的第一个分区，后跟 Microsoft 保留分区。 仅用于数据存储的 gpt 磁盘没有 EFI 系统分区，在这种情况下，Microsoft 保留分区为第一个分区。  
  
-   Windows 不会装载 Microsoft 保留分区。 不能将数据存储在这些数据上，也不能将其删除。  
  
-   每个 gpt 磁盘上都需要 Microsoft 保留分区。 此分区的大小取决于 gpt 磁盘的总大小。 Gpt 磁盘的大小必须至少为 32 MB 才能创建 Microsoft 保留分区。  
  
-   若要成功执行此操作，必须选择一个基本 gpt 磁盘。 使用 "**选择磁盘**" 命令可选择基本 gpt 磁盘，并将焦点移动到该磁盘。  
  
## <a name="BKMK_examples"></a>示例  
若要创建大小为 1000 mb 的 Microsoft 保留分区，请键入：  
  
```  
create partition msr size=1000  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  


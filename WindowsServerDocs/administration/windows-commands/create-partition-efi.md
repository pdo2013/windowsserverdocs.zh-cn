---
title: 创建 efi 分区
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cfc1fca-6515-4a4d-bfae-615fa8045ea9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 99970fba41a747a6bb4b1ca6cc4b7f603c547790
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434160"
---
# <a name="create-partition-efi"></a>创建 efi 分区

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

基于 Itanium\-计算机，创建可扩展固件接口\(EFI\) GUID 分区表上的系统分区\(gpt\)磁盘。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
create partition efi [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
|  参数  |                                                                                             描述                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size\=<n>  |                         以兆字节表示的分区的大小\(MB\)。 如果没有给定大小，则分区将继续，直到当前区域中没有更多可用空间为止。                         |
| offset\=<n> |             以千字节的偏移量\(KB\)，在创建分区。 如果没有给定偏移量，该分区放置在大小足以容纳它的第一个磁盘区域。              |
|    noerr    | 仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。 |
  
## <a name="remarks"></a>备注  
  
-   创建分区后，将焦点提供给新分区。  
  
-   若要成功执行此操作，必须选择 gpt 磁盘。 使用**选择的磁盘**命令选择某一磁盘，并将焦点移到它。  
  
## <a name="BKMK_examples"></a>示例  
若要在所选磁盘上创建 EFI 分区的 1000 兆字节为单位，键入：  
  
```  
create partition efi size=1000  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  


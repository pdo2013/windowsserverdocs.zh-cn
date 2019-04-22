---
title: 选择分区
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 25f70083-b8f7-4a8e-9b34-4b3ffbe06670
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c7d5675aa6c33ddbe1e5e873e1a7cf7a2e8f8017
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824958"
---
# <a name="select-partition"></a>选择分区

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

选择指定的分区，并将焦点转移给它。 此外可以使用此命令显示当前所选磁盘中具有焦点的分区。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
select partition=<n>  
```  
  
## <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|partition\=<n>|要接收焦点的分区数。 可以通过使用当前选择的磁盘上查看的所有分区号**分区**DiskPart 命令。|  
  
## <a name="remarks"></a>备注  
  
-   可以选择一个分区之前，必须首先选择磁盘使用**选择的磁盘**命令。  
  
-   如果指定没有分区号，则此命令显示当前所选磁盘中具有焦点的分区。  
  
-   如果具有一个对应的分区选择卷，则将自动选择该分区。  
  
-   如果使用相应的卷选择分区，则将自动选择该卷。  
  
## <a name="BKMK_examples"></a>示例  
若要将焦点移到分区 3 中，键入：  
  
```  
select partitition=3  
```  
  
若要显示当前所选磁盘中具有焦点的分区，请键入：  
  
```  
select partition  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法解答](command-line-syntax-key.md)  
  

  


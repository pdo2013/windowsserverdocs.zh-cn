---
title: select disk
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a0da614b-09d9-433b-b4eb-9127f84431cb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d9078242264b01ee4bc24dc590df24b1e53e548
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371078"
---
# <a name="select-disk"></a>select disk

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

选择指定的磁盘并将焦点移动到该磁盘。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
select disk={ <n> | <disk path> | system | next }  
```  
  
> [!NOTE]  
> " **@No__t-1**"、"**系统**" 和 "**下一个**" 参数仅在 Windows 7 和 windows Server 2008 R2 中可用。  
  
## <a name="parameters"></a>Parameters  
  
|  参数  |                                                                                                                                                                                                            描述                                                                                                                                                                                                            |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     <n>     | 指定接收焦点的磁盘数。 可以通过使用 DiskPart 中**列出的磁盘**命令查看计算机上的所有磁盘的数字。 **注意：** 在配置包含多个磁盘的系统时，请不要使用**select disk @ no__t**来指定系统磁盘。 重新启动时，计算机可能会重新分配磁盘号，并且具有相同磁盘配置的不同计算机可以具有不同的磁盘号。 |
| <disk path> |                                                                                                                 指定接收焦点的磁盘的位置，例如， **PCIROOT @ no__t-10 @ no__t-2 @ no__t-3PCI @ no__t-40F02 @ no__t-5 @ no__t-6atA @ no__t-7C00T00L00 @ no__t-8**。 若要查看磁盘的位置路径，请选择该磁盘，然后键入**详细磁盘**。                                                                                                                  |
|   system    |                                 在 BIOS 计算机上，指定磁盘0接收焦点。 在 EFI 计算机上，包含 EFI 系统分区 \(ESP @ no__t-1 的磁盘将接收焦点。 在 EFI 计算机上，如果没有 ESP，则该命令将失败，如果有多个 ESP，或者计算机从 Windows 预安装环境启动 \(Windows PE @ no__t-1。                                  |
|    下一步     |                                                                                                                                     选择磁盘后，此命令会循环访问磁盘列表中的所有磁盘。 运行此命令时，列表中的下一个磁盘将接收焦点。                                                                                                                                      |
  
## <a name="BKMK_examples"></a>示例  
若要将焦点转移到磁盘1，请键入：  
  
```  
select disk=1  
```  
  
若要使用磁盘的位置路径选择磁盘，请键入：  
  
```  
select disk=PCIROOT(0)#PCI(0100)#atA(C00T00L01)  
```  
  
若要将焦点转移到系统磁盘，请键入：  
  
```  
select disk=system  
```  
  
若要将焦点转移到计算机上的下一个磁盘，请键入：  
  
```  
select disk=next  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  


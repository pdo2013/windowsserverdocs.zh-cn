---
title: select disk
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6e8352b45cf3a46f14828c9c6796e4ec73499d5a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858958"
---
# <a name="select-disk"></a>select disk

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

选择指定的磁盘，并将焦点转移给它。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
select disk={ <n> | <disk path> | system | next }  
```  
  
> [!NOTE]  
> **<disk path>**，**系统**，并且**下一步**参数只是在 Windows 7 和 Windows Server 2008 R2 中可用。  
  
## <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|<n>|指定要接收焦点的磁盘数。 通过查看计算机上的所有磁盘的数字**列表磁盘**DiskPart 命令。 **注意：** 当有多个磁盘配置系统，不要使用**选择的磁盘\=0**指定系统磁盘。 重新启动，并具有相同的磁盘配置不同的计算机可以具有不同的磁盘号时，计算机可能重新分配磁盘号。|  
|<disk path>|指定要接收焦点，例如，磁盘的位置**PCIROOT\(0\)\#PCI\(0F02\)\#atA\(C00T00L00\)** . 若要查看磁盘的位置路径，请选择它，然后键入**详细信息磁盘**。|  
|system|BIOS 在计算机上，指定该磁盘 0 接收焦点。 EFI 计算机，包含 EFI 系统分区的磁盘上\(ESP\)用于为当前启动接收焦点。 EFI 计算机上，如果没有任何 ESP，如果存在多个 ESP，或从 Windows 预安装环境中启动计算机时该命令将失败\(Windows PE\)。|  
|下一步|选择一个磁盘后，此命令将循环访问的磁盘列表中的所有磁盘。 当运行此命令时，列表中的下一步磁盘将接收焦点。|  
  
## <a name="BKMK_examples"></a>示例  
若要切换焦点到磁盘 1，请键入：  
  
```  
select disk=1  
```  
  
若要通过使用其位置路径中选择一个磁盘，请键入：  
  
```  
select disk=PCIROOT(0)#PCI(0100)#atA(C00T00L01)  
```  
  
若要将焦点移至系统磁盘，请键入：  
  
```  
select disk=system  
```  
  
若要在计算机上，将焦点移到下一个磁盘中，键入：  
  
```  
select disk=next  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法解答](command-line-syntax-key.md)  
  

  


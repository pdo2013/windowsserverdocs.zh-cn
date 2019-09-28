---
title: help
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75dbf94f-d79c-45b2-9463-c06648218f4a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c3d76a71ec287e5c874ae3e4dec34016c5c80336
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375578"
---
# <a name="help"></a>help

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示有关指定命令的可用命令或详细帮助信息的列表。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
help [<command>]  
```  
  
## <a name="parameters"></a>Parameters  
  
| 参数 |                              描述                              |
|-----------|-----------------------------------------------------------------------|
| <command> | 指定要显示其详细帮助信息的命令。 |
  
## <a name="remarks"></a>备注  
  
-   如果未指定命令，则 "**帮助**" 将显示所有可能的命令。  
  
## <a name="BKMK_examples"></a>示例  
若要显示 DiskPart 中可用的所有命令的列表，请键入：  
  
```  
help  
```  
  
若要显示有关如何使用 DiskPart 中的**create partition primary**命令的详细帮助信息，请键入：  
  
```  
help create partition primary  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  


---
title: Dfsdiag TestSites
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6486c79cfa58bb262fd3161ad0801e84185b6629
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873968"
---
# <a name="dfsdiag-testsites"></a>Dfsdiag TestSites

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

检查 active directory 域服务的配置\(AD DS\)通过验证在站点服务器，后者将作为命名空间服务器或文件夹\(链接\)目标上所有的域具有相同的站点关联控制器。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
### <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|\/计算机：<server name>|若要验证站点关联的服务器的名称。|  
|\/DFSpath:<namespace root or DFS folder>|命名空间根路径或分布式文件系统\(DFS\)文件夹\(链接\)包含要为其验证站点关联的目标。|  
|\/Recurse|枚举，并验证指定的命名空间根目录下的所有文件夹目标的站点关联。|  
|\/完整|验证 AD DS 和服务器的注册表包含相同的站点关联信息。|  
  
## <a name="BKMK_Examples"></a>示例  
若要 TBD，键入：  
  
```  
dfsdiag /TestSites /Machine:MyServer  
```  
  
若要 TBD，键入：  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace1\Folder1 /Full  
```  
  
若要 TBD，键入：  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace2 /Recurse /Full  
```  
  
## <a name="additional-references"></a>其他参考  
  
-   [命令行语法解答](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  


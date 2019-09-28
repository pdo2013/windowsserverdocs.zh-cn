---
title: dfsdiag TestSites
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: af72da64dd20d4b37824355a494cb8f97f597b28
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378387"
---
# <a name="dfsdiag-testsites"></a>dfsdiag TestSites

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

通过验证充当命名空间服务器或文件夹 \(link @ no__t 的服务器是否在所有域控制器上都具有相同的站点关联来检查 active directory 域服务 @no__t 0AD DS @ no__t 站点的配置。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
### <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|\/Machine： <server name>|要在其上验证站点关联的服务器的名称。|  
|\/DFSpath： <namespace root or DFS folder>|命名空间 root 或分布式文件系统 \(DFS @ no__t 文件夹 \(link @ no__t，其中包含要为其验证站点关联的目标。|  
|\/Recurse|枚举并验证指定命名空间根目录下的所有文件夹目标的站点关联。|  
|\/Full|验证 AD DS 和服务器的注册表中是否包含相同的站点关联信息。|  
  
## <a name="BKMK_Examples"></a>示例  
若要待定，请键入：  
  
```  
dfsdiag /TestSites /Machine:MyServer  
```  
  
若要待定，请键入：  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace1\Folder1 /Full  
```  
  
若要待定，请键入：  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace2 /Recurse /Full  
```  
  
## <a name="additional-references"></a>其他参考  
  
-   [命令行语法项](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  


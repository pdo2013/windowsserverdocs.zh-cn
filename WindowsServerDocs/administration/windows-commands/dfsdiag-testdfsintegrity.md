---
title: dfsdiag TestDFSIntegrity
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 173ee832-26e1-4ec8-a23a-38a7d6229ac3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7f344e2d1fecc542efc39688f20165fd3e39a04a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378426"
---
# <a name="dfsdiag-testdfsintegrity"></a>dfsdiag TestDFSIntegrity

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

通过执行以下测试来检查分布式文件系统 \(DFS @ no__t 命名空间的完整性：  
  
-   检查 DFS 元数据是否已损坏或域控制器之间存在不一致。  
  
-   验证 access @ no__t-0based 枚举的配置，以确保 DFS 元数据和命名空间服务器共享之间的一致性。  
  
-   检测与重叠文件夹目标 @no__t 的重叠 DFS 文件夹-0links @ no__t、重复文件夹和文件夹。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full]  
```  
  
### <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|\/DFSRoot： <DFS root path>|要诊断的 DFS 命名空间。|  
|\/Recurse|执行包括命名空间 interlinks 的测试。|  
|\/Full|验证共享和 NTFS Acl 的一致性以及所有文件夹目标上的客户端配置。 它还会验证是否已设置联机属性。|  
  
## <a name="BKMK_Examples"></a>示例  
若要待定，请键入：  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full  
```  
  
## <a name="additional-references"></a>其他参考  
  
-   [命令行语法项](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  


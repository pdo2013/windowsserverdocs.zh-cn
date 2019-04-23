---
title: Dfsdiag TestDFSIntegrity
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9a79e034f7c60be89266eb29dcd69e8f73b2aafe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837088"
---
# <a name="dfsdiag-testdfsintegrity"></a>Dfsdiag TestDFSIntegrity

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

检查，分布式文件系统的完整性\(DFS\)命名空间通过执行以下测试：  
  
-   检查 DFS 元数据损坏或域控制器之间的不一致。  
  
-   将验证的访问权限配置\-基于枚举，以确保它是 DFS 元数据和命名空间服务器共享之间保持一致。  
  
-   检测到重叠的 DFS 文件夹\(链接\)，重复文件夹和包含相互重叠的文件夹目标的文件夹。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full]  
```  
  
### <a name="parameters"></a>Parameters  
  
|参数|描述|  
|-------|--------|  
|\/DFSRoot:<DFS root path>|要诊断的 DFS 命名空间。|  
|\/Recurse|执行测试包括命名空间内部链路。|  
|\/完整|验证共享和 NTFS Acl 和客户端端配置的全部文件夹目标的一致性。 它还验证设置的在线财产。|  
  
## <a name="BKMK_Examples"></a>示例  
若要 TBD，键入：  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full  
```  
  
## <a name="additional-references"></a>其他参考  
  
-   [命令行语法解答](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  


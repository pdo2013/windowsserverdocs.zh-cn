---
title: Dfsdiag TestDFSConfig
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 106aeeb9-ea79-4e6e-829c-eca06309bab2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 922b78b87f3bb66765b87348a3bf136e14c9e837
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436131"
---
# <a name="dfsdiag-testdfsconfig"></a>Dfsdiag TestDFSConfig

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

检查配置分布式文件系统\(DFS\)命名空间通过执行以下操作：  
  
-   验证正在运行 DFS Namespace 服务和命名空间的所有服务器上其启动类型设置为自动。  
  
-   验证 DFS 注册表配置在命名空间服务器之间保持一致。  
  
-   验证在运行 Windows Server 2008 或更高版本的群集命名空间服务器上的以下依赖项：  
  
    -   Namespace 根资源依赖于网络名称资源。  
  
    -   网络名称资源依赖于 IP 地址资源。  
  
    -   Namespace 根资源依赖于物理磁盘资源。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:<namespace>  
```  
  
### <a name="parameters"></a>Parameters  
  
|       参数       |               描述               |
|-----------------------|-----------------------------------------|
| \/DFSRoot:<namespace> | 命名空间\(DFS 根目录\)诊断。 |
  
## <a name="BKMK_Examples"></a>示例  
若要 TBD，键入：  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:\\Contoso.com\MyNamespace  
```  
  
## <a name="additional-references"></a>其他参考  
  
-   [命令行语法项](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  


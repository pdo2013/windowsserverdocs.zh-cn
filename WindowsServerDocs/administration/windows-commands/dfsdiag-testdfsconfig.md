---
title: dfsdiag TestDFSConfig
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8008e02d588edaa6fe7700a331c43f9680d89431
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378417"
---
# <a name="dfsdiag-testdfsconfig"></a>dfsdiag TestDFSConfig

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

通过执行以下操作，检查分布式文件系统 \(DFS @ no__t 命名空间的配置：  
  
-   验证 DFS 命名空间服务是否正在运行，并且其启动类型在所有命名空间服务器上是否设置为自动。  
  
-   验证 DFS 注册表配置在命名空间服务器之间是否一致。  
  
-   验证运行 Windows Server 2008 或更高版本的群集命名空间服务器上的以下依赖项：  
  
    -   与网络名称资源的命名空间根资源相关。  
  
    -   IP 地址资源依赖于网络名称资源。  
  
    -   命名空间根资源依赖于物理磁盘资源。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:<namespace>  
```  
  
### <a name="parameters"></a>Parameters  
  
|       参数       |               描述               |
|-----------------------|-----------------------------------------|
| \/DFSRoot： <namespace> | 命名空间 \(DFS root @ no__t-1 进行诊断。 |
  
## <a name="BKMK_Examples"></a>示例  
若要待定，请键入：  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:\\Contoso.com\MyNamespace  
```  
  
## <a name="additional-references"></a>其他参考  
  
-   [命令行语法项](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  


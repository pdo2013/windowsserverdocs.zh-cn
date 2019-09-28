---
title: dfsdiag
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 61a6ab9a90e4d0220cfe27d2d21120be19b9ff1f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378308"
---
# <a name="dfsdiag"></a>dfsdiag



@No__t-0 命令提供 DFS 命名空间的诊断信息。

## <a name="syntax"></a>语法

```
dfsdiag [ /TestDCs [/Domain:<Domain name>]| /TestSites </Machine:<server name>| /DFSPath:<namespace root or DFS folder> [/Recurse]> [/Full] | /TestDFSConfig /DFSRoot:<namespace> | /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full] | /TestReferral /DFSPath:<DFS path for getting referrals> [/Full] | /?] 

```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[Dfsdiag TestDCs](dfsdiag-testdcs.md)|检查域控制器配置。|
|[Dfsdiag TestSites](dfsdiag-testsites.md)|检查站点关联。|
|[Dfsdiag TestDFSConfig](dfsdiag-testdfsconfig.md)|检查 DFS 命名空间配置。|
|[Dfsdiag TestDFSIntegrity](dfsdiag-testdfsintegrity.md)|检查 DFS 命名空间的完整性。|
|[Dfsdiag TestReferral](dfsdiag-testreferral.md)|检查引用响应。|
|/?|在命令提示符下显示帮助。|

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
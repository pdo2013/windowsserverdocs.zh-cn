---
title: dfsdiag
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ab5c86ce7ed4760aef4941de55e8dcf8efe48c8f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819138"
---
# <a name="dfsdiag"></a>dfsdiag



`Dfsdiag`命令提供的 DFS 命名空间的诊断信息。

## <a name="syntax"></a>语法

```
dfsdiag [ /TestDCs [/Domain:<Domain name>]| /TestSites </Machine:<server name>| /DFSPath:<namespace root or DFS folder> [/Recurse]> [/Full] | /TestDFSConfig /DFSRoot:<namespace> | /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full] | /TestReferral /DFSPath:<DFS path for getting referrals> [/Full] | /?] 

```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[Dfsdiag TestDCs](dfsdiag-testdcs.md)|检查域控制器配置。|
|[Dfsdiag TestSites](dfsdiag-testsites.md)|检查站点关联。|
|[Dfsdiag TestDFSConfig](dfsdiag-testdfsconfig.md)|检查 DFS Namespace 配置。|
|[Dfsdiag TestDFSIntegrity](dfsdiag-testdfsintegrity.md)|检查 DFS Namespace 完整性。|
|[Dfsdiag TestReferral](dfsdiag-testreferral.md)|检查引用响应。|
|/?|在命令提示符下显示帮助。|

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
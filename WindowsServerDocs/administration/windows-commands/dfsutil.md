---
title: dfsutil
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
robots: noindex,nofollow
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45545b4e12d31c293ead5b18b83efd50d7bc37bb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439648"
---
# <a name="dfsutil"></a>dfsutil

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Dfsutil 命令管理 DFS 命名空间、 服务器和客户端。 dfsutil 命令将原始分布式文件系统术语中，使用更新的大多数命令的说明作为提供的 DFS 命名空间术语。

有关如何使用此命令的示例，请参阅 

## <a name="syntax"></a>语法

```
command </parameter> </param2>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|[dfsutil Root](dfsutil-root.md)|显示、 创建、 删除、 导入、 导出命名空间的根。|
|[dfsutil Link](dfsutil-link.md)|显示、 创建、 删除或移动文件夹\(链接\)。|
|[dfsutil Target](dfsutil-target.md)|显示内容、 创建、 删除文件夹目标或命名空间服务器。|
|[dfsutil Property](dfsutil-property.md)|显示或修改文件夹目标或命名空间服务器。|
|[dfsutil Client](dfsutil-client.md)|显示或修改客户端的信息或注册表项。|
|[dfsutil Server](dfsutil-server.md)|显示或修改命名空间配置。|
|[dfsutil Diag](dfsutil-diag.md)|执行诊断，或查看 dfsdirs\/dfspath。|
|[dfsutil Domain](dfsutil-domain.md)|显示所有域\-基于某个域中的命名空间。|
|[dfsutil Cache](dfsutil-cache.md)|显示或刷新客户端缓存。|
|[dfsutil oldcli](dfsutil-oldcli.md)|使用 dfsutil \/oldcli 命令，以使用的原始 dfsutil 语法。|

## <a name="remarks-optional-section"></a>备注 <optional section>
如果指定的对象\(如命名空间服务器\)命令的末尾，大多数命令将无需进一步的参数或命令显示有关对象的信息。 例如时使用 dfsutil 根命令，你可以将命名空间根路径追加到命令以查看有关根的信息。

## <a name="BKMK_Examples"></a>示例
&lt;下面是示例的详细的说明的放置位置。&gt;

```
This /is /the /example /of /calling /command /with /parameters
```

&lt;下面是另一个示例的详细的说明的放置位置。&gt;

```
This /is /a:different /example
```

## <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)



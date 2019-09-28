---
title: dfsutil
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 1a06806b109bbd324213f935892bbbab415362df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377977"
---
# <a name="dfsutil"></a>dfsutil

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

Dfsutil 命令管理 DFS 命名空间、服务器和客户端。 dfsutil 命令使用原始分布式文件系统术语，其中包含已更新的 DFS 命名空间术语，作为对大多数命令的说明。

有关如何使用此命令的示例，请参阅 

## <a name="syntax"></a>语法

```
command </parameter> </param2>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|[dfsutil 根](dfsutil-root.md)|显示、创建、删除、导入、导出命名空间根目录。|
|[dfsutil 链接](dfsutil-link.md)|显示、创建、删除或移动文件夹 \(links @ no__t-1。|
|[dfsutil 目标](dfsutil-target.md)|显示、创建、删除文件夹目标或命名空间服务器。|
|[dfsutil 属性](dfsutil-property.md)|显示或修改文件夹目标或命名空间服务器。|
|[dfsutil 客户端](dfsutil-client.md)|显示或修改客户端信息或注册表项。|
|[dfsutil 服务器](dfsutil-server.md)|显示或修改命名空间配置。|
|[dfsutil 诊断](dfsutil-diag.md)|执行诊断或查看 dfsdirs @ no__t-0dfspath。|
|[dfsutil 域](dfsutil-domain.md)|显示域中的所有域 @ no__t-0based 命名空间。|
|[dfsutil 缓存](dfsutil-cache.md)|显示或刷新客户端缓存。|
|[dfsutil oldcli](dfsutil-oldcli.md)|使用 dfsutil @no__t 0oldcli 命令来使用原始 dfsutil 语法。|

## <a name="remarks-optional-section"></a>备注 <optional section>
如果在命令的末尾指定 \(such 作为命名空间服务器 @ no__t 的对象，则大多数命令将显示有关对象的信息，而无需进一步的参数或命令。 例如，使用 dfsutil 根命令时，可以将命名空间根追加到命令，以查看有关根的信息。

## <a name="BKMK_Examples"></a>示例
&lt;Here 是放置示例详细说明的位置。 &gt;

```
This /is /the /example /of /calling /command /with /parameters
```

&lt;Here 是对其他示例进行详细说明的位置。 &gt;

```
This /is /a:different /example
```

## <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)



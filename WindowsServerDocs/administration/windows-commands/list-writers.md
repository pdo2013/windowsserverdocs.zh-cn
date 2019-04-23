---
title: 列表编写器
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1c30cbc4-f568-4fa7-b564-66c41d3ca82d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fbab6644d46dbb352a5d5a51abefb293f3ffe6f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866738"
---
# <a name="list-writers"></a>列表编写器



列出系统上的编写器。 如果使用不带参数，**列表**显示的输出**列出的元数据**默认情况下。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
list writers [metadata | detailed | status]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|元数据|列出的标识和状态的编写器，并显示元数据，例如组件的详细信息和排除的文件。 这是默认参数。|
|详细的|列出了与相同的信息**元数据**，但**详细**包括所有组件的完整文件列表。|
|status|列出了标识和注册的编写器的状态。|

## <a name="BKMK_examples"></a>示例

若要列出的标识和编写器的状态，请键入：
```
list writers status
```
类似于以下显示的输出：
```
Listing writer status ...
* WRITER "System Writer"
        - Status: 5 (VSS_WS_WAITING_FOR_BACKUP_COMPLETE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {e8132975-6f93-4464-a53e-1050253ae220}
        - Instance ID: {7e631031-c695-4229-9da1-a7de057e64cb}
* WRITER "Shadow Copy Optimization Writer"
        - Status: 1 (VSS_WS_STABLE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
        - Instance ID: {9e362607-9794-4dd4-a7cd-b3d5de0aad20}
...
...
...
* WRITER "Registry Writer"
        - Status: 1 (VSS_WS_STABLE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {afbab4a2-367d-4d15-a586-71dbb18f8485}
        - Instance ID: {e87ba7e3-f8d8-42d8-b2ee-c76ae26b98e8}
8 writers listed. 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
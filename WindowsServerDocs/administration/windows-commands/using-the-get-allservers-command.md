---
title: 使用 AllServers 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8dd7f9917a54a80b3c570b07fe1a87bd3bcbe4d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363259"
---
# <a name="using-the-get-allservers-command"></a>使用 AllServers 命令



检索有关所有 Windows 部署服务服务器的信息。

> [!NOTE]
> 如果你的环境中有许多 Windows 部署服务服务器，或者如果链接服务器的网络连接速度较慢，则此命令可能需要很长时间才能完成。

## <a name="syntax"></a>语法

```
WDSUTIL [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
```

## <a name="parameters"></a>Parameters

|   参数   |                                                                                                                 描述                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Show： {Config |                                                                                                                    映像                                                                                                                    |
|  [/Detailed]  | 与 **/show： Images**或 **/show： All**结合使用时，将返回每个映像中的所有映像元数据。 如果未指定 **/Detailed**选项，则默认行为是返回映像名称、说明和文件名。 |
| [/Forest： {Yes |                                                                                                                     No}]                                                                                                                     |

## <a name="BKMK_examples"></a>示例

若要查看有关所有服务器的信息，请键入：
```
WDSUTIL /Get-AllServers /Show:Config
```
若要查看所有服务器的详细信息，请键入：
```
WDSUTIL /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
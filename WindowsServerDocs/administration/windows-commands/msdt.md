---
title: msdt
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7bec16ab3f716148bb009dd56be475fcd058a897
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868895"
---
# <a name="msdt"></a>msdt



在命令行或自动脚本中调用疑难解答包，并启用其他选项，无需用户输入。

## <a name="syntax"></a>语法

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

## <a name="parameters"></a>Parameters

下表包括了 msdt 支持的参数和选项。


|      参数      |                                                                                            描述                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /id \<包名称 > |        指定要运行的诊断包。 有关可用包的列表，请参阅本主题后面的 "可用疑难解答包" 一节中的疑难解答包 ID。         |
|  /path \<目录  |                                                                                           diagpkg 文件                                                                                            |
|   /dci \<密钥 >   |                                        预填充以 msdt 为密钥字段。 仅当支持提供程序提供了密钥时，才使用此参数。                                         |
|  /dt \<目录 >   | 显示指定目录中的故障排除历史记录。 诊断结果存储在用户的 **%LOCALAPPDATA%\Diagnostics**或 **%LOCALAPPDATA%\ElevatedDiagnostics**目录中。 |
| /af \<应答文件 >  |                                               指定 XML 格式的应答文件，该文件包含对一个或多个诊断交互的响应。                                               |


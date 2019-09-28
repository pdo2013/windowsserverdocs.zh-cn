---
title: nlbmgr
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2843e303b296beca24132b62073b6776a343544b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373158"
---
# <a name="nlbmgr"></a>nlbmgr

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

使用网络负载平衡管理器，你可以从一台计算机配置和管理网络负载平衡群集和所有群集主机，还可以将群集配置复制到其他主机。 您可以使用安装在**systemroot\System32**文件夹中的命令**nlbmgr**从命令行启动网络负载平衡管理器。
## <a name="syntax"></a>语法
```
nlbmgr [/help] [/noping] [/hostlist <filename>] [/autorefresh <interval>]
```
### <a name="parameters"></a>Parameters

|        参数        |                                                                                                                                                                                                描述                                                                                                                                                                                                |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /help          |                                                                                                                                                                                   在命令提示符下显示帮助。                                                                                                                                                                                    |
|         /noping         | 阻止网络负载平衡管理器在尝试通过 Windows Management Instrumentation （WMI）联系这些主机之前对其执行 ping 操作。 如果在所有可用的网络适配器上禁用了 Internet 控制消息协议（ICMP），请使用此选项。 如果网络负载平衡管理器尝试与不可用的主机联系，则使用此选项将会出现延迟。 |
|  /hostlist <filename>   |                                                                                                                                                                将 filename 中指定的主机加载到网络负载平衡管理器中。                                                                                                                                                                 |
| /autorefresh <interval> |                                                                                                          导致网络负载平衡管理器每 <interval> 秒刷新其主机和群集信息。 如果未指定间隔，则信息每60秒刷新一次。                                                                                                          |
|           /?            |                                                                                                                                                                                   在命令提示符下显示帮助。                                                                                                                                                                                    |

## <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)


---
title: fondue
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc4467f6-ddbb-4d6d-b51e-5a50a957b8c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d75d2d9fb57f8888cfc5bf50e2f7796aefc66102
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377086"
---
# <a name="fondue"></a>fondue

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

通过从 Windows 更新或组策略指定的其他源下载所需的文件来启用 Windows 可选功能。 此功能的清单文件必须已安装在 Windows 映像中。 
## <a name="syntax"></a>语法
```
fondue.exe /enable-feature:<feature_name> [/caller-name:<program_name>] [/hide-ux:{all | rebootRequest}]
```
### <a name="parameters"></a>Parameters

|              参数              |                                                                                                                                                                     描述                                                                                                                                                                     |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /enable-feature： <*feature_name*>   |                                                                               指定要启用的 Windows 可选功能的名称。 每个命令行只能启用一项功能。 若要启用多个功能，请使用每个功能的 fondue。                                                                                |
|    /caller-name： <*program_name*>    |                                                                                 从脚本或批处理文件中调用 fondue 时，指定程序或进程的名称。 如果出现错误，则可以使用此选项将程序名称添加到 SQM 报表中。                                                                                 |
| /hide-ux： {all &#124; rebootRequest} | 使用 "**全部**" 可向用户隐藏所有消息，包括访问 Windows 更新的进度和权限请求。 如果权限是必需的，则操作将失败。<br /><br />使用**rebootRequest**仅隐藏要求重新启动计算机的权限的用户消息。 如果你有控制重新启动请求的脚本，请使用此选项。 |

## <a name="BKMK_Examples"></a>示例
若要启用 Microsoft .NET Framework 3.5，请键入：
```
fondue.exe /enable-feature:NETFX3
```
若要启用 Microsoft .NET Framework 3.5，请将程序名称添加到 SQM 报表，而不向用户显示消息，请键入：
```
fondue.exe /enable-feature:NETFX3 /caller-name:Admin.bat /hide-ux:all
```
## <a name="additional-references"></a>其他参考
- [命令行语法项](command-line-syntax-key.md)
  ## <a name="see-also"></a>请参阅
  [Microsoft .NET Framework 3.5 部署注意事项](https://go.microsoft.com/fwlink/?LinkId=248869)

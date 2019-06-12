---
title: fondue
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bcbbbf80f25f77d1feb83f358401e4d14da3d354
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439221"
---
# <a name="fondue"></a>fondue

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

通过从 Windows 更新或另一个组策略所指定的源下载所需的文件来启用 Windows 可选功能。 在 Windows 映像中必须已安装功能的清单文件。 
## <a name="syntax"></a>语法
```
fondue.exe /enable-feature:<feature_name> [/caller-name:<program_name>] [/hide-ux:{all | rebootRequest}]
```
### <a name="parameters"></a>Parameters

|              参数              |                                                                                                                                                                     描述                                                                                                                                                                     |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /enable-feature: <*feature_name*>   |                                                                               指定你想要启用 Windows 可选功能的名称。 仅可以启用每个命令行的一个功能。 若要启用多个功能，请为每个功能使用 fondue.exe。                                                                                |
|    /caller-name: <*program_name*>    |                                                                                 从脚本或批处理文件调用 fondue.exe 时指定的程序或进程名称。 此选项可用于将程序名称添加到 SQM 报告，如果出现错误。                                                                                 |
| /hide-ux:{all &#124; rebootRequest} | 使用**所有**隐藏给包括进度和权限请求，若要访问 Windows 更新用户的所有消息。 如果权限是必需的则操作将失败。<br /><br />使用**rebootRequest**仅隐藏用户消息询问权限重新启动计算机。 如果您有一个脚本控件重新启动请求，请使用此选项。 |

## <a name="BKMK_Examples"></a>示例
若要启用 Microsoft.NET Framework 3.5，请键入：
```
fondue.exe /enable-feature:NETFX3
```
若要启用 Microsoft.NET Framework 3.5，程序名称添加到 SQM 报告，并显示消息给用户，类型：
```
fondue.exe /enable-feature:NETFX3 /caller-name:Admin.bat /hide-ux:all
```
## <a name="additional-references"></a>其他参考
- [命令行语法项](command-line-syntax-key.md)
  ## <a name="see-also"></a>请参阅
  [Microsoft.NET Framework 3.5 部署注意事项](https://go.microsoft.com/fwlink/?LinkId=248869)

---
title: dcgpofix
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 81d5fa65-2aea-49d3-b353-357441846c00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2592210ae688f47dcf2d32c7bef560d52223141c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378758"
---
# <a name="dcgpofix"></a>dcgpofix



重新创建域的默认组策略对象（Gpo）。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
DCGPOFix [/ignoreschema] [/target: {Domain | DC | Both}] [/?]
```

### <a name="parameters"></a>Parameters

|    参数    |                                                                                                 描述                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /ignoreschema  | 忽略 Active Directory® schema mc 的版本</br>运行此命令时。 否则，该命令仅适用于在其中附带了命令的 Windows 版本的架构版本。 |
| /target {域 |                                                                                                     DC                                                                                                      |
|       /?        |                                                                                    在命令提示符下显示帮助。                                                                                     |

## <a name="remarks"></a>备注

-   除了服务器核心安装， **dcgpofix**命令在 windows Server 2008 R2 和 windows server 2008 中可用。
-   尽管组策略管理控制台（GPMC）随 Windows Server 2008 R2 和 Windows Server 2008 一起分发，但必须通过服务器管理器将组策略管理作为一项功能安装。

## <a name="BKMK_Examples"></a>示例

将默认域策略 GPO 还原到其原始状态。 你将丢失对此 GPO 所做的任何更改。 最佳做法是，应仅将默认域策略 GPO 配置为管理默认的帐户策略设置、密码策略、帐户锁定策略和 Kerberos 策略。 在此示例中，你将忽略 Active Directory 架构的版本，以便**dcgpofix**命令与用于发送命令的 Windows 版本的架构不同。
```
dcgpofix /ignoreschema /target:Domain
```
将默认域控制器策略 GPO 还原到其原始状态。 你将丢失对此 GPO 所做的任何更改。 最佳做法是，应仅将默认域控制器策略 GPO 配置为设置用户权限和审核策略。 在此示例中，你将忽略 Active Directory 架构的版本，以便**dcgpofix**命令与用于发送命令的 Windows 版本的架构不同。
```
dcgpofix /ignoreschema /target:DC
```

#### <a name="additional-references"></a>其他参考

-   [组策略技术中心](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [命令行语法项](command-line-syntax-key.md)
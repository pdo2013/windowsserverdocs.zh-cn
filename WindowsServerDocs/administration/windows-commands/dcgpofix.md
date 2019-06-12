---
title: dcgpofix
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 179d540371870075906bbcbf8ff912e1b883915d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433934"
---
# <a name="dcgpofix"></a>dcgpofix



重新创建为域默认组策略对象 (Gpo)。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
DCGPOFix [/ignoreschema] [/target: {Domain | DC | Both}] [/?]
```

### <a name="parameters"></a>Parameters

|    参数    |                                                                                                 描述                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /ignoreschema  | 将忽略 Active Directory® 架构 mc 的版本</br>当你运行此命令。 否则，该命令仅适用于与该命令产品附带的 Windows 版本相同的架构版本。 |
| /target {域 |                                                                                                     DC                                                                                                      |
|       /?        |                                                                                    在命令提示符下显示帮助。                                                                                     |

## <a name="remarks"></a>备注

-   **Dcgpofix**命令现已推出 Windows Server 2008 R2 和 Windows Server 2008 中，在服务器核心安装除外。
-   尽管组策略管理控制台 (GPMC) 随 Windows Server 2008 R2 和 Windows Server 2008 一起分发，你必须作为一项功能通过服务器管理器安装组策略管理。

## <a name="BKMK_Examples"></a>示例

将默认域策略 GPO 还原到其原始状态。 你将失去对此 GPO 所做的任何更改。 作为最佳做法，应配置仅用于管理默认帐户策略设置、 密码策略、 帐户锁定策略和 Kerberos 策略在默认域策略 GPO。 在此示例中，忽略 Active Directory 架构的版本，以便**dcgpofix**命令并不局限于该命令产品附带的 Windows 版本相同的架构。
```
dcgpofix /ignoreschema /target:Domain
```
将默认域控制器策略 GPO 还原到其原始状态。 你将失去对此 GPO 所做的任何更改。 作为最佳做法，应配置默认域控制器策略 GPO 仅用于设置用户权限和审核策略。 在此示例中，忽略 Active Directory 架构的版本，以便**dcgpofix**命令并不局限于该命令产品附带的 Windows 版本相同的架构。
```
dcgpofix /ignoreschema /target:DC
```

#### <a name="additional-references"></a>其他参考

-   [组策略技术中心](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [命令行语法项](command-line-syntax-key.md)
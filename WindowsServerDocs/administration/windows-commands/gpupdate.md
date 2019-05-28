---
title: gpupdate
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fd4e567-2ce1-4637-b611-c2f0895e5708
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d358c47bd278cf11c4bab6887302bf6d053529ec
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192664"
---
# <a name="gpupdate"></a>gpupdate



更新组策略设置。 有关如何使用此命令的示例，请参阅[示例](#examples)。

## <a name="syntax"></a>语法

```
gpupdate [/target:{Computer | User}] [/force] [/wait:<VALUE>] [/logoff] [/boot] [/sync] [/?]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/target:{Computer | 用户}|更新仅用户或仅计算机策略设置。|
|/force|重新应用所有策略设置。 默认情况下，仅已更改的策略设置应用。|
|/wait:\<值 >|设置策略处理，以返回到命令提示符前完成等待秒的数。 当超过时间限制时，命令提示符下显示，但继续进行策略处理。 默认值为 600 秒。 该值**0**是指不等待。 该值**为-1**意味着无限期等待。</br>在脚本中，通过使用此命令与时间限制指定，你可以运行**gpupdate**和继续命令完成后不依赖**gpupdate**。 或者，可以使用此命令指定以便无时间限制**gpupdate**完成运行之前运行依赖于它的其他命令。|
|/logoff|更新的组策略设置后，会导致注销。 这是这些组策略客户端的扩展，在后台更新周期不处理策略，但当用户下次登录时处理策略所必需的。 示例包括面向用户的软件安装和文件夹重定向。 如果有调用的扩展需要一个注销，则此选项无效。|
|/boot|应用组策略设置后，会导致计算机重新启动。 这是这些组策略客户端的扩展，在后台更新周期不处理策略，但处理策略在计算机启动时所必需的。 例如，为目标计算机的软件安装。 如果有调用的扩展需要重新启动，则此选项无效。|
|/sync|导致下一步前台策略应用程序以同步方式完成。 前台策略在计算机启动和用户登录时应用。 你可以为用户、 计算机或两者，通过使用指定这 **/target**参数。 **/Force**并 **/wait**参数将被忽略，如果你指定它们。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   **Gpupdate**命令现已推出 Windows Server 2008 R2、 Windows Server 2008、 Windows 7 旗舰版、 Windows 7 Professional、 Windows Vista Ultimate、 Windows Vista Enterprise 和 Windows Vista Business。

## <a name="examples"></a>示例

强制执行所有组策略设置，而不考虑它们是否发生更改的后台的更新。
```
gpupdate /force
```

#### <a name="additional-references"></a>其他参考

-   [组策略技术中心](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [命令行语法项](command-line-syntax-key.md)
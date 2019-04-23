---
title: cscript
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fba3cbca-594e-4663-bb22-4ee0f63a1ac6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ffdbd6f67e4e4c32022134191deabd861bf248b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827668"
---
# <a name="cscript"></a>cscript

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

启动一个脚本，以便在命令行环境中运行。
## <a name="syntax"></a>语法
```
cscript <Scriptname.extension> [/B] [/D] [/E:<Engine>] [{/H:cscript|/H:wscript}] [/I] [/Job:<Identifier>] [{/Logo|/NoLogo}] [/S] [/T:<Seconds>] [/X] [/U] [/?] [<ScriptArguments>]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|Scriptname.extension|可选文件扩展名与指定脚本文件的路径和文件的名称。|
|/B|指定批处理模式下，不会显示警报、 脚本错误或输入的提示。|
|/D|启动调试器。|
|/ E:<Engine>|指定用于运行脚本的引擎。|
|/H:cscript|注册为默认脚本宿主运行脚本的 cscript.exe。|
|/H:wscript|注册 wscript.exe 作为默认脚本宿主运行脚本。 这是默认设置。|
|/I|指定显示警报、 脚本错误和输入的提示的交互模式。 这是默认值和相反**b </B**。|
|/ 作业：<Identifier>|在运行作业由标识*标识符*.wsf 脚本文件中。|
|/ 徽标|指定运行脚本前，在控制台中显示 Windows Script Host 横幅。 这是默认值和相反 **/Nologo**。|
|/Nologo|指定运行脚本前不会显示 Windows Script Host 横幅。|
|/S|将保存当前用户的当前命令提示符选项。|
|/ T:<Seconds>|指定脚本可以运行 （以秒为单位） 的最大时间。 您可以指定 32,767 秒。 默认值为没有时间限制。|
|/U|指定用于从控制台重定向输入和输出的 Unicode。|
|/X|在调试器中启动该脚本。|
|/?|显示可用的命令参数并使用这些提供帮助。 这是与键入相同**cscript.exe**没有参数，没有任何脚本。|
|ScriptArguments|指定传递给脚本的参数。 每个脚本参数前面必须有一个斜杠 (**/**)。|
### <a name="remarks"></a>备注
-   执行此任务不要求具有管理凭据。 因此，作为安全方面的最佳做法，请考虑以不具有管理凭据的用户身份执行该任务。
-   若要打开命令提示符下，在**启动**屏幕上，键入**cmd**，然后单击**命令提示符下**。
-   每个参数是可选的;但是，您不能指定脚本参数，而无需指定一个脚本。 如果不指定的脚本或任何脚本参数，cscript.exe 显示 cscript.exe 语法和有效的主机选项。
-   **/T**参数可以设置一个计时器，从而防止过多正在运行的脚本。 当运行的时间超出了指定的值时，cscript 中断脚本引擎并结束该进程。
-   Windows 脚本文件通常具有以下文件扩展名之一：.wsf、.vbs、.js。
-   可以设置单个脚本的属性。 有关详细信息，请参阅相关主题。
-   Windows 脚本宿主可以使用.wsf 脚本文件。 每个.wsf 文件可以使用多个脚本引擎并执行多个作业。
-   如果您双击扩展名为没有关联的脚本文件**打开**对话框随即出现。 选择 wscript 或 cscript，，然后选择**始终使用该计划来打开此文件类型**。 这将 wscript.exe 或 cscript 注册为此文件类型的文件的默认脚本宿主。
-   可以设置单个脚本的属性。 请参阅[其他参考](#BKMK_references)有关详细信息。
-   Windows 脚本宿主可以使用.wsf 脚本文件。 每个.wsf 文件可以使用多个脚本引擎并执行多个作业。

#### <a name="BKMK_references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

---
title: wscript
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fbaf193-cdbd-414c-84c9-bb5720f84c29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 771c1231ee5379ec797f535505839de8671e32a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821298"
---
# <a name="wscript"></a>wscript



Windows 脚本宿主提供的用户可以在其中执行中使用的各种对象模型来执行任务的语言的各种脚本的环境。

## <a name="syntax"></a>语法

```
wscript [<scriptname>] [/b] [/d] [/e:<engine>] [{/h:cscript|/h:wscript}] [/i] [/job:<identifier>] [{/logo|/nologo}] [/s] [/t:<number>] [/x] [/?] [<ScriptArguments>]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|scriptname|指定脚本文件的路径和文件。|
|/b|指定批处理模式下，不会显示警报、 脚本错误或输入的提示。 这是相反 **/i**。|
|/d|启动调试器。|
|/e|指定用于运行脚本的引擎。 这样可以运行使用自定义的文件扩展名的脚本。 如果没有 /e 参数中，只能运行使用已注册的文件扩展名的脚本。 例如，如果你尝试运行以下命令：<br>```cscript test.admin```<br>您将收到此错误消息：输入的错误：没有文件扩展名的脚本引擎"。 管理员。"<br>使用非标准文件扩展名的一个优点是它可防止意外地双击一个脚本，运行内容实际上没有不想要运行。 <br>这不会创建.admin 文件扩展名和 VBScript 之间永久关联。 每次运行的脚本使用.admin 文件扩展名，你将需要使用 /e 参数。|
|/h:cscript|注册**cscript.exe**作为用于运行脚本的默认脚本宿主。|
|/h:wscript|注册**wscript.exe**作为用于运行脚本的默认脚本宿主。 这是默认值时 **/h**省略选项。|
|i|指定显示警报、 脚本错误和输入的提示的交互模式。</br>这是默认值和相反**b </b**。|
|/job:\<identifier>|在运行作业由标识*标识符*中 **.wsf**脚本文件。|
|/logo|指定运行脚本前，在控制台中显示 Windows Script Host 横幅。</br>这是默认值和相反 **/nologo**。|
|/nologo|指定运行脚本前不会显示 Windows Script Host 横幅。 这是相反 **/logo**。|
|/s|将保存当前用户的当前命令提示符选项。|
|/t:\<数 >|指定脚本可以运行 （以秒为单位） 的最大时间。 您可以指定 32,767 秒。</br>默认值为没有时间限制。|
|/x|在调试器中启动该脚本。|
|ScriptArguments|指定传递给脚本的参数。 每个脚本参数的前面必须是反斜杠 （/）。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   执行此任务不要求具有管理凭据。 因此，作为安全方面的最佳做法，请考虑以不具有管理凭据的用户身份执行该任务。
-   若要打开命令提示符，请在“开始”屏幕上，键入 **cmd**，然后单击“命令提示符”。
-   每个参数是可选的;但是，您不能指定脚本参数，而无需指定一个脚本。 如果未指定的脚本或任何脚本的参数、 **wscript.exe**显示**Windows 脚本主机设置**对话框中，可用于设置该的所有脚本的全局脚本属性**wscript.exe**在本地计算机上运行。
-   **/T**参数可以设置一个计时器，从而防止过多正在运行的脚本。 时间超过指定的值时**wscript**中断脚本引擎并结束该进程。
-   Windows 脚本文件通常具有以下文件扩展名之一： **.wsf**， **.vbs**， **.js**。
-   如果您双击扩展名为没有关联的脚本文件**打开**对话框随即出现。 选择**wscript**或**cscript**，然后选择**始终使用该计划来打开此文件类型**。 这将会注册**wscript.exe**或**cscript.exe**作为此文件类型的文件的默认脚本宿主。
-   可以设置单个脚本的属性。 请参阅[Windows Script Host 概述](https://technet.microsoft.com/library/cc738350(v=ws.10).aspx)有关详细信息。
-   可以使用 Windows Script Host **.wsf**脚本文件。 每个 **.wsf**文件可以使用多个脚本引擎并执行多个作业。

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)

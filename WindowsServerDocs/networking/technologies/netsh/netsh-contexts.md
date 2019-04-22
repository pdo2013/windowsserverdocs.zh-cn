---
title: Netsh 命令语法、上下文和格式设置
description: 可以使用本主题以了解如何输入 netsh 上下文和子上下文，了解 netsh 语法和命令格式设置，以及如何在本地和远程计算机运行 Windows Server 2016 或 Windows 10 上运行的 netsh 命令。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: adb77d841ba4d69b0d36bc7f19d4707638530c97
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823688"
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Netsh 命令语法、上下文和格式设置

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题以了解如何输入 netsh 上下文和子上下文，了解 netsh 语法和命令格式设置，以及如何在本地和远程计算机上运行的 netsh 命令。

Netsh 是计算机的命令行脚本实用工具，可用于显示或修改当前正在运行的网络配置。 可以通过在 netsh 提示符下键入命令运行 Netsh 命令和他们可以在批处理文件或脚本中使用。 可以使用 netsh 命令配置远程计算机和本地计算机。

Netsh 还提供脚本功能，可让你在批处理模式下对指定的计算机运行一组命令。 你可以使用 Netsh 将配置脚本保存在文本文件中，以便存档或者帮助你配置其他计算机。

## <a name="netsh-contexts"></a>Netsh 上下文

Netsh 与其他操作系统组件交互使用动态\-链接库\(DLL\)文件。 

每个 netsh 帮助程序 DLL 提供了一组广泛的功能称为*上下文*，这是一组特定于网络的服务器角色或功能的命令。 这些上下文提供的配置和监视对一个或多个服务、 实用工具或协议的支持扩展 netsh 的功能。 例如，Dhcpmon.dll 提供 netsh 使用上下文和组的配置和管理 DHCP 服务器所需的命令。

### <a name="obtain-a-list-of-contexts"></a>获取上下文的列表

可以通过运行 Windows Server 2016 或 Windows 10 的计算机上打开命令提示符或 Windows PowerShell 获取 netsh 上下文的列表。 键入命令**netsh**然后按 ENTER。 类型 **/？**，然后按 ENTER。

下面是运行 Windows Server 2016 Datacenter 的计算机上的这些命令的示例输出。

    PS C:\Windows\system32> netsh
    netsh>/?
    
    The following commands are available:
    
    Commands in this context:
    ..            - Goes up one context level.
    ?             - Displays a list of commands.
    abort         - Discards changes made while in offline mode.
    add           - Adds a configuration entry to a list of entries.
    advfirewall   - Changes to the `netsh advfirewall' context.
    alias         - Adds an alias.
    branchcache   - Changes to the `netsh branchcache' context.
    bridge        - Changes to the `netsh bridge' context.
    bye           - Exits the program.
    commit        - Commits changes made while in offline mode.
    delete        - Deletes a configuration entry from a list of entries.
    dhcpclient    - Changes to the `netsh dhcpclient' context.
    dnsclient     - Changes to the `netsh dnsclient' context.
    dump          - Displays a configuration script.
    exec          - Runs a script file.
    exit          - Exits the program.
    firewall      - Changes to the `netsh firewall' context.
    help          - Displays a list of commands.
    http          - Changes to the `netsh http' context.
    interface     - Changes to the `netsh interface' context.
    ipsec         - Changes to the `netsh ipsec' context.
    ipsecdosprotection - Changes to the `netsh ipsecdosprotection' context.
    lan           - Changes to the `netsh lan' context.
    namespace     - Changes to the `netsh namespace' context.
    netio         - Changes to the `netsh netio' context.
    offline       - Sets the current mode to offline.
    online        - Sets the current mode to online.
    popd          - Pops a context from the stack.
    pushd         - Pushes current context on stack.
    quit          - Exits the program.
    ras           - Changes to the `netsh ras' context.
    rpc           - Changes to the `netsh rpc' context.
    set           - Updates configuration settings.
    show          - Displays information.
    trace         - Changes to the `netsh trace' context.
    unalias       - Deletes an alias.
    wfp           - Changes to the `netsh wfp' context.
    winhttp       - Changes to the `netsh winhttp' context.
    winsock       - Changes to the `netsh winsock' context.
    
    The following sub-contexts are available:
     advfirewall branchcache bridge dhcpclient dnsclient firewall http interface ipsec ipsecdosprotection lan namespace netio ras rpc trace wfp winhttp winsock
    
    To view help for a command, type the command, followed by a space, and then
     type ?.


### <a name="subcontexts"></a>子上下文

Netsh 上下文可以包含命令和其他上下文中，调用*子上下文的局部*。 例如，在路由上下文中，您可以更改到 IP 和 IPv6 子上下文。

若要显示的命令和在上下文中，在 netsh 提示符下，可以使用的子上下文列表键入上下文名称，然后键入任一 **/？** 或**帮助**。 例如，若要在路由上下文中，在 netsh 提示符下显示的子上下文和可以使用的命令列表\(，即**netsh&gt;**\)，键入以下值之一：

**路由 /？**

**路由的帮助**

若要另一个上下文中执行任务，而无需更改当前上下文中，键入想要在 netsh 提示符下使用的命令的上下文路径。 例如，若要将名为 IGMP 上下文中而无需为 IGMP 上下文，在 netsh 提示符下，第一个更改"本地区域连接"接口添加键入：

**路由 ip igmp 添加接口"本地连接"startupqueryinterval = 21**

## <a name="running-netsh-commands"></a>正在运行的 netsh 命令

若要运行 netsh 命令，必须首先 netsh 命令提示符下键入**netsh**然后按 ENTER。 接下来，您可以更改为包含你想要使用的命令的上下文。 可用的上下文取决于已安装的网络组件。 例如，如果键入**dhcp**在 netsh 提示符下按 ENTER 键，netsh 更改为 DHCP 服务器上下文。 如果您不具备 DHCP 安装，但是，出现以下消息：

**找不到以下命令： dhcp。**

## <a name="formatting-legend"></a>格式化图例

可以使用以下格式设置图例理解和在 netsh 提示符下或批处理文件或脚本中运行命令时使用正确的 netsh 命令语法。

- 中的文本*斜体*是键入命令时必须提供的信息。 例如，如果一条命令有一个名为的参数*用户名*，必须键入的实际用户名。
- 中的文本**加粗**是必须严格按照所示键入该命令时键入的信息。
- 文本跟省略号\(...\)是可以在命令行中多次重复的参数。
- 方括号之间的文本 [&nbsp;] 是可选的一项。
- 大括号之间的文本 {&nbsp;} 用竖线分隔的选项提供了一组选择从中必须仅选择了一个，如`{enable|disable}`。
- 用 Courier 字体格式的文本是代码或程序输出。

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>从命令提示符或 Windows PowerShell 中运行的 Netsh 命令

若要启动网络 Shell 并输入 netsh 命令提示符或 Windows PowerShell 中，可以使用以下命令。

### <a name="netsh"></a>netsh

Netsh 是计算机的命令行脚本实用工具，可用于，本地或远程，显示或修改当前正在运行的网络配置。 使用不带参数， **netsh**将打开 Netsh.exe 命令提示符\(即**netsh&gt;**\)。

#### <a name="syntax"></a>语法

**netsh** \[ **-a**&nbsp;*别名文件*\] \[ **-c** &nbsp; *上下文* \] \[ **-r**&nbsp;*远程计算机*\] \[ **-u** \[ *DomainName\\*  \] *用户名* \] \[ **-p** &nbsp;*密码* |  \* \] \[{*NetshCommand* | **-f** &nbsp;*ScriptFile*}\]

#### <a name="parameters"></a>Parameters

**`-a`**

可选。 指定将返回到**netsh**提示运行之后*别名文件*。

**`AliasFile`**

可选。 指定包含一个或多个文本文件的名称**netsh**命令。

**`-c`**

可选。 指定该 netsh 进入指定**netsh**上下文。

**`Context`**

可选。 指定**netsh**你想要输入的上下文。 

**`-r`**

可选。 指定你想要在远程计算机上运行的命令。

>[!IMPORTANT]
>当您使用一些 netsh 命令远程在另一台计算机上**netsh – r**参数，必须在远程计算机上运行远程注册表服务。 如果未运行，Windows 将显示"未找到网络路径"错误消息。

***`RemoteComputer`***

可选。 指定你想要配置的远程计算机。

**`-u`**

可选。 指定你想要运行的用户帐户下的 netsh 命令。

***`DomainName\\`***

可选。 指定的用户帐户所在的域。 如果，默认值为本地域*DomainName\\* 未指定。

***`UserName`***

可选。 指定的用户帐户名称。

**`-p`**

可选。 指定你想要为用户帐户提供密码。

***`Password`***

可选。 指定使用指定的用户帐户的密码 **-u** *用户名*。

***`NetshCommand`***

可选。 指定**netsh**你想要运行的命令。

**`-f`**

可选。 退出**netsh**运行与指定的脚本后*ScriptFile*。

***`ScriptFile`***

可选。 指定你想要运行的脚本。

**`/?`**

可选。 在 netsh 提示符下显示帮助。

>[!NOTE]
>如果指定**`-r`** 另一个命令后, 跟**netsh**远程计算机上运行该命令，然后返回到 Cmd.exe 命令提示符下。 如果指定**`-r`** 而无需另一个命令**netsh**在远程模式下打开。 该过程是类似于使用**集机**的 Netsh 命令提示符处。 当你使用**`-r`**，设置的当前实例的目标计算机**netsh**仅。 退出并重新输入后**netsh**，目标计算机重置为本地计算机。 你可以运行**netsh**通过指定计算机的远程计算机上的命令名称在 WINS 中存储、 UNC 名称、 Internet 名称解析由 DNS 服务器或 IP 地址。

**键入 netsh 命令的参数字符串值**

在 Netsh 命令参考整个有包含一个字符串值是必需的参数的命令。

在其中一个字符串值之间包含空格字符，如包含多个单词的字符串值的情况下也需要将字符串值括在引号中。 例如，对于名为的参数**接口**字符串值为**无线网络连接**，字符串值两边使用引号：

**`interface="Wireless Network Connection"`**


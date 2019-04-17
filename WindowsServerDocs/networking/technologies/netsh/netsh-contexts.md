---
title: Netsh 命令语法、上下文和格式
description: 你可以使用此主题以了解如何输入 netsh 上下文和子上下文，了解 netsh 语法和命令格式，以及如何在运行 Windows Server 2016 或 Windows 10 的本地和远程计算机上运行 netsh 命令。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c7c16674b831431ff5bb1d0172cc2b2a939008f6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Netsh 命令语法、上下文和格式

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此主题以了解如何输入 netsh 上下文和子上下文，了解 netsh 语法和命令格式，以及如何在本地和远程计算机上运行 netsh 命令。

Netsh 是命令行脚本实用工具，以便你可以显示或修改的计算机的当前运行的网络配置。 可通过键入命令，在 netsh 提示符下运行 Netsh 命令，在他们可以使用批量文件或脚本。 可以通过使用 netsh 命令配置远程计算机和在本地计算机。

Netsh 还会提供允许您批模式下对特定的计算机中运行命令一组脚本功能。 与 netsh，你可以将配置脚本保存文本文件为了存档或帮助你将其他计算机配置中。

## <a name="netsh-contexts"></a>Netsh 上下文

Netsh 使用 dynamic\ 链接库 \(DLL\) 文件与其他操作系统的组件进行交互。 

DLL 每个 netsh 的帮助人员提供的大量称为功能*上下文*，这是一组命令特定于网络服务器角色或功能。 这些上下文扩展 netsh 功能，通过提供配置和监视为一项或多个服务、实用程序或协议的支持。 例如，Dhcpmon.dll 提供 netsh 上下文和套配置和管理 DHCP 服务器所需的命令。

### <a name="obtain-a-list-of-contexts"></a>获取上下文的列表

你可以通过运行 Windows Server 2016 或 Windows 10 的计算机上打开的命令提示符或 Windows PowerShell 获取 netsh 上下文中的列表。 键入此命令**netsh**并按 ENTER。 键入**/？**，然后按 ENTER。

以下是运行 Windows Server 2016 Datacenter 的计算机上的以下命令示例输出。

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


### <a name="subcontexts"></a>子

Netsh 上下文可能包含命令和其他上下文，称为*子上下文*。 例如，在路由环境中，你可以更改到 IP 和 IPv6 子上下文。

以显示子上下文中的上下文，netsh 提示时，可用于命令以及列表键入上下文名称，然后键入或者**/？** 或**帮助**。 例如，若要显示上下文以及你可以使用的命令列表路由环境中，在提示符下 netsh \ (即**netsh&gt;**\)，键入以下选项之一：

**路由 /？**

**路由帮助**

若要在另一台上下文中执行的任务，而无需更改来自你当前的上下文，键入你想要在 netsh 提示时使用的命令上下文路径。 例如，若要添加命名 IGMP 上下文中的"本地连接"，而改 IGMP 上下文，netsh 提示时，不的接口键入：

**路由 ip igmp 添加界面"本地连接"startupqueryinterval = 21**

## <a name="running-netsh-commands"></a>运行 netsh 命令

若要运行 netsh 命令时，你必须启动 netsh 从命令提示符通过键入**netsh**并按 ENTER。 接下来，你可以更改上下文，其中包含你想要使用的命令。 提供的上下文取决于你已安装的网络组件。 例如，如果你键入**dhcp** netsh 提示符下，按 ENTER 以 netsh 变为 DHCP 服务器上下文。 如果你不 DHCP 安装，但是，将显示以下消息：

**找不到以下命令：dhcp。**

## <a name="formatting-legend"></a>格式化传说

以下格式化图例用于解释和使用正确 netsh 命令语法时您在提示符下 netsh 或批量文件或脚本中运行命令。

- 中的文本*倾斜*是时键入命令，你必须提供的信息。 例如，如果某个命令有一个名为-参数*用户名*，你必须键入实际的用户名。
- 中的文本**加粗**必须准确显示时，你可以键入命令键入的信息。
- 省略号 \(...\) 后跟文本是参数，可在命令行重复多次。
- 装托架之间的文本 [&nbsp;] 是可选的一项。
- 大括号之间的文本 {&nbsp;} 选择管道的分隔提供了一套选项从中你必须选择一个，如`{enable|disable}`。
- 与 Courier 字体格式化文本是或程序输出。

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>从命令提示符或 Windows PowerShell 运行 Netsh 命令

若要开始网络 Shell 和输入 netsh 在命令提示符下，或在 Windows PowerShell 中，你可以使用以下命令。

### <a name="netsh"></a>netsh

Netsh 是计算机的一个允许你在本地或远程，显示或修改的当前正在运行的网络配置的命令行脚本实用程序。 使用不带参数，**netsh**打开 Netsh.exe 命令提示符 \ (即**netsh&gt;**\)。

#### <a name="syntax"></a>语法

**netsh**\[ **-a**&nbsp;*AliasFile*\] \[ **-c**&nbsp;*Context* \] \[**-r**&nbsp;*RemoteComputer*\] \[ **-u** \[ *DomainName\\* \] *UserName* \] \[ **-p**&nbsp;*Password* | \*\] \[{*NetshCommand* | **-f**&nbsp;*ScriptFile*}\]

#### <a name="parameters"></a>参数

**`-a`**

可选。 指定你返回到**netsh**提示符运行后*别名文件*。

**`AliasFile`**

可选。 指定的名称的文件，包含一项或多个**netsh**命令。

**`-c`**

可选。 指定该 netsh 输入指定**netsh**上下文。

**`Context`**

可选。 指定**netsh**你想要输入的上下文。 

**`-r`**

可选。 指定你希望在远程计算机上运行该命令。

>[!IMPORTANT]
>当你使用某些 netsh 命令远程在另一台计算机上**netsh – r**参数，必须在远程计算机上运行远程注册表服务。 如果它未运行的，Windows 将显示"网络路径找不到"错误消息。

***`RemoteComputer`***

可选。 指定你希望配置远程计算机。

**`-u`**

可选。 指定你希望运行 netsh 命令的用户帐户。

***`DomainName\\`***

可选。 指定的用户帐户的域。 默认值为地域如果*DomainName\\*未指定。

***`UserName`***

可选。 指定的用户帐户名。

**`-p`**

可选。 指定你希望提供的用户帐户的密码。

***`Password`***

可选。 指定你指定的用户帐户的密码**-u***用户名*。

***`NetshCommand`***

可选。 指定**netsh**你想要运行的命令。

**`-f`**

可选。 退出**netsh**后运行的你与指定的脚本*脚本文件*。

***`ScriptFile`***

可选。 指定你想要运行的脚本。

**`/?`**

可选。 显示在 netsh 提示符帮助。

>[!NOTE]
>如果你要指定**`-r`**另一个命令后, 跟**netsh**远程计算机上运行该命令，然后返回到 Cmd.exe 命令提示符。 如果你要指定**`-r`**没有其他命令，**netsh**远程型打开。 该进程是类似于使用**组计算机**在 Netsh 的命令提示符。 当你使用**`-r`**，设置目标计算机的当前实例**netsh**仅。 在退出并时都重新输入后**netsh**，与本地计算机重置目标计算机。 你可以运行**netsh**通过指定一台计算机远程计算机上的命令命名存储在 WINS、UNC 名称、Internet 名称以解决方法是 DNS 服务器，或者 IP 地址。

**键入参数字符串值为 netsh 命令**

Netsh 命令参考整个有包含的参数字符串值的所需的命令。

在这种情况，其中一个包含字符，如组成的多个字词，字符串值之间的空格是才能将字符串值放在引号。 例如，用于名为参数**界面**字符串值为**无线网络连接**，使用引号字符串值：

**`interface="Wireless Network Connection"`**


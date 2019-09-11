---
ms.date: 01/07/2019
ms.topic: conceptual
keywords: OpenSSH，SSH，SSHD，安装，安装程序
contributor: maertendMSFT
author: maertendMSFT
title: 安装适用于 Windows 的 OpenSSH
ms.openlocfilehash: 6a5d4d47fbb3f962c2a19582eb0a72810145a28c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866883"
---
# <a name="installation-of-openssh-for-windows-server-2019-and-windows-10"></a>Windows Server 2019 和 Windows 10 的 OpenSSH 安装 #

OpenSSH 客户端和 OpenSSH 服务器是 Windows Server 2019 和 Windows 10 1809 中的可单独安装组件。
具有这些 Windows 版本的用户应使用以下说明安装和配置 OpenSSH。 

> [!NOTE] 
> 从 PowerShell Github 存储库获取 OpenSSH 的用户（ https://github.com/PowerShell/OpenSSH-Portable) 应使用该存储库中的说明，__不应__使用这些说明。 


## <a name="installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809"></a>从 Windows Server 2019 或 Windows 10 1809 上的 "设置" UI 安装 OpenSSH

OpenSSH 客户端和服务器是 Windows 10 1809 的可安装功能。 

若要安装 OpenSSH，请启动 "设置"，然后跳到 "应用 > 应用和功能" > 管理可选功能 

扫描此列表，查看是否已安装 OpenSSH 客户端。 如果不是，则在页面顶部选择 "添加功能"，然后： 

* 若要安装 OpenSSH 客户端，请找到 "OpenSSH 客户端"，然后单击 "安装"。 
* 若要安装 OpenSSH 服务器，请找到 "OpenSSH 服务器"，然后单击 "安装"。 

安装完成后，请返回应用 > 应用和功能 > 管理可选功能，你应看到列出的 OpenSSH 组件。

> [!NOTE]
> 安装 OpenSSH 服务器将创建并启用名为 "OpenSSH-在 TCP 内服务器" 的防火墙规则。 这允许端口22上的入站 SSH 流量。 

## <a name="installing-openssh-with-powershell"></a>通过 PowerShell 安装 OpenSSH 

若要使用 PowerShell 安装 OpenSSH，请首先以管理员身份启动 PowerShell。
若要确保可以安装 OpenSSH 功能，请执行以下操作：

```powershell
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'

# This should return the following output:

Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent
Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

然后，安装服务器和/或客户端功能：

```powershell
# Install the OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Install the OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0

# Both of these should return the following output:

Path          :
Online        : True
RestartNeeded : False
```

## <a name="uninstalling-openssh"></a>正在卸载 OpenSSH

若要使用 Windows 设置卸载 OpenSSH，请启动 "设置"，然后在 "应用和功能" > "应用和功能" > 管理可选功能 "。 在已安装的功能列表中，选择 "OpenSSH 客户端" 或 "OpenSSH 服务器" 组件，然后选择 "卸载"。

若要使用 PowerShell 卸载 OpenSSH，请使用以下命令之一：

```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

如果服务在卸载后处于使用状态，则可能需要在删除 OpenSSH 后重新启动 Windows。


## <a name="initial-configuration-of-ssh-server"></a>SSH 服务器的初始配置

若要将 OpenSSH 服务器配置为在 Windows 上初始使用，请以管理员身份启动 PowerShell，然后运行以下命令以启动 SSHD 服务：

```powershell
Start-Service sshd
# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
# Confirm the Firewall rule is configured. It should be created automatically by setup. 
Get-NetFirewallRule -Name *ssh*
# There should be a firewall rule named "OpenSSH-Server-In-TCP", which should be enabled 
```

## <a name="initial-use-of-ssh"></a>SSH 的初始使用

在 Windows 上安装 OpenSSH 服务器后，可以使用安装有 SSH 客户端的任何 Windows 设备上的 PowerShell 快速对其进行测试。 在 PowerShell 中，键入以下命令： 

```powershell
Ssh username@servername
```

第一次连接到任何服务器时，将生成如下所示的消息：

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

答案必须是 "是" 或 "否"。 如果回答 "是"，则会将该服务器添加到本地系统的已知 ssh 主机列表。

此时，系统会提示输入密码。 作为安全预防措施，你的密码将不会在你键入时显示。 

连接后，会看到类似于以下内容的命令行提示：

```
domain\username@SERVERNAME C:\Users\username>
```

Windows OpenSSH 服务器使用的默认 shell 为 Windows 命令行界面。 


---
ms.date: 01/07/2019
ms.topic: conceptual
keywords: OpenSSH、 SSH、 SSHD，安装，安装程序
contributor: maertendMSFT
author: maertendMSFT
title: 安装用于 Windows 的 OpenSSH
ms.openlocfilehash: f617b01ee7dabd4897f99e374420f673e209e145
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859558"
---
# <a name="installation-of-openssh-for-windows-server-2019-and-windows-10"></a>安装用于 Windows Server 2019 和 Windows 10 的 OpenSSH #

OpenSSH 客户端和 OpenSSH 服务器是在 Windows Server 2019 和 Windows 10 1809年的单独安装组件。
使用这些 Windows 版本的用户应使用安装和配置 OpenSSH 遵循的说明。 

> [!NOTE] 
> 从 PowerShell Github 存储库获取 OpenSSH 的用户 (https://github.com/PowerShell/OpenSSH-Portable)应在此处使用的说明和__不应__使用这些说明。 


## <a name="installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809"></a>从设置在 Windows Server 2019 或 Windows 10 1809 UI 安装 OpenSSH

OpenSSH 客户端和服务器可安装 Windows 10 1809年功能。 

若要安装 OpenSSH，开始设置，则转到应用程序 > 应用程序和功能 > 管理可选功能。 

扫描此列表，以 OpenSSH 客户端是否已安装。 如果没有，然后在页面顶部选择"添加功能"，然后： 

* 若要安装的 OpenSSH 客户端，找到"OpenSSH 客户端"，然后单击"安装"。 
* 若要安装 OpenSSH 服务器，找到"OpenSSH 服务器"，然后单击"安装"。 

安装完成后，返回到应用程序 > 应用程序和功能 > 管理可选功能，您应看到列出的 OpenSSH 组件。

> [!NOTE]
> 安装 OpenSSH 服务器将创建并启用一个名为"OpenSSH 服务器-中-TCP"的防火墙规则。 这允许端口 22 上的入站的 SSH 流量。 

## <a name="installing-openssh-with-powershell"></a>使用 PowerShell 安装 OpenSSH 

若要安装使用 PowerShell 的 OpenSSH，请首先以管理员身份启动 PowerShell。
若要确保 OpenSSH 功能以安装方式提供：

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

## <a name="uninstalling-openssh"></a>卸载 OpenSSH

若要卸载 OpenSSH 使用 Windows 设置，启动设置，然后转到应用程序 > 应用程序和功能 > 管理可选功能。 在已安装的功能列表中，选择 OpenSSH 客户端或 OpenSSH 服务器组件中，然后选择卸载。

若要卸载 OpenSSH 使用 PowerShell，请使用以下命令之一：

```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

如果服务是在时间上将使用它删除 OpenSSH，已卸载后，可能需要 Windows 重新启动。


## <a name="initial-configuration-of-ssh-server"></a>使用 SSH 服务器的初始配置

若要在 Windows 上配置初次使用 OpenSSH 服务器，启动 PowerShell，以管理员身份，然后运行以下命令以启动 SSHD 服务：

```powershell
Start-Service sshd
# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
# Confirm the Firewall rule is configured. It should be created automatically by setup. 
Get-NetFirewallRule -Name *ssh*
# There should be a firewall rule named "OpenSSH-Server-In-TCP", which should be enabled 
```

## <a name="initial-use-of-ssh"></a>SSH 的初始使用

在 Windows 上的情况下安装 OpenSSH 服务器后，可以快速测试它使用 PowerShell 从任何 Windows 设备的 SSH 客户端安装。 在 PowerShell 中键入以下命令： 

```powershell
Ssh username@servername
```

第一个连接到任何服务器将导致类似于以下的消息：

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

答案必须是"yes"否"。 回答是将该服务器添加到本地系统的了解列表的 ssh 主机。

系统会提示输入密码在这里。 键入时，不会作为安全预防措施，显示你的密码。 

连接后你将看到类似于以下的命令外壳提示：

```
domain\username@SERVERNAME C:\Users\username>
```

Windows OpenSSH 服务器使用的默认 shell 是 Windows 命令行界面。 


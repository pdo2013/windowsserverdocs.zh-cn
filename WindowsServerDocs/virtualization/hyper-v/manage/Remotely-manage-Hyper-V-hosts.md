---
title: 远程管理 Hyper-v 主机
description: 介绍 Hyper-v 主机和 Hyper-v 管理器之间的版本兼容性，以及如何连接到不同环境中的远程主机，包括跨域和独立环境。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d34e98c-6134-479b-8000-3eb360b8b8a3
author: KBDAzure
ms.author: kathydav
ms.date: 12/06/2016
ms.openlocfilehash: 677e054fe42978697ef786b73daac75069f0408f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392687"
---
# <a name="remotely-manage-hyper-v-hosts-with-hyper-v-manager"></a>通过 Hyper-v 管理器远程管理 Hyper-v 主机

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows 10 Windows 8。1

本文列出了 Hyper-v 主机和 Hyper-v 管理器版本的支持组合，并介绍了如何连接到远程和本地 Hyper-v 主机，以便你可以对其进行管理。 

Hyper-v 管理器允许你管理少量的 Hyper-v 主机（远程和本地）。 它在安装 Hyper-v 管理工具时安装，你可以通过完全 Hyper-v 安装或仅工具安装执行此操作。 执行仅工具安装意味着你可以使用不满足硬件要求的计算机上的工具来托管 Hyper-v。 有关 Hyper-v 主机硬件的详细信息，请参阅[系统要求](../System-requirements-for-Hyper-V-on-Windows.md)。

如果未安装 Hyper-v 管理器，请参阅下面的[说明](#install-hyper-v-manager)。

## <a name="supported-combinations-of-hyper-v-manager-and-hyper-v-host-versions"></a>支持的 Hyper-v 管理器和 Hyper-v 主机版本组合

在某些情况下，可以使用与主机上的 Hyper-v 版本不同的 Hyper-v 管理器版本，如表中所示。 当你执行此操作时，Hyper-v 管理器将提供适用于你所管理的主机上的 Hyper-v 版本的功能。 例如，如果你使用 Windows Server 2012 R2 中的 Hyper-v 管理器版本远程管理在 Windows Server 2012 中运行 Hyper-v 的主机，你将无法使用该 Hyper-v 主机上的 Windows Server 2012 R2 中提供的功能。

下表显示了可以从 Hyper-v 管理器的特定版本管理的 Hyper-v 主机版本。 仅列出受支持的操作系统版本。 有关特定操作系统版本的支持状态的详细信息，请使用 " [Microsoft 生命周期策略](https://support.microsoft.com/lifecycle)" 页上的 "**搜索产品生命周期**" 按钮。 通常，早期版本的 Hyper-v 管理器只能管理运行相同版本或类似 Windows Server 版本的 Hyper-v 主机。

|Hyper-v 管理器版本 | Hyper-v 主机版本|
|---|---|
|Windows 2016，Windows 10|-Windows Server 2016-所有版本和安装选项，包括 Nano Server，以及 Hyper-v 服务器的相应版本 <br>-Windows Server 2012 R2-所有版本和安装选项，以及 Hyper-v 服务器的相应版本 <br>-Windows Server 2012-所有版本和安装选项，以及 Hyper-v 服务器的相应版本 <br> -Windows 10 <br> - Windows 8.1  |
| Windows Server 2012 R2，Windows 8。1 | -Windows Server 2012 R2-所有版本和安装选项，以及 Hyper-v 服务器的相应版本 <br>-Windows Server 2012-所有版本和安装选项，以及 Hyper-v 服务器的相应版本 <br>- Windows 8.1
| Windows Server 2012 | -Windows Server 2012-所有版本和安装选项，以及 Hyper-v 服务器的相应版本
| Windows Server 2008 R2 Service Pack 1、Windows 7 Service Pack 1 | -Windows Server 2008 R2-所有版本和安装选项，以及 Hyper-v 服务器的相应版本
| Windows Server 2008、Windows Vista Service Pack 2 | -Windows Server 2008-所有版本和安装选项，以及 Hyper-v 服务器的相应版本

>[!NOTE]
>Windows 8 2016 年1月12日的 Service pack 支持已结束。 有关详细信息，请参阅[WINDOWS 8.1 常见问题解答](https://support.microsoft.com/help/18581)。

## <a name="connect-to-a-hyper-v-host"></a>连接到 Hyper-v 主机

若要从 Hyper-v 管理器连接到 Hyper-v 主机，请右键单击左窗格中的 " **Hyper-v 管理器**"，然后单击 "**连接到服务器**"。

## <a name="manage-hyper-v-on-a-local-computer"></a>在本地计算机上管理 Hyper-v

在添加计算机（包括本地计算机）之前，hyper-v 管理器不会列出任何托管 Hyper-v 的计算机。 为此，请执行以下操作:

1. 在左窗格中，右键单击 " **Hyper-v 管理器**"。
2. 单击 "**连接到服务器**"。
3. 从 "**选择计算机**" 中，单击 "**本地计算机**"，然后单击 **"确定"** 。

如果无法连接：

* 可能仅安装了 Hyper-v 工具。 若要检查是否已安装 Hyper-v 平台，请查找虚拟机管理服务。 /（打开 "服务" 桌面应用程序：单击 "**开始**"，单击 "**开始搜索**" 框，键入**services.msc**，然后按**enter**。 如果未列出虚拟机管理服务，请按照[安装 hyper-v](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)中的说明安装 hyper-v 平台。
* 检查你的硬件是否满足要求。 请参阅[系统要求](../System-requirements-for-Hyper-V-on-Windows.md)。
* 检查你的用户帐户是否属于 Administrators 组或 Hyper-v Administrators 组。

## <a name="manage-hyper-v-hosts-remotely"></a>远程管理 Hyper-v 主机  

若要管理远程 Hyper-v 主机，请在本地计算机和远程主机上启用远程管理。

在 Windows Server 上，打开服务器管理器 @no__t**本地服务器**\>**远程管理**，然后单击 "**允许远程连接到此计算机**"。 

或者，在任一操作系统中，以管理员身份打开 Windows PowerShell 并运行： 

```
Enable-PSRemoting
```

### <a name="connect-to-hosts-in-the-same-domain"></a>连接到同一个域中的主机

对于 Windows 8.1 和更早版本，仅当主机位于同一个域中，并且本地用户帐户也在远程主机上时，远程管理才起作用。

若要将远程 Hyper-v 主机添加到 Hyper-v 管理器，请选择 "**选择计算机**" 对话框中的 "**另一台计算机**"，然后键入远程主机的主机名、NetBIOS 名称或完全限定的域名 \(FQDN @ no__t-3。

Windows Server 2016 和 Windows 10 中的 hyper-v 管理器提供了比以前版本更多的远程连接类型，如以下部分中所述。  

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-as-a-different-user"></a>以其他用户身份连接到 Windows 2016 或 Windows 10 远程主机

这允许你在本地计算机上未作为 hyper-v 管理员组或 Hyper-v 主机上的 Administrators 组的成员的用户运行时连接到 Hyper-v 主机上的用户。 为此，请执行以下操作:

1. 在左窗格中，右键单击 " **Hyper-v 管理器**"。
1. 单击 "**连接到服务器**"。
1. 选择 "**选择计算机**" 对话框中的 "**以其他用户身份连接**"。
1. 选择 "**设置用户**"。

>[!NOTE]
> 这仅适用于 Windows Server 2016 或 Windows 10**远程**主机。

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-using-ip-address"></a>使用 IP 地址连接到 Windows 2016 或 Windows 10 远程主机

为此，请执行以下操作:

1. 在左窗格中，右键单击 " **Hyper-v 管理器**"。
1. 单击 "**连接到服务器**"。
1. 在 "**另一台计算机**" 文本字段中键入 IP 地址。

>[!NOTE]
> 这仅适用于 Windows Server 2016 或 Windows 10**远程**主机。

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-outside-your-domain-or-with-no-domain"></a>连接到域之外的 Windows 2016 或 Windows 10 远程主机，或不使用域

为此，请执行以下操作:

1. 在要管理的 Hyper-v 主机上，以管理员身份打开 Windows PowerShell 会话。

1. 为专用网络区域创建必要的防火墙规则：   
   
   ```
   Enable-PSRemoting
   ```

2. 若要在公共区域上允许远程访问，请启用 CredSSP 和 WinRM 的防火墙规则：
  
   ```
   Enable-WSManCredSSP -Role server
   ```

    有关详细信息，请参阅[enable-enable-psremoting](https://technet.microsoft.com/library/hh849694.aspx) and [enable-wsmancredssp](https://technet.microsoft.com/library/hh849872.aspx)。

接下来，配置将用于管理 Hyper-v 主机的计算机。

1. 以管理员身份打开 Windows PowerShell 会话。
1. 运行以下命令:

     ```
     Set-Item WSMan:\localhost\Client\TrustedHosts -Value "fqdn-of-hyper-v-host"
     ```
     ```
     Enable-WSManCredSSP -Role client -DelegateComputer "fqdn-of-hyper-v-host"
     ```
1. 你可能还需要配置以下组策略： 
    * **计算机配置**\>**管理模板**@no__t**系统**\>**凭据委派**\>**允许向仅 NTLM 服务器身份验证委派全新凭据**
    * 单击 "**启用**" 并添加*wsman/fqdn-hyper-v 主机*。
1. 打开**Hyper-v 管理器**。
1. 在左窗格中，右键单击 " **Hyper-v 管理器**"。
1. 单击 "**连接到服务器**"。

>[!NOTE]
> 这仅适用于 Windows Server 2016 或 Windows 10**远程**主机。

有关[cmdlet 的详细](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/set-item)信息，请参阅[enable-wsmancredssp](https://technet.microsoft.com/library/hh849872.aspx)。

## <a name="install-hyper-v-manager"></a>安装 Hyper-v 管理器

若要使用 UI 工具，请选择一个适用于将运行 Hyper-v 管理器的计算机上的操作系统的工具：

在 Windows Server 上，打开服务器管理器 @no__t**管理**\>**添加角色和功能**。 转到 "**功能**" 页，然后展开 "**远程服务器管理工具**" @no__t 2 个**角色管理工具**\> 个**hyper-v 管理工具**。 

在 Windows 上，Hyper-v 管理器在[包含 hyper-v 的任何 Windows 操作系统](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility)上都可用。

1. 在 Windows 桌面上，单击 "开始" 按钮，然后开始键入 "**程序和功能**"。 
1. 在搜索结果中，单击 "**程序和功能**"。
1. 在左窗格中，单击 **"打开或关闭 Windows 功能**"。
1. 展开 Hyper-v 文件夹，然后**单击 "Hyper-v 管理工具**"。
1. 若要安装 Hyper-v 管理器，请单击 " **Hyper-v 管理工具**"。 如果还想要安装 Hyper-v 模块，请单击该选项。

若要使用 Windows PowerShell，请以管理员身份运行以下命令：

```
add-windowsfeature rsat-hyper-v-tools
```

## <a name="see-also"></a>请参阅  
 
[安装 Hyper-V](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md) 


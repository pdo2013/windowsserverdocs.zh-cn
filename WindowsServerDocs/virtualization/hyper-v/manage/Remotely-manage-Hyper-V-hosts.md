---
title: 远程管理的 HYPER-V 主机
description: 介绍之间的 HYPER-V 主机和 Hyper-v 管理器以及如何连接到远程主机在不同环境中，包括跨域和独立的版本兼容性。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d34e98c-6134-479b-8000-3eb360b8b8a3
author: KBDAzure
ms.author: kathydav
ms.date: 12/06/2016
ms.openlocfilehash: df66f308ee7999f97fe7e57a8b52256f2561faa2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870228"
---
# <a name="remotely-manage-hyper-v-hosts-with-hyper-v-manager"></a>远程管理的 HYPER-V 主机的 HYPER-V 管理器

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows 10 中，Windows 8.1

本文列出了支持的 HYPER-V 主机和 Hyper-v 管理器版本的组合，并介绍如何连接到远程和本地 HYPER-V 主机，以便可以管理它们。 

Hyper-v 管理器允许你管理少量的 HYPER-V 主机，远程和本地。 安装时安装 HYPER-V 管理工具，可以执行此操作通过完整的 HYPER-V 安装或仅工具安装。 仅工具安装意味着可以在不满足向主机的 HYPER-V 硬件要求的计算机上使用工具。 有关 HYPER-V 主机的硬件的详细信息，请参阅[系统要求](..\System-requirements-for-Hyper-V-on-Windows.md)。

如果未安装 Hyper-v 管理器，请参阅[说明](#install-hyper-v-manager)下面。

## <a name="supported-combinations-of-hyper-v-manager-and-hyper-v-host-versions"></a>支持的 Hyper-v 管理器和 HYPER-V 主机版本组合

在某些情况下可以在主机上使用的 HYPER-V 版本的 HYPER-V 管理器的版本不同，表中所示。 时执行此操作时，Hyper-v 管理器正在管理的主机上提供可用于版本的 HYPER-V 功能。 例如，如果在 Windows Server 2012 R2 中使用的版本的 HYPER-V 管理器远程管理 Windows Server 2012 中运行 HYPER-V 的主机，将无法使用该 HYPER-V 主机上的 Windows Server 2012 R2 中的可用功能。

下表显示了从特定版本的 Hyper-v 管理器可以管理哪些版本的 HYPER-V 主机。 仅支持列出的版本的操作系统。 对于特定的操作系统版本的支持状态的详细信息，请使用**搜索产品生命周期**按钮[Microsoft 生命周期策略](https://support.microsoft.com/lifecycle)页。 一般情况下，较旧版本的 Hyper-v 管理器只能管理运行相同版本或类似的 Windows Server 版本的 HYPER-V 主机。

|Hyper-v 管理器版本 | HYPER-V 主机版本|
|---|---|
|Windows 2016 中，Windows 10|Windows Server 2016-所有版本和安装都选项，包括 Nano Server 和 HYPER-V 服务器的相应版本 <br>Windows Server 2012 R2-所有版本和安装选项和 HYPER-V 服务器的相应版本 <br>Windows Server 2012-所有版本和安装选项和 HYPER-V 服务器的相应版本 <br> - Windows 10 <br> - Windows 8.1  |
| Windows Server 2012 R2, Windows 8.1 | Windows Server 2012 R2-所有版本和安装选项和 HYPER-V 服务器的相应版本 <br>Windows Server 2012-所有版本和安装选项和 HYPER-V 服务器的相应版本 <br>- Windows 8.1
| Windows Server 2012 | Windows Server 2012-所有版本和安装选项和 HYPER-V 服务器的相应版本
| Windows Server 2008 R2 Service Pack 1、 Windows 7 Service Pack 1 | Windows Server 2008 R2-所有版本和安装选项和 HYPER-V 服务器的相应版本
| Windows Server 2008、 Windows Vista Service Pack 2 | Windows Server 2008-所有版本和安装选项和 HYPER-V 服务器的相应版本

>[!NOTE]
>Service pack 支持 2016 年 1 月 12 日结束的 Windows 8。 有关详细信息，请参阅[Windows 8.1 常见问题](https://support.microsoft.com/help/18581)。

## <a name="connect-to-a-hyper-v-host"></a>连接到 HYPER-V 主机

若要从 Hyper-v 管理器连接到 HYPER-V 主机，右键单击**Hyper-v 管理器**在左窗格中，然后单击**连接到服务器**。

## <a name="manage-hyper-v-on-a-local-computer"></a>在本地计算机上管理的 HYPER-V

Hyper-v 管理器不会列出任何之前添加的计算机，包括本地计算机托管的 HYPER-V 的计算机。 要实现此目的，请执行以下操作：

1. 在左窗格中，右键单击**Hyper-v 管理器**。
2. 单击**连接到服务器**。
3. 从**选择计算机**，单击**本地计算机**，然后单击**确定**。

如果无法连接：

* 就可以安装仅 HYPER-V 工具。 若要检查安装了 HYPER-V 平台，请查找虚拟机管理服务。 \(打开服务桌面应用程序： 单击**启动**，单击**开始搜索**框中，键入**services.msc**，然后按**Enter**。 如果未列出的虚拟机管理服务，请按照中的说明安装 HYPER-V 平台[安装 HYPER-V](..\get-started\Install-the-Hyper-V-role-on-Windows-Server.md)。\)
* 检查硬件满足要求。 请参阅[系统要求](..\System-requirements-for-Hyper-V-on-Windows.md)。
* 检查你的用户帐户属于管理员组或 HYPER-V 管理员组。

## <a name="manage-hyper-v-hosts-remotely"></a>远程管理的 HYPER-V 主机  

若要管理远程 HYPER-V 主机，启用远程管理本地计算机和远程主机上。

在 Windows Server 上打开服务器管理器\>**本地服务器** \>**远程管理**，然后单击**允许远程连接到此计算机**. 

或者，从任一操作系统打开以管理员身份的 Windows PowerShell 并运行： 

```
Enable-PSRemoting
```

### <a name="connect-to-hosts-in-the-same-domain"></a>连接到同一个域中的主机

对于 Windows 8.1 及更早版本，远程管理仅适用于此主机处于同一个域和你的本地用户帐户也是远程主机上。

若要添加到 Hyper-v 管理器远程 HYPER-V 主机，请选择**另一台计算机**中**选择计算机**对话框中，键入远程主机的主机名、 NetBIOS 名称或完全限定的域名\(FQDN\)。

Windows Server 2016 和 Windows 10 中的 HYPER-V 管理器提供了比以前版本中，以下各节中所述的更多类型的远程连接。  

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-as-a-different-user"></a>以不同用户身份连接到 Windows 2016 或 Windows 10 的远程主机

这使你能够连接到 HYPER-V 主机不运行时在本地计算机上以用户是 HYPER-V 管理员组或 HYPER-V 主机上的 Administrators 组的成员身份。 要实现此目的，请执行以下操作：

1. 在左窗格中，右键单击**Hyper-v 管理器**。
1. 单击**连接到服务器**。
1. 选择**以其他用户身份连接**中**选择计算机**对话框的。
1. 选择**将用户设置**。

>[!NOTE]
> 这仅适用于 Windows Server 2016 或 Windows 10**远程**主机。

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-using-ip-address"></a>连接到 Windows 2016 或 Windows 10 远程主机使用的 IP 地址

要实现此目的，请执行以下操作：

1. 在左窗格中，右键单击**Hyper-v 管理器**。
1. 单击**连接到服务器**。
1. 键入到的 IP 地址**另一台计算机**文本字段。

>[!NOTE]
> 这仅适用于 Windows Server 2016 或 Windows 10**远程**主机。

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-outside-your-domain-or-with-no-domain"></a>连接到 Windows 2016 或 Windows 10 远程主机在域之外或没有域

要实现此目的，请执行以下操作：

1. 在要管理的 HYPER-V 主机上, 打开 Windows PowerShell 会话中的以管理员身份。

1. 创建专用的网络区域的必要的防火墙规则：   
   
   ```
   Enable-PSRemoting
   ```

2. 若要允许在公共区域上的远程访问，请启用 CredSSP 和 WinRM 防火墙规则：
  
   ```
   Enable-WSManCredSSP -Role server
   ```

    有关详细信息，请参阅[Enable-psremoting](https://technet.microsoft.com/library/hh849694.aspx)并[Enable-wsmancredssp](https://technet.microsoft.com/library/hh849872.aspx)。

接下来，配置将用于管理的 HYPER-V 主机的计算机。

1. 打开 Windows PowerShell 会话以管理员身份。
1. 运行以下命令：

     ```
     Set-Item WSMan:\localhost\Client\TrustedHosts -Value "fqdn-of-hyper-v-host"
     ```
     ```
     Enable-WSManCredSSP -Role client -DelegateComputer "fqdn-of-hyper-v-host"
     ```
1. 您可能还需要配置以下组策略： 
    * **计算机配置** \> **管理模板** \> **系统** \> **凭据委派** \> **允许委派新的凭据用于仅 NTLM 服务器身份验证**
    * 单击**启用**并添加*wsman/fqdn 的-hyper-v-主机*。
1. 打开**HYPER-V 管理器**。
1. 在左窗格中，右键单击**Hyper-v 管理器**。
1. 单击**连接到服务器**。

>[!NOTE]
> 这仅适用于 Windows Server 2016 或 Windows 10**远程**主机。

有关 cmdlet 的详细信息，请参阅[Set-item](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/set-item)并[Enable-wsmancredssp](https://technet.microsoft.com/library/hh849872.aspx)。

## <a name="install-hyper-v-manager"></a>安装 HYPER-V 管理器

若要使用的 UI 工具，请选择适用于将运行的 HYPER-V 管理器的计算机上的操作系统：

在 Windows Server 上打开服务器管理器\>**管理** \> **添加角色和功能**。 将移动到**功能**页上，展开**远程服务器管理工具** \> **角色管理工具** \> **HYPER-V 管理工具**。 

在 Windows 中上的 HYPER-V 管理器是可在上找到[任何包括 HYPER-V 的 Windows 操作系统](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility)。

1. 在 Windows 桌面上，单击开始按钮，然后开始键入**程序和功能**。 
1. 在搜索结果中，单击**程序和功能**。
1. 在左窗格中，单击**打开或关闭打开的 Windows 功能**。
1. 展开 HYPER-V 文件夹中，并**单击 HYPER-V 管理工具**。
1. 若要安装 Hyper-v 管理器，单击**HYPER-V 管理工具**。 如果你想要安装的 HYPER-V 模块，请单击该选项。

若要使用 Windows PowerShell，请以管理员身份运行以下命令：

```
add-windowsfeature rsat-hyper-v-tools
```

## <a name="see-also"></a>请参阅  
 
[安装 Hyper-V](..\get-started\Install-the-Hyper-V-role-on-Windows-Server.md) 


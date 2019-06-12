---
title: 安装 Windows Admin Center
description: 如何安装 Windows Admin Center Windows PC 上或服务器上，以便多个用户可以访问 Windows Admin Center 使用 web 浏览器。
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a9eb7944cd35dfa68e3c36cdc6c016f483a9f1e1
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811962"
---
# <a name="install-windows-admin-center"></a>安装 Windows Admin Center

> 适用于：Windows Admin Center，Windows Admin Center 预览版

本主题介绍如何安装 Windows Admin Center Windows PC 上或服务器上，以便多个用户可以访问 Windows Admin Center 使用 web 浏览器。

> [!Tip]
> 不熟悉 Windows Admin Center？
> [了解有关 Windows Admin Center 的更多信息](../understand/windows-admin-center.md)或[立即下载](https://aka.ms/windowsadmincenter)。

## <a name="determine-your-installation-type"></a>确定您的安装类型

审阅[安装选项](../plan/installation-options.md)其中包括[受支持的操作系统](../plan/installation-options.md#supported-operating-systems-installation)。 若要在 Azure 中的 VM 上安装 Windows Admin Center，请参阅[在 Azure 中部署 Windows Admin Center](../azure/deploy-wac-in-azure.md)。

## <a name="install-on-windows-10"></a>在 Windows 10 上安装

在 Windows 10 上安装 Windows Admin Center 时，默认情况下使用端口 6516，但是你可以选择指定其他端口。 你还可以创建桌面快捷方式，并让 Windows Admin Center 管理你的 TrustedHosts。

> [!NOTE]
> 在工作组环境中或在域中使用本地管理员凭据时需要修改 TrustedHosts。 如果你选择放弃此设置，则必须[手动配置 TrustedHosts](../support/troubleshooting.md#configure-trustedhosts)。

从**开始**菜单中启动 Windows Admin Center 时，它会在默认浏览器中打开。

首次启动 Windows Admin Center 时，你将在桌面的通知区域中看到一个图标。 右键单击此图标，然后选择**打开**以在默认浏览器中打开该工具，或选择**退出**以退出后台进程。

## <a name="install-on-windows-server-with-desktop-experience"></a>在具有桌面体验的 Windows Server 上安装

在 Windows Server 上，系统会以网络服务形式安装 Windows Admin Center。 你必须指定服务侦听的端口，但是，它需要 HTTPS 的证书。 安装程序可以创建自签名证书以进行测试，你也可以提供已在计算机上安装的证书的指纹。 如果你使用生成的证书，那么它将与服务器的 DNS 名称匹配。 如果使用自己的证书，请确保证书中提供的名称匹配的计算机名称 （不支持证书的通配符。）您还可以选择让管理你 TrustedHosts Windows Admin Center。

> [!NOTE]
> 在工作组环境中或在域中使用本地管理员凭据时需要修改 TrustedHosts。 如果你选择放弃此设置，则必须[手动配置 TrustedHosts](../support/troubleshooting.md#configure-trustedhosts)

安装完成后，从远程计算机打开浏览器并导航到安装程序的最后一步中显示的 URL。

> [!WARNING]
> 自动生成的证书在安装后的 60 天过期。

## <a name="install-on-server-core"></a>在 Server Core 上安装

如果你有 Windows Server 的 Server Core 安装，则可以利用命令提示符（以管理员身份运行）安装 Windows Admin Center。 分别使用 `SME_PORT` 和 `SSL_CERTIFICATE_OPTION` 参数指定端口和 SSL 证书。 如果你要使用现有证书，请使用 `SME_THUMBPRINT` 指定其指纹。

> [!WARNING]
> 安装 Windows Admin Center 将重启 WinRM 服务，将会断开所有远程 PowerShells 会话。 建议你从本地 Cmd 或 PowerShell 安装。 如果您正在安装会通过 WinRM 服务重新启动中断的自动化解决方案，可以添加参数```RESTART_WINRM=0```安装参数，但 WinRM 必须重新启动的 Windows Admin Center 函数。

运行以下命令以安装 Windows Admin Center 并自动生成自签名的证书：

```   
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SSL_CERTIFICATE_OPTION=generate
```

运行以下命令以使用现有证书安装 Windows Admin Center：

```
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SME_THUMBPRINT=<thumbprint> SSL_CERTIFICATE_OPTION=installed
```

> [!WARNING]
> 不要使用点斜杠相对路径表示法（例如 `.\<WindowsAdminCenterInstallerName>.msi`）从 PowerShell 调用 `msiexec`。 该表示法不受支持，会导致安装失败。 请删除 `.\` 前缀或指定 MSI 的完整路径。

## <a name="updating-windows-admin-center"></a>更新 Windows Admin Center

通过使用 Microsoft 更新或手动安装，可以更新 Windows Admin Center 的非预览版本。 

升级到新版 Windows Admin Center 时保留您的设置。 我们不正式支持的 Windows Admin Center 升级 Insider Preview 版本-我们认为这是更好的做法进行全新安装的但我们不会阻止它。
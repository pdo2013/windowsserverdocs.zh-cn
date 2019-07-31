---
title: 安装 Windows Admin Center
description: 如何在 Windows PC 或服务器上安装 Windows 管理中心, 使多个用户可以使用 web 浏览器访问 Windows 管理中心。
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 07/17/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: e67102d1fa8b35d90e97df64cb8bd2991b205ad5
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68658881"
---
# <a name="install-windows-admin-center"></a>安装 Windows Admin Center

> 适用于：Windows Admin Center、Windows Admin Center 预览版

本主题介绍如何在 Windows 电脑或服务器上安装 Windows 管理中心, 使多个用户可以使用 web 浏览器访问 Windows 管理中心。

> [!Tip]
> 不熟悉 Windows Admin Center？
> [了解有关 Windows Admin Center 的更多信息](../understand/windows-admin-center.md)或[立即下载](https://aka.ms/windowsadmincenter)。

## <a name="determine-your-installation-type"></a>确定你的安装类型

查看[安装选项](../plan/installation-options.md), 其中包括[受支持的操作系统](https://docs.microsoft.com/windows-server/manage/windows-admin-center/plan/installation-options#installation-supported-operating-systems)。 若要在 Azure 中的 VM 上安装 Windows 管理中心, 请参阅[在 azure 中部署 Windows 管理中心](../azure/deploy-wac-in-azure.md)。

## <a name="install-on-windows-10"></a>在 Windows 10 上安装

在 Windows 10 上安装 Windows Admin Center 时，默认情况下使用端口 6516，但是你可以选择指定其他端口。 你还可以创建桌面快捷方式，并让 Windows Admin Center 管理你的 TrustedHosts。

> [!NOTE]
> 在工作组环境中或在域中使用本地管理员凭据时需要修改 TrustedHosts。 如果你选择放弃此设置，则必须[手动配置 TrustedHosts](../support/troubleshooting.md#configure-trustedhosts)。

从**开始**菜单中启动 Windows Admin Center 时，它会在默认浏览器中打开。

首次启动 Windows Admin Center 时，你将在桌面的通知区域中看到一个图标。 右键单击此图标，然后选择**打开**以在默认浏览器中打开该工具，或选择**退出**以退出后台进程。

## <a name="install-on-windows-server-with-desktop-experience"></a>在具有桌面体验的 Windows Server 上安装

在 Windows Server 上，系统会以网络服务形式安装 Windows Admin Center。 你必须指定服务侦听的端口，但是，它需要 HTTPS 的证书。 安装程序可以创建自签名证书以进行测试，你也可以提供已在计算机上安装的证书的指纹。 如果你使用生成的证书，那么它将与服务器的 DNS 名称匹配。 如果你使用自己的证书, 请确保证书中提供的名称与计算机名称匹配 (不支持通配符证书。)你还可以选择让 Windows 管理中心管理你的 TrustedHosts。

> [!NOTE]
> 在工作组环境中或在域中使用本地管理员凭据时需要修改 TrustedHosts。 如果你选择放弃此设置，则必须[手动配置 TrustedHosts](../support/troubleshooting.md#configure-trustedhosts)

安装完成后, 从远程计算机打开浏览器并导航到安装程序的最后一步中提供的 URL。

> [!WARNING]
> 自动生成的证书在安装后的 60 天过期。

## <a name="install-on-server-core"></a>在 Server Core 上安装

如果你有 Windows Server 的 Server Core 安装，则可以利用命令提示符（以管理员身份运行）安装 Windows Admin Center。 分别使用 `SME_PORT` 和 `SSL_CERTIFICATE_OPTION` 参数指定端口和 SSL 证书。 如果你要使用现有证书，请使用 `SME_THUMBPRINT` 指定其指纹。

> [!WARNING]
> 安装 Windows 管理中心将重新启动 WinRM 服务, 该服务将切断所有远程 PowerShells 会话。 建议从本地 Cmd 或 PowerShell 安装。 如果正在使用将由 WinRM 服务重启中断的自动化解决方案进行安装, 则可以将参数```RESTART_WINRM=0```添加到安装参数, 但必须重新启动 WinRM, Windows 管理中心才能正常工作。

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

## <a name="upgrading-to-a-new-version-of-windows-admin-center"></a>升级到新版本的 Windows 管理中心

可以使用 Microsoft 更新或通过手动安装来更新 Windows Admin Center 非预览版。

升级到新版本的 Windows 管理中心时, 会保留您的设置。 我们不支持对 Windows 管理中心中心的内幕预览版本进行升级-我们认为更好的做法是进行全新安装-但我们不会将其阻止。

## <a name="updating-the-certificate-used-by-windows-admin-center"></a>更新 Windows 管理中心使用的证书

如果已将 Windows 管理中心部署为服务, 则必须为 HTTPS 提供证书。 若要以后更新此证书, 请重新运行安装程序, 然后选择```change```。

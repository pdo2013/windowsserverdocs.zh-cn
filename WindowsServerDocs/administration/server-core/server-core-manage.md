---
title: 管理服务器核心
description: 了解如何管理 Windows Server 的服务器核心安装
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 6836e5db36727294d215f7f98e0faeede55a612a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869298"
---
# <a name="manage-a-server-core-server"></a>管理服务器核心服务器
 
> 适用于：Windows Server （半年频道） 和 Windows Server 2016

可以按以下方式来管理服务器核心服务器：
- 使用[Windows Admin Center](../../manage/windows-admin-center/overview.md)
- 使用[远程服务器管理工具](../../remote/remote-server-administration-tools.md)Windows 10 上运行
- 使用 Windows PowerShell 本地或远程管理
- 使用远程[服务器管理器](../server-manager/server-manager.md)
- 使用远程[mmc 管理单元](#managing-with-microsoft-management-console)
- 使用远程[远程桌面服务](#managing-with-remote-desktop-services)

您还可以添加硬件和管理本地，驱动程序，只要从命令行执行的操作。

有一些重要限制和提示，请记住，当您使用服务器核心时：

- 如果关闭所有命令提示符窗口并且想要打开新的命令提示符窗口，可以从任务管理器执行的。 按**CTRL\+ALT\+删除**，单击**启动任务管理器**，单击**更多详细信息 > 文件 > 运行**，然后键入**cmd.exe**。 (类型**Powershell.exe**打开 PowerShell 命令窗口。)或者，你可以注销，然后重新登录。
- 任何试图启动 Windows 资源管理器的命令或工具都将无法工作。 例如，运行**开始。** 在命令提示符下，将无法工作。
- 没有支持 HTML 呈现或服务器核心中的 HTML 帮助。
- 服务器核心支持 Windows 安装程序在静默模式下，以便可以从 Windows 安装程序文件安装工具和实用程序。 在 Server Core 上安装 Windows Installer 程序包时, 使用 **/qb**选项显示基本用户界面。
- 若要更改时区，请运行**Set-date**。
- 若要更改国际设置，请运行**control intl.cpl**。
- **Control.exe**不会独立运行。 必须使用运行**Timedate.cpl**或**Intl.cpl**。
- **Winver.exe**在 Server Core 中不可用。 若要获取版本信息可使用**Systeminfo.exe**。

## <a name="managing-server-core-with-windows-admin-center"></a>管理 Windows Admin Center Server Core
[Windows Admin Center](../../manage/windows-admin-center/overview.md) 是基于浏览器的管理应用，可用于本地管理 Windows Server，而无需依赖 Azure 或云。 利用 Windows Admin Center，你可以完全控制服务器基础架构的各个方面，对于在未连接到 Internet 的专用网络上进行管理特别有用。 可以在 Windows 10 上，网关服务器，或带桌面体验的 Windows Server 安装上安装 Windows Admin Center，然后连接到你想要管理的服务器核心系统。

## <a name="managing-server-core-remotely-with-server-manager"></a>管理 Server Core 远程使用服务器管理器中

服务器管理器是可帮助你预配和管理本地和远程基于 Windows 的服务器从你的桌面，而无需物理访问服务器，或者需要启用远程桌面协议 (RDP) 的 Windows Server 中的管理控制台每台服务器连接。 服务器管理器支持远程、 多服务器管理。

若要让在远程服务器上运行的服务器管理器能够管理你的本地服务器，请运行 Windows PowerShell cmdlet **Configure-SMRemoting.exe –Enable**。

## <a name="managing-with-microsoft-management-console"></a>使用 Microsoft 管理控制台管理

您可以远程使用多个管理单元用于 Microsoft 管理控制台 (MMC) 管理服务器核心服务器。

若要使用 mmc 管理单元来管理服务器核心服务器是域成员： 

1. 启动一个 MMC 管理单元，如计算机管理。
2. 管理单元中，右键单击，然后单击**连接到另一台计算机**。
2. 键入服务器核心服务器的计算机名称，然后单击**确定**。 您可以立即使用 MMC 管理单元来管理服务器核心服务器，就像任何其他 PC 或服务器。

若要使用 mmc 管理单元来管理的服务器核心服务器*不*域成员： 

1. 建立替代凭据以用于连接到服务器核心计算机通过远程计算机上的命令提示符下键入以下命令：
   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```
   如果你想要提示输入密码，则忽略 **/pass**选项。

2. 出现提示时，键入您指定的用户名的密码。
   如果服务器核心服务器上的防火墙已未配置为允许 MMC 管理单元连接，请按照以下步骤来配置 Windows 防火墙以允许 mmc 管理单元。 然后继续执行步骤 3。
3. 在另一台计算机上启动一个 MMC 管理单元，如**计算机管理**。
4. 在左窗格中，管理单元中，右键单击，然后单击**连接到另一台计算机**。 (例如，在计算机管理示例中，你右键单击**计算机管理 （本地）**。)
5. 在中**另一台计算机**，键入服务器核心服务器的计算机名称，然后单击**确定**。 你现在可以使用 MMC 管理单元来管理服务器核心服务器，像管理运行 Windows Server 操作系统的任何其他计算机一样。

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>若要将 Windows 防火墙配置为允许连接 MMC 管理单元
若要允许所有 MMC 管理单元连接，请运行以下命令：

```
Enable-NetFirewallRule -DisplayGroup "Remote Administration"
```

若要允许仅特定 MMC 管理单元连接，运行以下命令：
```
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

其中*rulegroup*是的以下内容，具体取决于哪些管理单元中你想要连接之一：

| MMC 管理单元                            | 规则组                                            |
|----------------------------------------|-------------------------------------------------------|
| 事件查看器                           | 远程事件日志管理                           |
| Services                               | 远程服务管理                             |
| 共享文件夹                         | 文件和打印机共享                              |
| 任务计划程序                         | 性能日志和警报、 文件和打印机共享 |
| 磁盘管理                        | 远程卷管理                              |
| Windows 防火墙和高级的安全 | Windows 防火墙远程管理                    |


> [!NOTE] 
> 某些 MMC 管理单元没有允许其通过防火墙连接的相应规则组。 不过，启用事件查看器、服务或共享文件夹的规则组将允许连接大多数其他管理单元。 
>
> 此外，某些管理单元在可以通过 Windows 防火墙连接之前需要进行进一步配置：
>
> - 磁盘管理。 你必须首先在服务器核心计算机上启动虚拟磁盘服务 (VDS)。 你还必须在运行 MMC 管理单元的计算机上正确配置磁盘管理规则。
> - IP 安全监视器。 你必须首先启用该管理单元的远程管理。 为此，请在命令提示符下键入**Cscript \windows\system32\scregedit.wsf /im 1**
> - 可靠性和性能。 此管理单元不需要进行进一步配置，但是当你使用它监视服务器核心计算机时，你可能仅能监视性能数据。 可靠性数据不可用。

## <a name="managing-with-remote-desktop-services"></a>使用远程桌面服务管理

可以使用[远程桌面](../../remote/remote-desktop-services/welcome-to-rds.md)来管理服务器核心服务器从远程计算机。

在可以访问服务器核心之前，你将需要运行以下命令： 
```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```
这可让管理模式的远程桌面接受连接。

## <a name="add-hardware-and-manage-drivers-locally"></a>添加硬件和管理本地驱动程序

若要将硬件添加到服务器核心服务器，请安装新硬件的硬件供应商提供的说明进行操作。 

如果硬件不是插，您将需要手动安装驱动程序。 为此，请将驱动程序文件复制到临时位置的服务器上，并运行以下命令：
```
pnputil –i –a <driverinf>
```
其中*driverinf*是驱动程序的.inf 文件的文件名。

如果出现提示，重新启动计算机。

若要查看安装了哪些驱动程序，请运行以下命令： 
```
sc query type= driver
```

> [!NOTE] 
> 若要命令能成功完成，你必须保留等号后的空格。

若要禁用设备驱动程序，运行以下命令： 
```
sc delete <service_name>
```

其中*service_name*是你在运行时的服务名称**sc 查询类型 = 驱动程序**。
---
title: 管理服务器核心
description: 了解如何管理的 Windows Server 的服务器核心安装
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 6836e5db36727294d215f7f98e0faeede55a612a
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2018
ms.locfileid: "1718707"
---
# <a name="manage-a-server-core-server"></a>管理服务器核心服务器
 
> 适用于： Windows Server （半年通道） 和 Windows Server 2016

您可以通过以下方式来管理的服务器核心服务器：
- 使用[Windows 管理中心](../../manage/windows-admin-center/overview.md)
- 使用 Windows 10 上运行的[远程服务器管理工具](../../remote/remote-server-administration-tools.md)
- 本地和远程使用 Windows PowerShell
- 使用远程[服务器管理器](../server-manager/server-manager.md)
- 使用[MMC 管理单元中](#managing-with-microsoft-management-console)的远程
- 远程通过[远程桌面服务](#managing-with-remote-desktop-services)

此外可以添加硬件和，只要您执行的操作从命令行管理本地，驱动程序。

有一些重要限制和提示需要使用服务器核心时注意以下事项：

- 如果您关闭所有的命令提示符窗口并打开一个新的命令提示符窗口，可以执行的操作从任务管理器。 按**CTRL\ + ALT\ + DELETE**，请单击**启动任务管理器**，请单击**更多详细信息 > 文件 > 运行**，然后键入**cmd.exe**。 （键入**Powershell.exe**打开 windows PowerShell 命令）。或者，您可以先注销并重新登录。
- 将无法运行任意命令或尝试启动 Windows 资源管理器的工具。 例如，运行**开始。** 在命令提示符下不起作用。
- 没有支持 HTML 呈现或在服务器核心的 HTML 帮助。
- 服务器核心支持 Windows Installer 在安静模式下，以便您可以从 Windows Installer 文件安装工具和实用程序。 在服务器核心安装 Windows Installer 程序包时, 使用 **/qb**选项显示基本的用户界面。
- 若要更改时区，运行**设置日期**。
- 若要更改国际设置，请运行**控件 intl.cpl**。
- **Control.exe**不会运行其自己。 您必须运行它与**Timedate.cpl**或**Intl.cpl**。
- 在服务器核心**Winver.exe**不可用。 若要获取版本信息，请使用**Systeminfo.exe**。

## <a name="managing-server-core-with-windows-admin-center"></a>管理与 Windows 管理中心的服务器核心
[Windows Admin Center](../../manage/windows-admin-center/overview.md)是基于浏览器的管理应用程序，允许在本地管理的 Windows 服务器与没有 Azure 或云依赖项。 Windows Admin Center 为您提供了完全控制 server 基础结构的各个方面，未连接到 Internet 的专用网络上的管理特别有用。 您可以在 Windows 10、，网关在服务器上或安装的 Windows Server 桌面体验，请安装 Windows Admin Center，然后连接到您要管理的服务器核心系统。

## <a name="managing-server-core-remotely-with-server-manager"></a>管理服务器核心远程使用服务器管理器中

服务器管理器是可帮助您设置和管理本地和远程基于 Windows 的服务器从您的桌面，而无需是物理访问服务器或需要启用远程桌面协议 (RDP) 的 Windows Server 中管理控制台向每台服务器的连接。 服务器管理器支持远程、 多台服务器的管理。

若要管理服务器管理器的远程服务器上运行您本地服务器，请运行 Windows PowerShell cmdlet**配置 SMRemoting.exe – 启用**。

## <a name="managing-with-microsoft-management-console"></a>使用 Microsoft 管理控制台管理

您可以远程使用许多单元 Microsoft 管理控制台 (MMC) 管理您的服务器核心服务器。

使用 mmc 管理单元来管理是域成员的服务器核心服务器： 

1. 启动 MMC 管理单元中，例如计算机管理。
2. 管理单元，右键单击，然后单击**连接到另一台计算机**。
2. 键入的服务器核心服务器计算机名称，然后单击**确定**。 您现在可以使用 mmc 管理单元来管理的服务器核心服务器，就像 PC 或服务器。

一个 MMC 管理单元用于管理的服务器核心服务器这是*不*是域成员： 

1. 建立备用凭据，用于连接到的服务器核心计算机远程计算机上的命令提示符处键入以下命令：
   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```
   如果您想要提示输入密码，则**忽略/传递**选项。

2. 出现提示时，键入您指定的用户名的密码。
   如果服务器核心服务器上的防火墙没有已配置为允许 MMC 管理单元连接，请按照以下步骤来配置 Windows 防火墙以允许 MMC 管理单元中。 然后继续执行步骤 3。
3. 在另一台计算机上启动 MMC 管理单元中，例如**计算机管理**。
4. 在左窗格中，管理单元，右键单击，然后单击**连接到另一台计算机**。 （例如，在计算机管理示例中，您将右键单击**计算机管理 （本地）**）。
5. **另一台计算机**中, 键入服务器核心服务器的计算机名称，然后单击**确定**。 您现在可以使用 mmc 管理单元来管理的服务器核心服务器，就像运行 Windows Server 操作系统的任何其他计算机。

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>配置 Windows 防火墙以允许 mmc 管理单元 (s) 连接
若要允许所有 MMC 管理单元连接，请运行以下命令：

```
Enable-NetFirewallRule -DisplayGroup "Remote Administration"
```

要允许仅特定 MMC 管理单元连接，请运行以下命令：
```
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

其中*rulegroup*是的以下内容，具体取决于哪些单元您想要连接之一：

| Mmc 管理单元                            | 规则组                                            |
|----------------------------------------|-------------------------------------------------------|
| 事件查看器                           | 远程事件日志管理                           |
| 服务                               | 远程服务管理                             |
| 共享的文件夹                         | 文件和打印机共享                              |
| 任务计划程序                         | 性能日志和警报、 文件和打印机共享 |
| 磁盘管理                        | 远程卷管理                              |
| Windows 防火墙和高级的安全功能 | Windows 防火墙远程管理                    |


> [!NOTE] 
> 某些 mmc 管理单元不具有一个相应的规则组，使其通过防火墙连接。 但是，启用事件查看器、 服务或共享文件夹的规则组将允许大多数其他管理单元连接。 
>
> 此外，某些单元需要进一步配置，才能通过 Windows 防火墙进行连接：
>
> - 磁盘管理。 在服务器核心计算机上，必须首先启动虚拟磁盘服务 (VDS)。 您还必须运行 mmc 管理单元的计算机上正确配置磁盘管理规则。
> - IP 安全监视器。 您必须先启用远程管理的此管理单元。 为此，请在命令提示符处，键入**Cscript \windows\system32\scregedit.wsf /im 1**
> - 可靠性和性能。 管理单元中不需要任何进一步配置，但时用于监视服务器核心计算机，您可以仅监视性能数据。 可靠性数据不可用。

## <a name="managing-with-remote-desktop-services"></a>使用远程桌面服务管理

您可以使用[远程桌面](../../remote/remote-desktop-services/welcome-to-rds.md)从远程计算机管理的服务器核心服务器。

您可以访问服务器核心之前，需要运行以下命令： 
```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```
这样管理模式的远程桌面以接受的连接。

## <a name="add-hardware-and-manage-drivers-locally"></a>添加硬件和管理本地驱动程序

若要添加到的服务器核心服务器的硬件，按照由硬件供应商提供的用于安装新硬件的说明。 

如果硬件不插，您将需要手动安装驱动程序。 为此，驱动程序文件复制到的服务器上，一个临时位置，然后运行以下命令：
```
pnputil –i –a <driverinf>
```
其中*driverinf*是驱动程序的.inf 文件的文件名。

如果出现提示，请重新启动计算机。

若要查看安装哪些驱动程序，请运行以下命令： 
```
sc query type= driver
```

> [!NOTE] 
> 等号命令成功完成后，必须包括的空间。

若要禁用设备驱动程序，请运行以下命令： 
```
sc delete <service_name>
```

*Service_name*所在的运行时将你的服务名称**sc 查询类型 = 驱动程序**。
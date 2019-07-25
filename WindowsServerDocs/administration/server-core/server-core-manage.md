---
title: 管理服务器核心
description: 了解如何管理 Windows Server 的 Server Core 安装
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 07/23/2019
ms.openlocfilehash: bbb04e761dbb1dd48d95e15d11c91608f4d6c240
ms.sourcegitcommit: 216d97ad843d59f12bf0b563b4192b75f66c7742
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476540"
---
# <a name="manage-a-server-core-server"></a>管理服务器核心服务器
 
> 适用于：Windows Server 2019、Windows Server 2016 和 Windows Server (半年频道)

可以通过以下方式管理服务器核心服务器:
- 使用[Windows 管理中心](../../manage/windows-admin-center/overview.md)
- 使用在 Windows 10 上运行的[远程服务器管理工具](../../remote/remote-server-administration-tools.md)
- 使用 Windows PowerShell 本地或远程管理
- 使用[服务器管理器](../server-manager/server-manager.md)远程
- 使用[MMC 管理单元](#managing-with-microsoft-management-console)远程
- 远程处理[远程桌面服务](#managing-with-remote-desktop-services)

你还可以在本地添加硬件和管理驱动程序, 前提是从命令行执行此操作。

使用服务器核心时, 需要牢记一些重要限制和提示:

- 如果你关闭所有命令提示符窗口并想要打开一个新的命令提示符窗口, 则可以从任务管理器中执行此操作。 按**CTRL\+ALT\+DELETE**, 依次单击 "**启动任务管理器**"、"**更多详细信息 > 文件 >" 运行**", 然后键入**cmd.exe**。 (键入 " **powershell** " 以打开 powershell 命令窗口。)或者, 你可以注销, 然后重新登录。
- 任何试图启动 Windows 资源管理器的命令或工具都将无法工作。 例如, 运行**start。** 在命令提示符下不起作用。
- 服务器核心不支持 HTML 呈现或 HTML 帮助。
- 服务器核心在安静模式下支持 Windows Installer, 以便可以从 Windows Installer 文件安装工具和实用工具。 在服务器核心上安装 Windows Installer 包时, 请使用 **/qb**选项显示基本用户界面。
- 若要更改时区, 请运行 "**设置-日期**"。
- 若要更改国际设置, 请运行**控制国际 cpl**。
- 不会自行运行**Control。** 必须用 Timedate.cpl**或** 来运行它。
- **Winver**在 Server Core 中不可用。 若要获取版本信息, 请使用**systeminfo.exe**。

## <a name="managing-server-core-with-windows-admin-center"></a>通过 Windows 管理中心管理服务器核心
[Windows Admin Center](../../manage/windows-admin-center/overview.md) 是基于浏览器的管理应用，可用于本地管理 Windows Server，而无需依赖 Azure 或云。 利用 Windows Admin Center，你可以完全控制服务器基础架构的各个方面，对于在未连接到 Internet 的专用网络上进行管理特别有用。 您可以在 Windows 10、网关服务器或安装有桌面体验的 Windows Server 上安装 Windows 管理中心, 然后连接到要管理的服务器核心系统。

## <a name="managing-server-core-remotely-with-server-manager"></a>通过服务器管理器远程管理服务器核心

服务器管理器是 Windows Server 中的管理控制台, 可帮助你从桌面设置和管理本地和远程基于 Windows 的服务器, 而无需物理访问服务器或启用远程桌面协议 (RDP)与每个服务器的连接。 服务器管理器支持远程多服务器管理。

若要让在远程服务器上运行的服务器管理器能够管理你的本地服务器，请运行 Windows PowerShell cmdlet **Configure-SMRemoting.exe –Enable**。

## <a name="managing-with-microsoft-management-console"></a>通过 Microsoft 管理控制台进行管理

你可以远程使用许多 Microsoft 管理控制台 (MMC) 管理单元来管理你的服务器核心服务器。

使用 MMC 管理单元管理作为域成员的服务器核心服务器: 

1. 启动 MMC 管理单元, 如 "计算机管理"。
2. 右键单击管理单元, 然后单击 "**连接到另一台计算机**"。
2. 键入 Server Core 服务器的计算机名, 然后单击 **"确定"** 。 你现在可以使用 MMC 管理单元来管理服务器核心服务器, 就像管理任何其他电脑或服务器一样。

使用 MMC 管理单元管理*不*是域成员的服务器核心服务器: 

1. 通过在远程计算机上的命令提示符下键入以下命令, 建立用于连接到服务器核心计算机的备用凭据:

   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```

   如果要提示输入密码, 请忽略 **/pass**选项。

2. 出现提示时, 键入指定用户名的密码。
   如果服务器核心服务器上的防火墙尚未配置为允许 MMC 管理单元连接, 请按照以下步骤配置 Windows 防火墙以允许 MMC 管理单元。 然后继续执行步骤3。
3. 在另一台计算机上, 启动 MMC 管理单元, 如 "**计算机管理**"。
4. 在左窗格中, 右键单击管理单元, 然后单击 "**连接到另一台计算机**"。 (例如, 在 "计算机管理" 示例中, 右键单击 "**计算机管理 (本地)** "。)
5. 在 "**另一台计算机**" 中, 键入 server Core 服务器的计算机名, 然后单击 **"确定"** 。 你现在可以使用 MMC 管理单元来管理服务器核心服务器，像管理运行 Windows Server 操作系统的任何其他计算机一样。

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>若要将 Windows 防火墙配置为允许连接 MMC 管理单元
若要允许所有 MMC 管理单元连接, 请运行以下命令:

```PowerShell
Enable-NetFirewallRule -DisplayGroup "Remote Administration"
```

若要仅允许特定 MMC 管理单元连接, 请运行以下内容:

```PowerShell
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

其中*rulegroup*是以下各项之一, 具体取决于要连接的管理单元:

| MMC 管理单元                            | 规则组                                            |
| ---------------------------------------- | ------------------------------------------------------- |
| 事件查看器                           | 远程事件日志管理                           |
| Services                               | 远程服务管理                             |
| 共享文件夹                         | 文件和打印机共享                              |
| 任务计划程序                         | 性能日志和警报、文件和打印机共享 |
| 磁盘管理                        | 远程卷管理                              |
| Windows 防火墙和高级安全性 | Windows 防火墙远程管理                    |


> [!NOTE] 
> 某些 MMC 管理单元没有允许其通过防火墙连接的相应规则组。 不过，启用事件查看器、服务或共享文件夹的规则组将允许连接大多数其他管理单元。 
>
> 此外，某些管理单元在可以通过 Windows 防火墙连接之前需要进行进一步配置：
>
> - 磁盘管理。 你必须首先在服务器核心计算机上启动虚拟磁盘服务 (VDS)。 你还必须在运行 MMC 管理单元的计算机上正确配置磁盘管理规则。
> - IP 安全监视器。 你必须首先启用该管理单元的远程管理。 为此, 请在命令提示符下键入**Cscript \windows\system32\scregedit.wsf/im 1**
> - 可靠性和性能。 此管理单元不需要进行进一步配置，但是当你使用它监视服务器核心计算机时，你可能仅能监视性能数据。 可靠性数据不可用。

## <a name="managing-with-remote-desktop-services"></a>用远程桌面服务管理

可以使用[远程桌面](../../remote/remote-desktop-services/welcome-to-rds.md)从远程计算机管理服务器核心服务器。

你需要运行以下命令, 然后才能访问 Server Core: 

```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```

这可让管理模式的远程桌面接受连接。

## <a name="add-hardware-and-manage-drivers-locally"></a>在本地添加硬件和管理驱动程序

若要将硬件添加到服务器核心服务器, 请按照硬件供应商提供的说明安装新硬件。 

如果硬件不是即插即用, 则需要手动安装驱动程序。 为此, 请将驱动程序文件复制到服务器上的临时位置, 并运行以下命令:

```
pnputil –i –a <driverinf>
```

其中, *driverinf*是驱动程序的 .inf 文件的文件名。

如果出现提示，重新启动计算机。

若要查看已安装的驱动程序, 请运行以下命令: 

```
sc query type= driver
```

> [!NOTE] 
> 若要命令能成功完成，你必须保留等号后的空格。

若要禁用设备驱动程序, 请运行以下内容:

```
sc delete <service_name>
```

其中, *service_name*是在运行**sc query type = driver**时获得的服务的名称。
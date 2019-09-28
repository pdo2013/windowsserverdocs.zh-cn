---
title: 管理 Hyper-v Integration Services
description: 描述如何打开和关闭 integration services 并在需要时进行安装
ms.technology: compute-hyper-v
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.date: 12/20/2016
ms.topic: article
ms.prod: windows-server
ms.service: na
ms.assetid: 9cafd6cb-dbbe-4b91-b26c-dee1c18fd8c2
ms.openlocfilehash: 39d57afbd8c4df78764c5975d4cc3d48848475c1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392773"
---
>适用于：Windows 10、Windows Server 2016、Windows Server 2019

# <a name="manage-hyper-v-integration-services"></a>管理 Hyper-v Integration Services

Hyper-v Integration Services 通过与 Hyper-v 主机进行双向通信来增强虚拟机性能并提供便利功能。 其中许多服务都是很便利（如来宾文件复制），而其他服务则对虚拟机的功能很重要，例如合成设备驱动程序。 这组服务和驱动程序有时称为 "集成组件"。 可以控制每个给定虚拟机的个别便利服务是否正常运行。 驱动程序组件不应被手动提供服务。

有关每个 integration services 的详细信息，请参阅[hyper-v Integration Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services)。

> [!IMPORTANT]
> 必须在主机和来宾中同时启用每个要使用的服务，才能正常运行。 默认情况下，所有集成 Hyper-V 来宾服务接口服务在 Windows 来宾操作系统上都处于启用状态。 可以单独打开和关闭服务。 下一部分介绍了如何操作。

## <a name="turn-an-integration-service-on-or-off-using-hyper-v-manager"></a>使用 Hyper-v 管理器打开或关闭集成服务

1. 在中心窗格中，右键单击虚拟机，然后单击 "**设置**"。
  
2. 从 "**设置**" 窗口的左窗格中的 "**管理**" 下，单击 " **Integration Services**"。
  
"Integration Services" 窗格列出了 Hyper-v 主机上可用的所有集成服务，以及该主机是否已启用虚拟机以使用它们。

### <a name="turn-an-integration-service-on-or-off-using-powershell"></a>使用 PowerShell 启用或禁用集成服务

若要在 PowerShell 中执行此操作，请使用[enable-vmintegrationservice](https://technet.microsoft.com/library/hh848500.aspx)和[enable-vmintegrationservice](https://technet.microsoft.com/library/hh848488.aspx)。

下面的示例演示如何为名为 "demovm" 的虚拟机启用和禁用来宾文件复制集成服务。

1. 获取正在运行的 integration services 的列表：
  
    ``` PowerShell
    Get-VMIntegrationService -VMName "DemoVM"
    ```

1. 输出应如下所示：

    ``` PowerShell
   VMName      Name                    Enabled PrimaryStatusDescription SecondaryStatusDescription
   ------      ----                    ------- ------------------------ --------------------------
   DemoVM      Guest Service Interface False   OK
   DemoVM      Heartbeat               True    OK                       OK
   DemoVM      Key-Value Pair Exchange True    OK
   DemoVM      Shutdown                True    OK
   DemoVM      Time Synchronization    True    OK
   DemoVM      VSS                     True    OK
   ```

1. 启用来宾服务接口：

   ``` PowerShell
   Enable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
   ```

1. 验证是否已启用来宾服务接口：

   ```
   Get-VMIntegrationService -VMName "DemoVM" 
   ``` 

1. 禁用来宾服务接口：

    ```
    Disable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
    ```
   
## <a name="checking-the-guests-integration-services-version"></a>检查来宾的 integration services 版本
某些功能可能无法正常工作，或者如果来宾的集成服务不是最新的，则不能正常工作。 若要获取 Windows 的版本信息，请登录到来宾操作系统，打开命令提示符，并运行以下命令：

```
REG QUERY "HKLM\Software\Microsoft\Virtual Machine\Auto" /v IntegrationServicesVersion
```

较早版本的来宾操作系统将没有所有可用的服务。 例如，Windows Server 2008 R2 来宾不能有 "Hyper-V 来宾服务接口"。

## <a name="start-and-stop-an-integration-service-from-a-windows-guest"></a>启动和停止 Windows 来宾的集成服务
为了使 integration services 能够完全正常运行，除了在主机上启用外，还必须在来宾内运行其相应的服务。 在 Windows 来宾中，每个 integration services 都作为标准 Windows 服务列出。 你可以使用 "控制面板" 或 "PowerShell" 中的 "服务" 小程序来停止和启动这些服务。

> [!IMPORTANT]
> 停止集成服务可能会严重影响主机管理虚拟机的能力。 若要正常工作，必须在主机和来宾上启用要使用的每个 integration services。
> 最佳做法是，仅应使用上述说明从 Hyper-v 控制集成服务。 在 Hyper-v 中更改来宾操作系统中的匹配服务时，该服务将自动停止或启动。
> 如果在来宾操作系统中启动一个服务，但该服务在 Hyper-v 中处于禁用状态，则该服务将停止。 如果在 Hyper-v 中启用了来宾操作系统中的服务，则 Hyper-v 最终将再次启动该服务。 如果在来宾中禁用该服务，Hyper-v 将无法启动该服务。

### <a name="use-windows-services-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>使用 Windows 服务在 Windows 来宾中启动或停止集成服务

1. 通过以管理员身份或通过双击 "控制面板" 中的 "服务" 图标运行 @no__t 来打开服务管理器。

    ![显示 "Windows 服务" 窗格的屏幕截图](media/HVServices.png) 

1. 查找以 "Hyper-v" 开头的服务。 

1. 右键单击要启动或停止的服务。 单击所需的操作。

### <a name="use-windows-powershell-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>使用 Windows PowerShell 在 Windows 来宾中启动或停止集成服务

1. 若要获取 integration services 列表，请运行：

    ```
    Get-Service -Name vm*
    ```

1.  输出应如下所示：

    ```PowerShell
    Status   Name               DisplayName
    ------   ----               -----------
    Running  vmicguestinterface Hyper-V Guest Service Interface
    Running  vmicheartbeat      Hyper-V Heartbeat Service
    Running  vmickvpexchange    Hyper-V Data Exchange Service
    Running  vmicrdv            Hyper-V Remote Desktop Virtualizati...
    Running  vmicshutdown       Hyper-V Guest Shutdown Service
    Running  vmictimesync       Hyper-V Time Synchronization Service
    Stopped  vmicvmsession      Hyper-V VM Session Service
    Running  vmicvss            Hyper-V Volume Shadow Copy Requestor
    ```

1. 运行 "[启动服务](https://technet.microsoft.com/library/hh849825.aspx)" 或 "[停止服务](https://technet.microsoft.com/library/hh849790.aspx)"。 例如，若要关闭 Windows PowerShell Direct，请运行：

    ```
    Stop-Service -Name vmicvmsession
    ```

## <a name="start-and-stop-an-integration-service-from-a-linux-guest"></a>从 Linux 来宾启动和停止集成服务 

Linux 集成服务通常通过 Linux 内核提供。 Linux integration services 驱动程序名为**hv_utils**。

1. 若要确定是否已加载**hv_utils** ，请使用以下命令：

   ``` BASH
   lsmod | grep hv_utils
   ``` 
  
2. 输出应如下所示：  
  
    ``` BASH
    Module                  Size   Used by
    hv_utils               20480   0
    hv_vmbus               61440   8 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
    ```

3. 若要查看所需的守护程序是否正在运行，请使用此命令。
  
    ``` BASH
    ps -ef | grep hv
    ```
  
4. 输出应如下所示： 
  
    ```BASH
    root       236     2  0 Jul11 ?        00:00:00 [hv_vmbus_con]
    root       237     2  0 Jul11 ?        00:00:00 [hv_vmbus_ctl]
    ...
    root       252     2  0 Jul11 ?        00:00:00 [hv_vmbus_ctl]
    root      1286     1  0 Jul11 ?        00:01:11 /usr/lib/linux-tools/3.13.0-32-generic/hv_kvp_daemon
    root      9333     1  0 Oct12 ?        00:00:00 /usr/lib/linux-tools/3.13.0-32-generic/hv_kvp_daemon
    root      9365     1  0 Oct12 ?        00:00:00 /usr/lib/linux-tools/3.13.0-32-generic/hv_vss_daemon
    scooley  43774 43755  0 21:20 pts/0    00:00:00 grep --color=auto hv          
    ```

5. 若要查看哪些守护程序可用，请运行：

    ``` BASH
    compgen -c hv_
    ```
  
6. 输出应如下所示：
  
    ``` BASH
    hv_vss_daemon
    hv_get_dhcp_info
    hv_get_dns_info
    hv_set_ifconfig
    hv_kvp_daemon
    hv_fcopy_daemon     
    ```
  
   可能列出的 Integration services 守护程序包括以下各项。 如果缺少任何这些程序，可能不会在您的系统上受支持，或者它们可能未安装。 查找详细信息，请参阅[Windows 上的 Hyper-v 支持的 Linux 和 FreeBSD 虚拟机](https://technet.microsoft.com/library/dn531030.aspx)。  
   - **hv_vss_daemon**：创建实时 Linux 虚拟机备份需要此守护程序。
   - **hv_kvp_daemon**：此守护程序允许设置和查询内部和外部密钥值对。
   - **hv_fcopy_daemon**：此后台程序在主机和来宾之间实现文件复制服务。  

### <a name="examples"></a>示例

这些示例演示了如何停止和启动 KVP 守护程序，名为 `hv_kvp_daemon`。

1. 使用进程 ID \(PID @ no__t 来停止守护程序的进程。 若要查找 PID，请查看输出的第二列，或使用 `pidof`。 Hyper-v 守护程序作为根运行，所以需要根权限。

    ``` BASH
    sudo kill -15 `pidof hv_kvp_daemon`
    ```

1. 若要验证所有 `hv_kvp_daemon` 进程是否已消失，请运行：

    ```
    ps -ef | hv
    ```

1. 若要重新启动该守护程序，请将守护程序作为根运行：

    ``` BASH
    sudo hv_kvp_daemon
    ``` 

1. 若要验证是否使用新的进程 ID 列出了 `hv_kvp_daemon` 进程，请运行：

    ```
    ps -ef | hv
    ```

## <a name="keep-integration-services-up-to-date"></a>使 integration services 保持最新

建议使 integration services 保持最新状态，以获取虚拟机的最佳性能和最新功能。 默认情况下，如果这些来宾设置为从 Windows 更新获取重要更新，则默认情况下会发生这种情况。 当你更新内核时，使用当前内核的 Linux 来宾将接收到最新的集成组件。

**对于在 Windows 10 主机上运行的虚拟机：**

> [!NOTE]
> Windows 10 上的 Hyper-v 不附带映像文件 vmguest.iso，因为它不再需要。

| Guest  | 更新机制 | 说明 |
|:---------|:---------|:---------|
| Windows 10 | Windows 更新 | |
| Windows 8.1 | Windows 更新 | |
| Windows 8 | Windows 更新 | 需要“数据交换”集成服务。* |
| Windows 7 | Windows 更新 | 需要“数据交换”集成服务。* |
| Windows Vista (SP 2) | Windows 更新 | 需要“数据交换”集成服务。* |
| - | | |
| Windows Server 2016 | Windows 更新 | |
| Windows Server 半年频道 | Windows 更新 | |
| Windows Server 2012 R2 | Windows 更新 | |
| Windows Server 2012 | Windows 更新 | 需要“数据交换”集成服务。* |
| Windows Server 2008 R2 (SP 1) | Windows 更新 | 需要“数据交换”集成服务。* |
| Windows Server 2008 (SP 2) | Windows 更新 | 仅在 Windows Server 2016 中提供扩展支持（[阅读详细信息](https://support.microsoft.com/lifecycle?p1=12925)）。 |
| Windows Home Server 2011 | Windows 更新 | 在 Windows Server 2016 中将不受支持（[阅读详细信息](https://support.microsoft.com/lifecycle?p1=15820)）。 |
| Windows Small Business Server 2011 | Windows 更新 | 非主要支持（[阅读更多](https://support.microsoft.com/lifecycle?p1=15817)）。 |
| - | | |
| Linux 来宾 | 程序包管理器 | Linux 集成服务内置于发行版中，但可能有可选的更新可用。 ******** |

\* 如果无法启用数据交换集成服务，这些来宾的集成服务将从[下载中心](https://support.microsoft.com/kb/3071740)作为 cabinet （cab）文件提供。 此[博客文章](https://blogs.technet.com/b/virtualization/archive/2015/07/24/integration-components-available-for-virtual-machines-not-connected-to-windows-update.aspx)提供了应用 cab 的说明。

**对于在 Windows 8.1 主机上运行的虚拟机：**

| Guest  | 更新机制 | 说明 |
|:---------|:---------|:---------|
| Windows 10 | Windows 更新 | |
| Windows 8.1 | Windows 更新 | |
| Windows 8 | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| Windows 7 | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| Windows Vista (SP 2) | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| Windows XP（SP 2、SP 3） | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| - | | |
| Windows Server 2016 | Windows 更新 | |
| Windows Server 半年频道 | Windows 更新 | |
| Windows Server 2012 R2 | Windows 更新 | |
| Windows Server 2012 | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| Windows Server 2008 R2 | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| Windows Server 2008 (SP 2) | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| Windows Home Server 2011 | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| Windows Small Business Server 2011 | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| Windows Server 2003 R2 (SP 2) | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| Windows Server 2003 (SP 2) | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| - | | |
| Linux 来宾 | 程序包管理器 | Linux 集成服务内置于发行版中，但可能有可选的更新可用。 ** |


**对于在 Windows 8 主机上运行的虚拟机：**

| Guest  | 更新机制 | 说明 |
|:---------|:---------|:---------|
| Windows 8.1 | Windows 更新 | |
| Windows 8 | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| Windows 7 | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| Windows Vista (SP 2) | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| Windows XP（SP 2、SP 3） | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| - | | |
| Windows Server 2012 R2 | Windows 更新 | |
| Windows Server 2012 | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| Windows Server 2008 R2 | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。|
| Windows Server 2008 (SP 2) | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| Windows Home Server 2011 | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| Windows Small Business Server 2011 | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| Windows Server 2003 R2 (SP 2) | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| Windows Server 2003 (SP 2) | 集成服务磁盘 | 请参阅下面的[说明](#install-or-update-integration-services)。 |
| - | | |
| Linux 来宾 | 程序包管理器 | Linux 集成服务内置于发行版中，但可能有可选的更新可用。 ** |

有关 Linux 来宾的详细信息，请参阅[Windows 上的 Hyper-v 支持的 Linux 和 FreeBSD 虚拟机](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows)。

## <a name="install-or-update-integration-services"></a>安装或更新 integration services

对于早于 Windows Server 2016 和 Windows 10 的主机，你将需要在来宾操作系统中手动安装或更新 integration services。 
  
1.  打开 Hyper-V 管理器。 在服务器管理器的 "工具" 菜单中，单击 " **Hyper-v 管理器**"。  
  
2.  连接到虚拟机。 右键单击该虚拟机，然后单击 "**连接**"。  
  
3.  在虚拟机连接的“操作”菜单中，单击 **“插入集成服务安装盘”** . 该操作将在虚拟 DVD 驱动器中加载安装盘。 根据来宾操作系统的不同，您可能需要手动启动安装。  
  
4.  安装完成后，所有集成服务均可使用。

对于联机虚拟机，无法在 Windows PowerShell 会话中自动执行或执行这些步骤。 可以将其应用于脱机 VHDX 映像;[请参阅此博客文章](https://blogs.technet.microsoft.com/virtualization/2013/04/18/how-to-install-integration-services-when-the-virtual-machine-is-not-running/)。

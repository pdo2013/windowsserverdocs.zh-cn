---
title: 管理的 HYPER-V 集成服务
description: 介绍如何启用和禁用集成服务，并根据需要进行安装
ms.technology: compute-hyper-v
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.date: 12/20/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.service: na
ms.assetid: 9cafd6cb-dbbe-4b91-b26c-dee1c18fd8c2
ms.openlocfilehash: e2c14e471abb9af7a9182100969a8dd94a17205a
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812202"
---
>适用于：Windows 10，Windows Server 2016 中，Windows Server 2019

# <a name="manage-hyper-v-integration-services"></a>管理的 HYPER-V 集成服务

HYPER-V 集成服务增强虚拟机性能，并通过利用与 HYPER-V 主机的双向通信提供便利功能。 其中很多服务都很便利，例如来宾文件副本，而有些则是重要的虚拟机的功能，例如，合成设备驱动程序。 这一组服务和驱动程序有时称为"集成组件"。 您可以控制各个方便服务运行的任何给定的虚拟机。 驱动程序组件不应手动提供服务。

有关每个集成服务的详细信息，请参阅[HYPER-V Integration Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services)。

> [!IMPORTANT]
> 你想要使用每个的服务必须在主机和来宾中启用才能正常。 除"HYPER-V 来宾服务接口"的所有集成服务上都的 Windows 来宾操作系统上的默认值。 服务可以打开和关闭单独。 接下来的部分介绍如何操作。

## <a name="turn-an-integration-service-on-or-off-using-hyper-v-manager"></a>打开或关闭使用 Hyper-v 管理器启用的集成服务

1. 在中心窗格中，右键单击虚拟机，然后单击**设置**。
  
2. 从左窗格**设置**窗口下**管理**，单击**Integration Services**。
  
集成服务窗格列出了所有可用的 HYPER-V 主机上的 integration services 以及主机是否已启用虚拟机能够使用它们。

### <a name="turn-an-integration-service-on-or-off-using-powershell"></a>打开或关闭使用 PowerShell 启用集成服务

若要在 PowerShell 中执行此操作，请使用[Enable-vmintegrationservice](https://technet.microsoft.com/library/hh848500.aspx)并[Disable-vmintegrationservice](https://technet.microsoft.com/library/hh848488.aspx)。

下面的示例演示启用来宾文件副本集成服务打开和关闭名为"demovm"虚拟机。

1. 获取正在运行的集成服务的列表：
  
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

1. 验证已启用来宾服务接口：

   ```
   Get-VMIntegrationService -VMName "DemoVM" 
   ``` 

1. 关闭来宾服务接口：

    ```
    Disable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
    ```
   
## <a name="checking-the-guests-integration-services-version"></a>正在检查来宾的集成服务版本
某些功能可能无法正确或者根本如果来宾的集成服务不是最新。 若要获取版本信息的 Windows，登录到来宾操作系统中，打开命令提示符，并运行此命令：

```
REG QUERY "HKLM\Software\Microsoft\Virtual Machine\Auto" /v IntegrationServicesVersion
```

早期版本的来宾操作系统将不具有所有可用的服务。 例如，Windows Server 2008 R2 来宾不能具有"HYPER-V 来宾服务接口"。

## <a name="start-and-stop-an-integration-service-from-a-windows-guest"></a>启动和停止从 Windows 来宾的集成服务
为了使完全正常工作的集成服务，其相应的服务必须运行除主机上启用来宾内。 在 Windows 来宾，每个集成服务被列为标准的 Windows 服务。 可以使用 PowerShell 或控制面板中的服务小程序来停止和启动这些服务。

> [!IMPORTANT]
> 停止的集成服务可能会严重影响主机的功能来管理你的虚拟机。 若要正常工作，必须在主机和来宾上启用你想要使用每个集成服务。
> 最佳做法是，您应仅控制使用上面的说明 HYPER-V 的集成服务。 来宾操作系统中匹配的服务将停止或更改其状态的 HYPER-V 中时自动启动。
> 如果来宾操作系统中启动服务，但已禁用的 HYPER-V 中，将停止该服务。 如果停止服务中的 HYPER-V 启用了来宾操作系统中，HYPER-V 将最终再次启动它。 如果禁用来宾中的服务时，HYPER-V 将无法启动它。

### <a name="use-windows-services-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>使用 Windows 服务启动或停止 Windows 来宾中的集成服务

1. 通过运行打开服务管理器```services.msc```以管理员身份或通过双击控制面板中的服务图标。

    ![屏幕截图，显示 Windows 服务窗格](media/HVServices.png) 

1. 查找以"HYPER-V"开头的服务。 

1. 右键单击要启动或停止的服务。 单击所需的操作。

### <a name="use-windows-powershell-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>使用 Windows PowerShell 来启动或停止 Windows 来宾中的集成服务

1. 若要获取的集成服务的列表，请运行：

    ```
    Get-Service -Name vm*
    ```

1.  输出应类似于此：

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

1. 请运行[Start-service](https://technet.microsoft.com/library/hh849825.aspx)或[停止服务](https://technet.microsoft.com/library/hh849790.aspx)。 例如，若要关闭 Windows PowerShell Direct，请运行：

    ```
    Stop-Service -Name vmicvmsession
    ```

## <a name="start-and-stop-an-integration-service-from-a-linux-guest"></a>启动和停止从 Linux 来宾的集成服务 

Linux 集成服务通常通过 Linux 内核提供。 Linux 集成服务驱动程序名为**hv_utils**。

1. 若要查明**hv_utils**加载时，使用以下命令：

   ``` BASH
   lsmod | grep hv_utils
   ``` 
  
2. 输出应类似于此：  
  
    ``` BASH
    Module                  Size   Used by
    hv_utils               20480   0
    hv_vmbus               61440   8 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
    ```

3. 若要查找所需的守护程序是否正在运行，请使用此命令。
  
    ``` BASH
    ps -ef | grep hv
    ```
  
4. 输出应类似于此： 
  
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
  
6. 输出应类似于此：
  
    ``` BASH
    hv_vss_daemon
    hv_get_dhcp_info
    hv_get_dns_info
    hv_set_ifconfig
    hv_kvp_daemon
    hv_fcopy_daemon     
    ```
  
   集成服务守护程序，可能会列出包括以下内容。 如果缺少任一元素，它们可能不支持你的系统上，或可能未安装它们。 查找详细信息，请参阅[Windows 上的 HYPER-V 的支持的 Linux 和 FreeBSD 虚拟机](https://technet.microsoft.com/library/dn531030.aspx)。  
   - **hv_vss_daemon**:创建实时 Linux 虚拟机备份需要此守护程序。
   - **hv_kvp_daemon**:此守护程序允许设置和查询内部和外部键/值对。
   - **hv_fcopy_daemon**:此守护程序实现的文件复制到主机和来宾之间的服务。  

### <a name="examples"></a>示例

这些示例演示如何停止和启动 KVP 守护程序，名为`hv_kvp_daemon`。

1. 使用进程 ID \(PID\)停止守护程序的进程。 若要查找的 PID，查看输出中，第二个列，或使用`pidof`。 HYPER-V 守护程序运行以 root 身份，因此你将需要根权限。

    ``` BASH
    sudo kill -15 `pidof hv_kvp_daemon`
    ```

1. 若要验证是否所有`hv_kvp_daemon`进程都将消失，运行：

    ```
    ps -ef | hv
    ```

1. 若要再次启动该守护程序，请以 root 身份运行该守护程序：

    ``` BASH
    sudo hv_kvp_daemon
    ``` 

1. 若要验证的`hv_kvp_daemon`进程列出新的进程 id，运行：

    ```
    ps -ef | hv
    ```

## <a name="keep-integration-services-up-to-date"></a>保持最新集成服务

我们建议你将集成服务保持最新状态，以获取最佳性能和最新功能的虚拟机。 发生这种情况对于大多数 Windows 来宾默认情况下如果它们已进行设置以从 Windows Update 中获取重要更新。 更新内核时，使用当前内核的 Linux 来宾会收到最新的集成组件。

**对于在 Windows 10 主机上运行的虚拟机：**

> [!NOTE]
> 由于不再需要时，图像文件 vmguest.iso 不包含在 Windows 10 上的 HYPER-V。

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
| Windows Server 2008 (SP 2) | Windows 更新 | 扩展仅在 Windows Server 2016 中的支持 ([阅读更多](https://support.microsoft.com/lifecycle?p1=12925))。 |
| Windows Home Server 2011 | Windows 更新 | 将不支持在 Windows Server 2016 ([阅读更多](https://support.microsoft.com/lifecycle?p1=15820))。 |
| Windows Small Business Server 2011 | Windows 更新 | 非主要支持（[阅读更多](https://support.microsoft.com/lifecycle?p1=15817)）。 |
| - | | |
| Linux 来宾 | 程序包管理器 | 适用于 Linux 集成服务内置于发行版，但可能有可选的更新可用。 ******** |

\* 如果无法启用数据交换集成服务，这些来宾的集成服务的可从[下载中心](https://support.microsoft.com/kb/3071740)作为 cabinet (cab) 文件。 在此提供了有关应用 cab 的说明[博客文章](https://blogs.technet.com/b/virtualization/archive/2015/07/24/integration-components-available-for-virtual-machines-not-connected-to-windows-update.aspx)。

**对于在 Windows 8.1 主机上运行的虚拟机：**

| Guest  | 更新机制 | 说明 |
|:---------|:---------|:---------|
| Windows 10 | Windows 更新 | |
| Windows 8.1 | Windows 更新 | |
| Windows 8 | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| Windows 7 | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| Windows Vista (SP 2) | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| Windows XP（SP 2、SP 3） | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| - | | |
| Windows Server 2016 | Windows 更新 | |
| Windows Server 半年频道 | Windows 更新 | |
| Windows Server 2012 R2 | Windows 更新 | |
| Windows Server 2012 | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| Windows Server 2008 R2 | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| Windows Server 2008 (SP 2) | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| Windows Home Server 2011 | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| Windows Small Business Server 2011 | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| Windows Server 2003 R2 (SP 2) | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| Windows Server 2003 (SP 2) | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| - | | |
| Linux 来宾 | 程序包管理器 | 适用于 Linux 集成服务内置于发行版，但可能有可选的更新可用。 ** |


**对于在 Windows 8 主机上运行的虚拟机：**

| Guest  | 更新机制 | 说明 |
|:---------|:---------|:---------|
| Windows 8.1 | Windows 更新 | |
| Windows 8 | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| Windows 7 | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| Windows Vista (SP 2) | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| Windows XP（SP 2、SP 3） | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| - | | |
| Windows Server 2012 R2 | Windows 更新 | |
| Windows Server 2012 | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| Windows Server 2008 R2 | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。|
| Windows Server 2008 (SP 2) | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| Windows Home Server 2011 | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| Windows Small Business Server 2011 | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| Windows Server 2003 R2 (SP 2) | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| Windows Server 2003 (SP 2) | 集成服务磁盘 | 请参阅[说明](#install-or-update-integration-services)下文。 |
| - | | |
| Linux 来宾 | 程序包管理器 | 适用于 Linux 集成服务内置于发行版，但可能有可选的更新可用。 ** |

有关 Linux 来宾的更多详细信息，请参阅[Windows 上的 HYPER-V 的支持的 Linux 和 FreeBSD 虚拟机](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows)。

## <a name="install-or-update-integration-services"></a>安装或更新集成服务

主机在早于 Windows Server 2016 和 Windows 10 中，你将需要手动安装或更新的来宾操作系统中的集成服务。 
  
1.  打开 Hyper-V 管理器。 从工具菜单的服务器管理器中，单击**Hyper-v 管理器**。  
  
2.  连接到虚拟机。 右键单击虚拟机，然后单击**Connect**。  
  
3.  在虚拟机连接的“操作”菜单中，单击 **“插入集成服务安装盘”** . 该操作将在虚拟 DVD 驱动器中加载安装盘。 根据来宾操作系统，可能需要手动启动安装。  
  
4.  安装完成后，所有集成服务均可使用。

不能自动或联机虚拟机在 Windows PowerShell 会话中完成这些步骤。 可以将它们应用于脱机 VHDX 映像;[请参阅此博客文章](https://blogs.technet.microsoft.com/virtualization/2013/04/18/how-to-install-integration-services-when-the-virtual-machine-is-not-running/)。

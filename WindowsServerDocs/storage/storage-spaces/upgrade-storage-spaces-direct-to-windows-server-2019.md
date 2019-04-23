---
title: 存储空间直通群集升级到 Windows Server 2019
description: 如何升级到 Windows Server 2019-或者同时运行的 Vm，或者它们正在停止时的存储空间直通群集。
author: robhindman
ms.author: robhind
manager: eldenc
ms.date: 03/06/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-spaces
ms.openlocfilehash: 107ccfb5bf9ed3d71b076f3fa66a4f337380a607
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853778"
---
# <a name="upgrade-a-storage-spaces-direct-cluster-to-windows-server-2019"></a>存储空间直通群集升级到 Windows Server 2019

本主题介绍如何将存储空间直通群集升级到 Windows Server 2019。 将 Windows Server 2016 中的存储空间直通群集升级到 Windows Server 2019，使用有四种[群集操作系统滚动升级过程](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)— 两个涉及保持运行的 Vm 和涉及的两个正在停止所有 Vm。
每种方法具有不同的优势和劣势，因此选择一个最适合你组织的需求：

-   **就地升级时虚拟机正在运行**群集中每个服务器上，此选项会产生任何 VM 的停机时间，但你将需要等待存储作业完成后升级每个服务器 （镜像修复）。

-   **当 Vm 正在运行时清理操作系统安装**群集中每个服务器上，此选项会产生任何 VM 的停机时间，但需要等待每个服务器和所有存储作业完成后升级每个服务器，并且您将必须设置 （镜像修复）其应用程序并再次角色。

-   **就地升级时停止 Vm**群集中每个服务器上，此选项会导致 VM 停机，但无需等待存储作业 （镜像修复），所以速度快一些。

-   **清理操作系统安装期间，Vm 会停止**群集中每个服务器上，此选项会导致 VM 停机，但无需等待存储作业 （镜像修复），所以速度快一些。

## <a name="prerequisites-and-limitations"></a>先决条件和限制

然后继续进行升级：

-   检查有可用的备份，以防在升级过程中不存在任何问题。

-   检查硬件供应商具有 BIOS、 固件和驱动程序，它们将在 Windows Server 2019 上支持的服务器。

有升级过程需要注意的一些限制：

-   若要启用存储空间直通的 Windows Server 2019 版本早于 176693.292，客户可能需要联系 Microsoft 支持以启用存储空间直通和软件定义的网络功能的注册表项。 有关详细信息，请参阅 Microsoft 知识库[一文 4464776](https://support.microsoft.com/help/4464776/software-defined-data-center-and-software-defined-networking)。

-   在升级时使用 ReFS 卷的群集，有一些限制：

-   ReFS 卷上完全支持升级，但是，升级后的卷不会受益于 ReFS 中 Windows Server 2019 的增强功能。 这些好处，例如镜像加速奇偶校验、 提高性能要求新创建的 Windows Server 2019 ReFS 卷。 换而言之，你将必须创建新卷使用`New-Volume`cmdlet 或服务器管理器。 下面是一些新的卷会得到的 ReFS 增强功能：

    -   **映射绕过日志**: ReFS，仅适用于群集 （存储空间直通） 系统且不能应用于独立的存储池的性能改善。

    -   **压缩**效率改进 Windows Server 2019 中的特定于多个复原卷

-   在升级之前的 Windows Server 2016 存储空间直通的群集服务器，我们建议将服务器放入存储维护模式。 有关详细信息，请参阅事件 5120 一部分[排查存储空间直通](troubleshooting-storage-spaces.md#event-5120-with-statusiotimeout-c00000b5)。
    虽然在 Windows Server 2016 中已修复此问题，我们建议将每个存储空间直通服务器置于存储维护模式在升级过程作为最佳做法。

-   没有使用 SET 开关的软件定义的网络环境的已知的问题。 此问题涉及的 HYPER-V VM 从 Windows Server 2019 到 Windows Server 2016 （实时迁移到早期版本的操作系统） 的实时迁移。 若要确保成功的实时迁移，我们建议更改正在实时迁移从 Windows Server 2019 到 Windows Server 2016 的 Vm 上的 VM 网络设置。 2019-01 D 修补程序汇总包中的 Windows Server 2019 修复此问题，也称为构建 17763.292。 有关详细信息，请参阅 Microsoft 知识库[一文 4476976](https://support.microsoft.com/help/4476976/windows-10-update-kb4476976)。

由于更高版本的已知问题，某些客户可能会考虑构建新的 Windows Server 2019 群集并将数据复制从旧群集，而不是升级其 Windows Server 2016 群集使用了如下所述的四个过程之一。

## <a name="performing-an-in-place-upgrade-while-vms-are-running"></a>当 Vm 正在运行时执行就地升级

此选项会产生任何 VM 的停机时间，但你将需要等待存储作业完成后升级每个服务器 （镜像修复）。 尽管各个服务器将按顺序重启在升级过程中，该群集，以及所有 Vm 中的其余服务器将保持运行状态。

1.  检查群集中的所有服务器已都安装最新的 Windows 更新。  
    有关详细信息，请参阅[Windows Server 2016 和 Windows 10 更新历史记录](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。
    至少，安装 Microsoft 知识库文章[一文 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (2019 年 2 月 19 日)。 生成号 (请参阅`ver`命令) 应为 14393.2828 或更高版本。

2.  如果你使用软件定义的网络使用组开关，打开提升的 PowerShell 会话并运行以下命令以禁用在群集上的所有 Vm 的 VM 实时迁移的验证检查：

    ```PowerShell
    Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
    Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
    ```

1.  一次，一台群集服务器上执行以下步骤：

    1.  使用 HYPER-V 虚拟机实时迁移移动正在运行的 Vm 关闭你将要升级的服务器。

    2.  暂停使用以下 PowerShell 命令在群集服务器，请注意，隐藏某些内部组。  
    我们建议执行此步骤应时刻注意，如果你没有已处于活动状态迁移 Vm 关闭，因此你可以跳过上一步您的喜好而定，此 cmdlet 将执行操作的服务器。

         ```PowerShell
        Suspend-ClusterNode -Drain
        ```

    3.  将服务器放入存储维护模式中，通过运行以下 PowerShell 命令：

        ```PowerShell
        Get-StorageFaultDomain -type StorageScaleUnit | 
        Where FriendlyName -Eq <ServerName> | 
        Enable-StorageMaintenanceMode
        ```

    2.  运行以下 cmdlet 来检查是否该**OperationalStatus**值是**处于维护模式**:

        ```PowerShell
        Get-PhysicalDisk
        ```

    3.  通过运行在服务器上执行升级安装的 Windows Server 2019 **setup.exe**并使用"保留个人文件和应用程序"选项。  
    安装完成后，该服务器将保持在群集和群集服务自动启动。

    4.  检查新升级的服务器具有最新的 Windows Server 2019 更新。  
    有关详细信息，请参阅[Windows 10 和 Windows Server 2019 更新历史记录](https://support.microsoft.com/help/4464619/windows-10-update-history)。
    生成号 (请参阅`ver`命令) 应为 17763.292 或更高版本。

    5.  使用以下 PowerShell 命令从存储维护模式中删除服务器：

        ```PowerShell
        Get-StorageFaultDomain -type StorageScaleUnit | 
        Where FriendlyName -Eq <ServerName> | 
        Disable-StorageMaintenanceMode
        ```

    1.  使用以下 PowerShell 命令来恢复服务器：

        ```PowerShell
        Resume-ClusterNode
        ```

    1.  等待存储修复作业完成并返回到正常状态的所有磁盘。  
    在服务器升级过程中运行，这可能需要相当长的时间，具体取决于 Vm 的数目。 下面是要运行的命令：

        ```PowerShell
        Get-StorageJob
        Get-VirtualDisk
        ```

2.  升级群集中的下一个服务器。

3.  所有服务器均已都升级到 Windows Server 2019 后，使用以下 PowerShell cmdlet 来更新群集功能级别。
    
    ```PowerShell
    Update-ClusterFunctionalLevel
    ```

    >   [!NOTE]
    >   我们建议尽快，更新群集功能级别从技术上讲具有最多四个星期来执行此操作也是如此。

1.  更新群集功能级别后，使用以下 cmdlet 更新存储池。  
    此时，新 cmdlet，如`Get-ClusterPerf`将是群集中的任何服务器上完全正常运行。

    ```PowerShell
    Update-StoragePool
    ```

2.  选择通过停止每个 VM，在升级虚拟机配置级别使用`Update-VMVersion`cmdlet，然后重新启动 Vm。

3.  如果你要使用软件定义的网络集开关，并且已禁用的 VM 实时迁移检查按照到更高版本，使用以下 cmdlet 重新启用实时 VM 验证检查：

    ```PowerShell
    Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
    Set-ClusterParameter  SkipMigrationDestinationCheck -Value 0
    ```

1.  验证已升级的群集可以按照预期方式。  
    角色应正确的故障转移，如果在群集上使用 VM 实时迁移，则应已成功实时迁移虚拟机。

2.  通过运行群集验证来验证群集 (`Test-Cluster`) 并检查群集验证报告。

## <a name="performing-a-clean-os-installation-while-vms-are-running"></a>当 Vm 正在运行时执行干净操作系统安装

此选项会产生任何 VM 的停机时间，但你将需要等待存储作业完成后升级每个服务器 （镜像修复）。 尽管各个服务器将按顺序重启在升级过程中，该群集，以及所有 Vm 中的其余服务器将保持运行状态。

1.  检查群集中的所有服务器都运行最新的更新。  
    有关详细信息，请参阅[Windows Server 2016 和 Windows 10 更新历史记录](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。
    至少，安装 Microsoft 知识库文章[一文 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (2019 年 2 月 19 日)。 生成号 (请参阅`ver`命令) 应为 14393.2828 或更高版本。

2.  如果你使用软件定义的网络使用组开关，打开提升的 PowerShell 会话并运行以下命令以禁用在群集上的所有 Vm 的 VM 实时迁移的验证检查：

    ```PowerShell
    Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
    Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
    ```

3.  一次，一台群集服务器上执行以下步骤：

    1.  使用 HYPER-V 虚拟机实时迁移移动正在运行的 Vm 关闭你将要升级的服务器。

    2.  暂停使用以下 PowerShell 命令在群集服务器，请注意，隐藏某些内部组。  
    我们建议执行此步骤应时刻注意，如果你没有已处于活动状态迁移 Vm 关闭，因此你可以跳过上一步您的喜好而定，此 cmdlet 将执行操作的服务器。

         ```PowerShell
        Suspend-ClusterNode -Drain
        ```

    3.  将服务器放入存储维护模式中，通过运行以下 PowerShell 命令：

        ```PowerShell
        Get-StorageFaultDomain -type StorageScaleUnit | 
        Where FriendlyName -Eq <ServerName> | 
        Enable-StorageMaintenanceMode
        ```

    4.  运行以下 cmdlet 来检查是否该**OperationalStatus**值是**处于维护模式**:

        ```PowerShell
        Get-PhysicalDisk
        ```

    3.  通过运行以下 PowerShell 命令中收回从群集的服务器：  

        ```PowerShell
        Remove-ClusterNode <ServerName>
        ```

    4.  在服务器上执行全新安装的 Windows Server 2019： 格式在系统驱动器，运行**setup.exe** ，并使用"空"选项。  
    您必须配置的服务器标识、 角色、 功能和安装程序完成后的应用程序和服务器重新启动。

    5.  在服务器上安装 HYPER-V 角色和故障转移群集功能 (可以使用`Install-WindowsFeature`cmdlet)。

    6.  安装最新存储和网络硬件驱动程序批准用于存储空间直通服务器制造商提供的。

    7.  检查新升级的服务器具有最新的 Windows Server 2019 更新。  
    有关详细信息，请参阅[Windows 10 和 Windows Server 2019 更新历史记录](https://support.microsoft.com/help/4464619/windows-10-update-history)。
    生成号 (请参阅`ver`命令) 应为 17763.292 或更高版本。

    8.  使用以下 PowerShell 命令重新加入到群集的服务器：

        ```PowerShell
        Add-ClusterNode
        ```

    9.  使用以下 PowerShell 命令从存储维护模式中删除服务器：

        ```PowerShell
        Get-StorageFaultDomain -type StorageScaleUnit | 
        Where FriendlyName -Eq <ServerName> | 
        Disable-StorageMaintenanceMode
        ```

    10.  等待存储修复作业完成并返回到正常状态的所有磁盘。  
    在服务器升级过程中运行，这可能需要相当长的时间，具体取决于 Vm 的数目。 下面是要运行的命令：

         ```PowerShell
         Get-StorageJob
         Get-VirtualDisk
         ```

4.  升级群集中的下一个服务器。

5.  所有服务器均已都升级到 Windows Server 2019 后，使用以下 PowerShell cmdlet 来更新群集功能级别。
    
    ```PowerShell
    Update-ClusterFunctionalLevel
    ```

    >   [!NOTE]
    >   我们建议尽快，更新群集功能级别从技术上讲具有最多四个星期来执行此操作也是如此。

6.  更新群集功能级别后，使用以下 cmdlet 更新存储池。  
    此时，新 cmdlet，如`Get-ClusterPerf`将是群集中的任何服务器上完全正常运行。

    ```PowerShell
    Update-StoragePool
    ```

7.  （可选） 通过停止每个 VM 并使用升级虚拟机配置级别`Update-VMVersion`cmdlet，然后重新启动 Vm。

8.  如果你要使用软件定义的网络集开关，并且已禁用的 VM 实时迁移检查按照到更高版本，使用以下 cmdlet 重新启用实时 VM 验证检查：

    ```PowerShell
    Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" | 
    Set-ClusterParameter SkipMigrationDestinationCheck -Value 0
    ```

9.  验证已升级的群集可以按照预期方式。  
    角色应正确的故障转移，如果在群集上使用 VM 实时迁移，则应已成功实时迁移虚拟机。

10.  通过运行群集验证来验证群集 (`Test-Cluster`) 并检查群集验证报告。

## <a name="performing-an-in-place-upgrade-while-vms-are-stopped"></a>执行就地升级时停止 Vm

此选项会导致 VM 停机，但可能需要较少的时间比如果保留在升级过程中运行，因为无需等待存储作业完成后升级每个服务器 （镜像修复） 的 Vm。 尽管各个服务器将按顺序重启在升级过程中，群集中剩余的服务器保持运行状态。

1.  检查群集中的所有服务器都运行最新的更新。  
    有关详细信息，请参阅[Windows Server 2016 和 Windows 10 更新历史记录](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。
    至少，安装 Microsoft 知识库文章[一文 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (2019 年 2 月 19 日)。 生成号 (请参阅`ver`命令) 应为 14393.2828 或更高版本。

2.  停止群集上运行的 Vm。

3.  一次，一台群集服务器上执行以下步骤：

    1.  暂停使用以下 PowerShell 命令在群集服务器，请注意，隐藏某些内部组。  
    我们建议执行此步骤要谨慎。

         ```PowerShell
        Suspend-ClusterNode -Drain
        ```

    2.  将服务器放入存储维护模式中，通过运行以下 PowerShell 命令：

        ```PowerShell
        Get-StorageFaultDomain -type StorageScaleUnit | 
        Where FriendlyName -Eq <ServerName> | 
        Enable-StorageMaintenanceMode
        ```

    3.  运行以下 cmdlet 来检查是否该**OperationalStatus**值是**处于维护模式**:

        ```PowerShell
        Get-PhysicalDisk
        ```

    4.  通过运行在服务器上执行升级安装的 Windows Server 2019 **setup.exe**并使用"保留个人文件和应用程序"选项。  
    安装完成后，该服务器将保持在群集和群集服务自动启动。

    5.  检查新升级的服务器具有最新的 Windows Server 2019 更新。  
    有关详细信息，请参阅[Windows 10 和 Windows Server 2019 更新历史记录](https://support.microsoft.com/help/4464619/windows-10-update-history)。
    生成号 (请参阅`ver`命令) 应为 17763.292 或更高版本。

    6.  使用以下 PowerShell 命令从存储维护模式中删除服务器：

        ```PowerShell
        Get-StorageFaultDomain -type StorageScaleUnit | 
        Where FriendlyName -Eq <ServerName> | 
        Disable-StorageMaintenanceMode
        ```

    7.  使用以下 PowerShell 命令来恢复服务器：

        ```PowerShell
        Resume-ClusterNode
        ```

    8.  等待存储修复作业完成并返回到正常状态的所有磁盘。  
    这应该是相对较快，因为 Vm 未运行。 下面是要运行的命令：

        ```PowerShell
        Get-StorageJob
        Get-VirtualDisk
        ```

4.  升级群集中的下一个服务器。
5.  所有服务器均已都升级到 Windows Server 2019 后，使用以下 PowerShell cmdlet 来更新群集功能级别。
    
    ```PowerShell
    Update-ClusterFunctionalLevel
    ```

    >   [!NOTE]
    >   我们建议尽快，更新群集功能级别从技术上讲具有最多四个星期来执行此操作也是如此。

6.  更新群集功能级别后，使用以下 cmdlet 更新存储池。  
    此时，新 cmdlet，如`Get-ClusterPerf`将是群集中的任何服务器上完全正常运行。

    ```PowerShell
    Update-StoragePool
    ```

7.  在群集上启动 Vm，并检查它们正在正常工作。

8.  （可选） 通过停止每个 VM 并使用升级虚拟机配置级别`Update-VMVersion`cmdlet，然后重新启动 Vm。

9.  验证已升级的群集可以按照预期方式。  
    角色应正确的故障转移，如果在群集上使用 VM 实时迁移，则应已成功实时迁移虚拟机。

10.  通过运行群集验证来验证群集 (`Test-Cluster`) 并检查群集验证报告。

## <a name="performing-a-clean-os-installation-while-vms-are-stopped"></a>Vm 已停止时执行干净操作系统安装

此选项会导致 VM 停机，但可能需要较少的时间比如果保留在升级过程中运行，因为无需等待存储作业完成后升级每个服务器 （镜像修复） 的 Vm。 尽管各个服务器将按顺序重启在升级过程中，群集中剩余的服务器保持运行状态。

1.  检查群集中的所有服务器都运行最新的更新。  
    有关详细信息，请参阅[Windows Server 2016 和 Windows 10 更新历史记录](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。
    至少，安装 Microsoft 知识库文章[一文 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (2019 年 2 月 19 日)。 生成号 (请参阅`ver`命令) 应为 14393.2828 或更高版本。

2.  停止群集上运行的 Vm。

3.  一次，一台群集服务器上执行以下步骤：

    2.  暂停使用以下 PowerShell 命令在群集服务器，请注意，隐藏某些内部组。  
    我们建议执行此步骤要谨慎。

         ```PowerShell
        Suspend-ClusterNode -Drain
        ```

    3.  将服务器放入存储维护模式中，通过运行以下 PowerShell 命令：

        ```PowerShell
        Get-StorageFaultDomain -type StorageScaleUnit | 
        Where FriendlyName -Eq <ServerName> | 
        Enable-StorageMaintenanceMode
        ```

    4.  运行以下 cmdlet 来检查是否该**OperationalStatus**值是**处于维护模式**:

        ```PowerShell
        Get-PhysicalDisk
        ```

    5.  通过运行以下 PowerShell 命令中收回从群集的服务器：  
    
        ```PowerShell
        Remove-ClusterNode <ServerName>
        ```

    6.  在服务器上执行全新安装的 Windows Server 2019： 格式在系统驱动器，运行**setup.exe** ，并使用"空"选项。  
    您必须配置的服务器标识、 角色、 功能和安装程序完成后的应用程序和服务器重新启动。

    7.  在服务器上安装 HYPER-V 角色和故障转移群集功能 (可以使用`Install-WindowsFeature`cmdlet)。

    8.  安装最新存储和网络硬件驱动程序批准用于存储空间直通服务器制造商提供的。

    9.  检查新升级的服务器具有最新的 Windows Server 2019 更新。  
    有关详细信息，请参阅[Windows 10 和 Windows Server 2019 更新历史记录](https://support.microsoft.com/help/4464619/windows-10-update-history)。
    生成号 (请参阅`ver`命令) 应为 17763.292 或更高版本。

    10.  使用以下 PowerShell 命令重新加入到群集的服务器：

         ```PowerShell
         Add-ClusterNode
         ```

    11.  使用以下 PowerShell 命令从存储维护模式中删除服务器：

         ```PowerShell
         Get-StorageFaultDomain -type StorageScaleUnit | 
         Where FriendlyName -Eq <ServerName> | 
         Disable-StorageMaintenanceMode
         ```

    10.  等待存储修复作业完成并返回到正常状态的所有磁盘。  
    在服务器升级过程中运行，这可能需要相当长的时间，具体取决于 Vm 的数目。 下面是要运行的命令：

         ```PowerShell
         Get-StorageJob
         Get-VirtualDisk
         ```

4.  升级群集中的下一个服务器。

5.  所有服务器均已都升级到 Windows Server 2019 后，使用以下 PowerShell cmdlet 来更新群集功能级别。
    
    ```PowerShell
    Update-ClusterFunctionalLevel
    ```

    >   [!NOTE]
    >   我们建议尽快，更新群集功能级别从技术上讲具有最多四个星期来执行此操作也是如此。

6.  更新群集功能级别后，使用以下 cmdlet 更新存储池。  
    此时，新 cmdlet，如`Get-ClusterPerf`将是群集中的任何服务器上完全正常运行。

    ```PowerShell
    Update-StoragePool
    ```

7.  在群集上启动 Vm，并检查它们正在正常工作。

8.  （可选） 通过停止每个 VM 并使用升级虚拟机配置级别`Update-VMVersion`cmdlet，然后重新启动 Vm。

9.  验证已升级的群集可以按照预期方式。  
    角色应正确的故障转移，如果在群集上使用 VM 实时迁移，则应已成功实时迁移虚拟机。

10.  通过运行群集验证来验证群集 (`Test-Cluster`) 并检查群集验证报告。

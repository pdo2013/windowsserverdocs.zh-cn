---
title: 将存储空间直通群集升级到 Windows Server 2019
description: 如何将存储空间直通群集升级到 Windows Server 2019-将 Vm 保持运行状态或停止运行。
author: robhindman
ms.author: robhind
manager: eldenc
ms.date: 03/06/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-spaces
ms.openlocfilehash: 9966ee0fd3c0a2d1df0180bf177dec03343efc14
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867186"
---
# <a name="upgrade-a-storage-spaces-direct-cluster-to-windows-server-2019"></a>将存储空间直通群集升级到 Windows Server 2019

本主题介绍如何将存储空间直通群集升级到 Windows Server 2019。 有四种方法可以使用[群集操作系统滚动升级过程](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)将存储空间直通群集从 windows server 2016 升级到 windows server 2019，这两种方法涉及到让 vm 保持运行，另外两个则涉及停止所有 vm。 每种方法都有不同的优势和劣势，因此请选择最符合组织需求的方法：

- **就地升级，而 vm**在群集中的每个服务器上运行-此选项不会造成 vm 停机，但需要等待存储作业（镜像修复）在升级每个服务器后完成。

- **清理 OS**在群集中的每个服务器上运行时，此选项不会造成 vm 停机，但需要等待存储作业（镜像修复）在升级每个服务器后完成，并且必须设置每个服务器及其所有应用和角色。

- **就地升级，而**在群集中的每个服务器上停止 vm —此选项会导致 vm 停机，但不需要等待存储作业（镜像修复），因此速度更快。

- **清理 OS 安装，而**在群集中的每个服务器上停止 Vm —此选项会导致 vm 停机时间，但不需要等待存储作业（镜像修复），因此速度更快。

## <a name="prerequisites-and-limitations"></a>先决条件和限制

继续升级之前：

- 检查是否有可用的备份，以防升级过程中出现任何问题。

- 检查您的硬件供应商是否为其将在 Windows Server 2019 上支持的服务器提供了 BIOS、固件和驱动程序。

要注意的升级过程有一些限制：

- 若要启用 Windows Server 2019 版本早于176693.292 的存储空间直通，客户可能需要与 Microsoft 支持部门联系，以启用存储空间直通和软件定义的网络功能。 有关详细信息，请参阅 Microsoft 知识库[文章 4464776](https://support.microsoft.com/help/4464776/software-defined-data-center-and-software-defined-networking)。

- 升级具有 ReFS 卷的群集时，有几个限制：

- ReFS 卷上完全支持升级，但升级后的卷将无法从 Windows Server 2019 中的 ReFS 增强功能中获益。 这些优势（如提高镜像加速奇偶校验的性能）需要新创建的 Windows Server 2019 ReFS 卷。 换句话说，您必须使用`New-Volume` cmdlet 或服务器管理器创建新卷。 下面是一些 ReFS 增强功能：

    - **映射绕过映射**：仅适用于群集（存储空间直通）系统且不适用于独立存储池的 ReFS 性能改进。

    - **压缩**Windows Server 2019 中特定于多复原卷的提高效率。

- 在升级 Windows Server 2016 存储空间直通群集服务器之前，建议将服务器置于存储维护模式下。 有关详细信息，请参阅[疑难解答存储空间直通](troubleshooting-storage-spaces.md)的事件5120部分。 尽管此问题已在 Windows Server 2016 中得到解决，但建议在升级过程中将每个存储空间直通服务器置于存储维护模式，这是最佳做法。

- 使用 SET 开关的软件定义网络环境存在一个已知问题。 此问题涉及从 Windows Server 2019 到 Windows Server 2016 的 Hyper-v VM 实时迁移（到早期操作系统的实时迁移）。 若要确保实时迁移成功，我们建议在从 Windows Server 2019 到 Windows Server 2016 的实时迁移的 Vm 上更改 VM 网络设置。 此问题已在01D 修补程序汇总包（也称为版本17763.292）中针对 Windows Server 2019 解决。 有关详细信息，请参阅 Microsoft 知识库[文章 4476976](https://support.microsoft.com/help/4476976/windows-10-update-kb4476976)。

由于上述已知问题，某些客户可能会考虑构建新的 Windows Server 2019 群集并从旧群集复制数据，而不是使用下面所述的四个过程之一来升级其 Windows Server 2016 群集。

## <a name="performing-an-in-place-upgrade-while-vms-are-running"></a>在 Vm 运行时执行就地升级

此选项不会造成 VM 停机，但需要等待存储作业（镜像修复）在升级每个服务器后完成。 虽然在升级过程中将按顺序重新启动各个服务器，但群集中的剩余服务器以及所有 Vm 都将继续运行。

1. 检查群集中的所有服务器是否都安装了最新的 Windows 更新。 有关详细信息，请参阅[windows 10 和 Windows Server 2016 更新历史记录](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。 至少要安装 Microsoft 知识库[文章 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) （2月19日，2019）。 生成号（请参阅`ver`命令）应为14393.2828 或更高版本。

2. 如果要将软件定义的网络与交换机交换机一起使用，请打开提升的 PowerShell 会话，并运行以下命令，在群集上的所有 Vm 上禁用 VM 实时迁移验证检查：

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. 一次在一个群集服务器上执行以下步骤：

   1. 使用 Hyper-v VM 实时迁移将正在运行的 Vm 从即将升级的服务器上移走。

   2. 使用以下 PowerShell 命令暂停群集服务器-请注意，某些内部组处于隐藏状态。 建议此步骤小心-如果尚未在服务器上实时迁移 Vm，则此 cmdlet 将为你执行此操作，因此，如果你愿意，可以跳过上述步骤。

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. 通过运行以下 PowerShell 命令，将服务器置于存储维护模式：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. 运行以下 cmdlet 以检查**OperationalStatus**值是否处于**维护模式**：

       ```PowerShell
       Get-PhysicalDisk
       ```

   5. 通过运行**setup.exe**并使用 "保留个人文件和应用" 选项，在服务器上执行 Windows server 2019 的升级安装。 安装完成后，服务器将保留在群集中，群集服务将自动启动。

   6. 检查新升级的服务器是否具有最新的 Windows Server 2019 更新。 有关详细信息，请参阅[windows 10 和 Windows Server 2019 更新历史记录](https://support.microsoft.com/help/4464619/windows-10-update-history)。 生成号（请参阅`ver`命令）应为17763.292 或更高版本。

   7. 使用以下 PowerShell 命令从存储维护模式中删除服务器：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   8. 使用以下 PowerShell 命令恢复服务器：

       ```PowerShell
       Resume-ClusterNode
       ```

   9. 等待存储修复作业完成，所有磁盘都将恢复正常状态。 这可能需要相当长的时间，具体取决于在服务器升级过程中运行的 Vm 的数量。 下面是要运行的命令:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. 升级群集中的下一个服务器。

5. 将所有服务器升级到 Windows Server 2019 后，请使用以下 PowerShell cmdlet 更新群集功能级别。

   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > 我们建议尽快更新群集功能级别，尽管从技术上讲，最多可有四周的时间。

6. 更新群集功能级别后，请使用以下 cmdlet 更新存储池。 此时，新的 cmdlet （如`Get-ClusterPerf` ）将在群集中的任何服务器上完全正常运行。

   ```PowerShell
   Update-StoragePool
   ```

7. 可以选择通过停止每个 vm，使用`Update-VMVersion` cmdlet，然后再次启动 vm，来升级 vm 配置级别。

8. 如果将软件定义的网络与 SET 开关和禁用的 VM 实时迁移检查一起使用，请使用以下 cmdlet 重新启用 VM 实时验证检查：

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter  SkipMigrationDestinationCheck -Value 0
   ```

9. 验证升级的群集是否按预期方式工作。 角色应正确地进行故障转移，并且如果在群集上使用 VM 实时迁移，Vm 应能成功地进行实时迁移。

10. 通过运行群集验证（`Test-Cluster`）并检查群集验证报告来验证群集。

## <a name="performing-a-clean-os-installation-while-vms-are-running"></a>Vm 运行时执行干净的操作系统安装

此选项不会造成 VM 停机，但需要等待存储作业（镜像修复）在升级每个服务器后完成。 虽然在升级过程中将按顺序重新启动各个服务器，但群集中的剩余服务器以及所有 Vm 都将继续运行。

1. 检查群集中的所有服务器是否都在运行最新更新。 有关详细信息，请参阅[windows 10 和 Windows Server 2016 更新历史记录](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。 至少要安装 Microsoft 知识库[文章 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) （2月19日，2019）。 生成号（请参阅`ver`命令）应为14393.2828 或更高版本。

2. 如果要将软件定义的网络与交换机交换机一起使用，请打开提升的 PowerShell 会话，并运行以下命令，在群集上的所有 Vm 上禁用 VM 实时迁移验证检查：

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. 一次在一个群集服务器上执行以下步骤：

   1. 使用 Hyper-v VM 实时迁移将正在运行的 Vm 从即将升级的服务器上移走。

   2. 使用以下 PowerShell 命令暂停群集服务器-请注意，某些内部组处于隐藏状态。 建议此步骤小心-如果尚未在服务器上实时迁移 Vm，则此 cmdlet 将为你执行此操作，因此，如果你愿意，可以跳过上述步骤。

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. 通过运行以下 PowerShell 命令，将服务器置于存储维护模式：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. 运行以下 cmdlet 以检查**OperationalStatus**值是否处于**维护模式**：

       ```PowerShell
       Get-PhysicalDisk
       ```

   3.  通过运行以下 PowerShell 命令从群集中逐出服务器：  

       ```PowerShell
       Remove-ClusterNode <ServerName>
       ```

   4. 在服务器上执行 Windows Server 2019 的干净安装：格式化系统驱动器，运行**setup.exe** ，并使用 "无" 选项。 安装完成后，必须配置服务器标识、角色、功能和应用程序，然后重新启动服务器。

   5. 在服务器上安装 hyper-v 角色和故障转移群集功能（可以使用`Install-WindowsFeature` cmdlet）。

   6. 为你的服务器制造商批准的硬件安装最新的存储和网络驱动程序，以便与存储空间直通一起使用。

   7. 检查新升级的服务器是否具有最新的 Windows Server 2019 更新。 有关详细信息，请参阅[windows 10 和 Windows Server 2019 更新历史记录](https://support.microsoft.com/help/4464619/windows-10-update-history)。 生成号（请参阅`ver`命令）应为17763.292 或更高版本。

   8. 使用以下 PowerShell 命令将服务器重新加入群集：

       ```PowerShell
       Add-ClusterNode
       ```

   9. 使用以下 PowerShell 命令从存储维护模式中删除服务器：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   10. 等待存储修复作业完成，所有磁盘都将恢复正常状态。 这可能需要相当长的时间，具体取决于在服务器升级过程中运行的 Vm 的数量。 下面是要运行的命令:

        ```PowerShell
        Get-StorageJob
        Get-VirtualDisk
        ```

4. 升级群集中的下一个服务器。

5. 将所有服务器升级到 Windows Server 2019 后，请使用以下 PowerShell cmdlet 更新群集功能级别。
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > 我们建议尽快更新群集功能级别，尽管从技术上讲，最多可有四周的时间。

6. 更新群集功能级别后，请使用以下 cmdlet 更新存储池。 此时，新的 cmdlet （如`Get-ClusterPerf` ）将在群集中的任何服务器上完全正常运行。

   ```PowerShell
   Update-StoragePool
   ```

7. 可以选择通过停止每个 vm 并使用`Update-VMVersion` cmdlet，然后再次启动 vm，来升级 vm 配置级别。

8. 如果将软件定义的网络与 SET 开关和禁用的 VM 实时迁移检查一起使用，请使用以下 cmdlet 重新启用 VM 实时验证检查：

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" | 
   Set-ClusterParameter SkipMigrationDestinationCheck -Value 0
   ```

9. 验证升级的群集是否按预期方式工作。 角色应正确地进行故障转移，并且如果在群集上使用 VM 实时迁移，Vm 应能成功地进行实时迁移。

10. 通过运行群集验证（`Test-Cluster`）并检查群集验证报告来验证群集。

## <a name="performing-an-in-place-upgrade-while-vms-are-stopped"></a>Vm 停止时执行就地升级

此选项会导致 VM 停机时间，但要花费的时间比在升级过程中保留 Vm 的时间少，因为在升级每个服务器后无需等待存储作业（镜像修复）完成。 虽然在升级过程中将按顺序重新启动各个服务器，但群集中的剩余服务器仍保持运行状态。

1. 检查群集中的所有服务器是否都在运行最新更新。 有关详细信息，请参阅[windows 10 和 Windows Server 2016 更新历史记录](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。 至少要安装 Microsoft 知识库[文章 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) （2月19日，2019）。 生成号（请参阅`ver`命令）应为14393.2828 或更高版本。

2. 停止群集上运行的 Vm。

3. 一次在一个群集服务器上执行以下步骤：

   1. 使用以下 PowerShell 命令暂停群集服务器-请注意，某些内部组处于隐藏状态。 建议执行此步骤以谨慎。

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   2. 通过运行以下 PowerShell 命令，将服务器置于存储维护模式：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   3. 运行以下 cmdlet 以检查**OperationalStatus**值是否处于**维护模式**：

       ```PowerShell
       Get-PhysicalDisk
       ```

   4. 通过运行**setup.exe**并使用 "保留个人文件和应用" 选项，在服务器上执行 Windows server 2019 的升级安装。  
   安装完成后，服务器将保留在群集中，群集服务将自动启动。

   5.  检查新升级的服务器是否具有最新的 Windows Server 2019 更新。  
   有关详细信息，请参阅[windows 10 和 Windows Server 2019 更新历史记录](https://support.microsoft.com/help/4464619/windows-10-update-history)。
   生成号（请参阅`ver`命令）应为17763.292 或更高版本。

   6.  使用以下 PowerShell 命令从存储维护模式中删除服务器：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   7.  使用以下 PowerShell 命令恢复服务器：

       ```PowerShell
       Resume-ClusterNode
       ```

   8.  等待存储修复作业完成，所有磁盘都将恢复正常状态。  
   这应该会相对较快，因为 Vm 未运行。 下面是要运行的命令:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. 升级群集中的下一个服务器。
5. 将所有服务器升级到 Windows Server 2019 后，请使用以下 PowerShell cmdlet 更新群集功能级别。
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   我们建议尽快更新群集功能级别，尽管从技术上讲，最多可有四周的时间。

6. 更新群集功能级别后，请使用以下 cmdlet 更新存储池。  
   此时，新的 cmdlet （如`Get-ClusterPerf` ）将在群集中的任何服务器上完全正常运行。

   ```PowerShell
   Update-StoragePool
   ```

7. 启动群集上的 Vm，并检查它们是否正常工作。

8. 可以选择通过停止每个 vm 并使用`Update-VMVersion` cmdlet，然后再次启动 vm，来升级 vm 配置级别。

9. 验证升级的群集是否按预期方式工作。  
   角色应正确地进行故障转移，并且如果在群集上使用 VM 实时迁移，Vm 应能成功地进行实时迁移。

10. 通过运行群集验证（`Test-Cluster`）并检查群集验证报告来验证群集。

## <a name="performing-a-clean-os-installation-while-vms-are-stopped"></a>Vm 停止时执行干净的操作系统安装

此选项会导致 VM 停机时间，但要花费的时间比在升级过程中保留 Vm 的时间少，因为在升级每个服务器后无需等待存储作业（镜像修复）完成。 虽然在升级过程中将按顺序重新启动各个服务器，但群集中的剩余服务器仍保持运行状态。

1. 检查群集中的所有服务器是否都在运行最新更新。  
   有关详细信息，请参阅[windows 10 和 Windows Server 2016 更新历史记录](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。
   至少要安装 Microsoft 知识库[文章 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) （2月19日，2019）。 生成号（请参阅`ver`命令）应为14393.2828 或更高版本。

2. 停止群集上运行的 Vm。

3. 一次在一个群集服务器上执行以下步骤：

   2. 使用以下 PowerShell 命令暂停群集服务器-请注意，某些内部组处于隐藏状态。  
      建议执行此步骤以谨慎。

       ```PowerShell
      Suspend-ClusterNode -Drain
      ```

   3. 通过运行以下 PowerShell 命令，将服务器置于存储维护模式：

      ```PowerShell
      Get-StorageFaultDomain -type StorageScaleUnit | 
      Where FriendlyName -Eq <ServerName> | 
      Enable-StorageMaintenanceMode
      ```

   4. 运行以下 cmdlet 以检查**OperationalStatus**值是否处于**维护模式**：

      ```PowerShell
      Get-PhysicalDisk
      ```

   5. 通过运行以下 PowerShell 命令从群集中逐出服务器：  
    
      ```PowerShell
      Remove-ClusterNode <ServerName>
      ```

   6. 在服务器上执行 Windows Server 2019 的干净安装：格式化系统驱动器，运行**setup.exe** ，并使用 "无" 选项。  
      安装完成后，必须配置服务器标识、角色、功能和应用程序，然后重新启动服务器。

   7. 在服务器上安装 hyper-v 角色和故障转移群集功能（可以使用`Install-WindowsFeature` cmdlet）。

   8. 为你的服务器制造商批准的硬件安装最新的存储和网络驱动程序，以便与存储空间直通一起使用。

   9. 检查新升级的服务器是否具有最新的 Windows Server 2019 更新。  
      有关详细信息，请参阅[windows 10 和 Windows Server 2019 更新历史记录](https://support.microsoft.com/help/4464619/windows-10-update-history)。
      生成号（请参阅`ver`命令）应为17763.292 或更高版本。

   10. 使用以下 PowerShell 命令将服务器重新加入群集：

      ```PowerShell
      Add-ClusterNode
      ```

   11. 使用以下 PowerShell 命令从存储维护模式中删除服务器：

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   12. 等待存储修复作业完成，所有磁盘都将恢复正常状态。  
       这可能需要相当长的时间，具体取决于在服务器升级过程中运行的 Vm 的数量。 下面是要运行的命令:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. 升级群集中的下一个服务器。

5. 将所有服务器升级到 Windows Server 2019 后，请使用以下 PowerShell cmdlet 更新群集功能级别。
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   我们建议尽快更新群集功能级别，尽管从技术上讲，最多可有四周的时间。

6. 更新群集功能级别后，请使用以下 cmdlet 更新存储池。  
   此时，新的 cmdlet （如`Get-ClusterPerf` ）将在群集中的任何服务器上完全正常运行。

   ```PowerShell
   Update-StoragePool
   ```

7. 启动群集上的 Vm，并检查它们是否正常工作。

8. 可以选择通过停止每个 vm 并使用`Update-VMVersion` cmdlet，然后再次启动 vm，来升级 vm 配置级别。

9. 验证升级的群集是否按预期方式工作。  
   角色应正确地进行故障转移，并且如果在群集上使用 VM 实时迁移，Vm 应能成功地进行实时迁移。

10. 通过运行群集验证（`Test-Cluster`）并检查群集验证报告来验证群集。

---
title: 在 Windows 10 或 Windows Server 上的 HYPER-V 中虚拟机版本升级
description: 为提供的说明和升级版本的虚拟机的注意事项
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 897f2454-5aee-445c-a63e-f386f514a0f6
author: KBDAzure
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 8b277796492ca7d72b1a8713484c691cb9a8dca3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819438"
---
# <a name="upgrade-virtual-machine-version-in-hyper-v-on-windows-10-or-windows-server"></a>在 Windows 10 或 Windows Server 上的 HYPER-V 中虚拟机版本升级

>适用于：Windows 10，Windows Server 2016 中，Windows Server 2019

提供的最新的 HYPER-V 功能在虚拟机上通过升级配置版本。 不执行前此操作：

- 升级到最新版本的 Windows 或 Windows Server 的 HYPER-V 主机。
- 升级群集功能级别。
- 您一定不会需要将虚拟机移回运行 Windows 或 Windows Server 的早期版本的 HYPER-V 主机。

有关详细信息，请参阅[群集操作系统滚动升级](../../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)并[在 VMM 中执行的 HYPER-V 主机群集滚动升级](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade)。

## <a name="step-1-check-the-virtual-machine-configuration-versions"></a>第 1 步：检查虚拟机配置版本

1. 在 Windows 桌面上，单击“开始”按钮并键入名称 **Windows PowerShell** 的任一部分。
2. 右键单击 Windows PowerShell，然后选择**以管理员身份运行**。
3. 使用[GET-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm)cmdlet。 运行以下命令以获取你的虚拟机的版本。

```PowerShell
Get-VM * | Format-Table Name, Version
```

您还可以查看配置版本的 HYPER-V 管理器中选择的虚拟机并查看**摘要**选项卡。

## <a name="step-2-upgrade-the-virtual-machine-configuration-version"></a>步骤 2：升级虚拟机配置版本

1. 关闭 Hyper-v 管理器中的虚拟机。
2. 选择操作 > 升级配置版本。 如果此选项不适用于虚拟机，然后它是已最高配置版本支持的 HYPER-V 主机。

若要使用 Windows PowerShell 升级虚拟机配置版本，请使用[Update-vmversion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) cmdlet。 运行以下命令，其中 vmname 是虚拟机的名称。

```PowerShell
Update-VMVersion <vmname>
```

## <a name="BKMK_SupportedConfigVersions"></a>支持的虚拟机配置版本

运行 PowerShell cmdlet [Get VMHostSupportedVersion](https://docs.microsoft.com/powershell/module/hyper-v/get-vmhostsupportedversion)若要查看的 HYPER-V 主机支持哪些虚拟机配置版本。 当你创建虚拟机时，则使用默认配置版本创建它。 若要查看默认值，请运行以下命令。

```PowerShell
Get-VMHostSupportedVersion -Default
```

如果你需要创建可以将它们移到 HYPER-V 主机的虚拟机运行较旧版本的 Windows，使用[NEW-VM](https://docs.microsoft.com/powershell/module/hyper-v/new-vm) cmdlet 与-version 参数。 例如，若要创建可以将它们移动到运行 Windows Server 2012 R2 的 HYPER-V 主机的虚拟机，请运行以下命令。 此命令将创建虚拟机配置版本 5.0 创建名为"WindowsCV5"。

```PowerShell
New-VM -Name "WindowsCV5" -Version 5.0
```

>[!NOTE]
>可以导入已为运行 Windows 的较旧版本的 HYPER-V 主机创建的虚拟机或从备份中还原它们。 如果 VM 的配置版本未列出所支持的 HYPER-V 主机操作系统下表中，您必须更新虚拟机配置版本，然后才能启动 VM。

### <a name="supported-vm-configuration-versions-for-long-term-servicing-hosts"></a>支持的长期维护主机的虚拟机配置版本

下表列出了支持运行 Windows 的长期维护服务版本的主机的虚拟机配置版本。

|Windows 版本的 HYPER-V 主机| 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
|---|---|---|---|---|---|---|---|---|---|
|Windows Server 2019|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 企业版 LTSC 2019|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 企业版 2016 长期服务|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 企业版 2015 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|
|Windows Server 2012 R2|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|
|Windows 8.1|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|

### <a name="supported-vm-configuration-versions-for-semi-annual-channel-hosts"></a>每半年频道主机的受支持的 VM 配置版本

下表列出了对于运行 Windows 的当前支持的半年频道版本的主机的 VM 配置版本。 若要获取每半年频道版本的 Windows 上的详细信息，请访问以下页面了解[Windows Server](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview)和[Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview#servicing-channels)

|Windows 版本的 HYPER-V 主机| 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
|---|---|---|---|---|---|---|---|---|---|
|Windows Server 版本 1809|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 2018 年 10 月更新 （版本 1809年）|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 版本 1803|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 2018 年 4 月更新 （版本 1803年）|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Fall Creators Update （版本 1709年）|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 版本 1709|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 创意者更新 (1703 版本)|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 周年更新 （版本 1607年）|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="why-should-i-upgrade-the-virtual-machine-configuration-version"></a>为什么应该升级虚拟机配置版本？

当移动或虚拟机导入到 Windows Server 2019、 Windows Server 2016 或 Windows 10 中，虚拟机运行 HYPER-V 的计算机"不会自动更新配置。 这意味着，你可以移动虚拟机返回到运行 Windows 或 Windows Server 的早期版本的 HYPER-V 主机。 但是，这也意味着，您不能使用的一些新的虚拟机功能直到手动更新配置版本。 已升级后，不能降级虚拟机配置版本。

虚拟机配置版本表示虚拟机的配置，使用的版本的 HYPER-V 保存状态和快照文件的兼容性。 更新配置版本，会更改用于存储的虚拟机配置和检查点文件的文件结构。 你还可以为该 HYPER-V 主机支持的最新版本更新的配置版本。 升级后的虚拟机使用新的配置文件格式，该文件格式旨在提高读取和写入虚拟机配置数据的效率。 升级还减少了存储失败时数据损坏的可能性。

下表列出了说明、 文件扩展名和用于新的或已升级虚拟机的文件的每种类型的默认位置。

 |虚拟机的文件类型 | 描述|
 |---|---|
|配置 |以二进制文件格式存储的虚拟机配置信息。 <br /> 文件扩展名：.vmcx <br /> 默认位置：C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Machines|
 |运行时状态|以二进制文件格式存储的虚拟机运行时状态信息。 <br />文件扩展名：.vmrs 和.vmgs <br />默认位置：C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Machines|
|虚拟硬盘|将存储虚拟机的虚拟硬盘。 <br /> 文件扩展名：.vhd 或.vhdx <br />默认位置：C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Hard Disks|
 |自动虚拟硬盘 |用于虚拟机检查点的差异磁盘文件。 <br /> 文件扩展名：.avhdx <br /> 默认位置：C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Hard Disks|
 |Checkpoint|检查点存储在多个检查点文件中。 每个检查点都会创建一个配置文件和运行时状态文件。 <br /> 文件扩展名：.vmrs 和.vmcx <br />默认位置：C:\ProgramData\Microsoft\Windows\Snapshots|

## <a name="what-happens-if-i-dont-upgrade-the-virtual-machine-configuration-version"></a>如果不升级虚拟机配置版本，会发生什么情况？

如果你有使用早期版本的 HYPER-V 创建的虚拟机，一些功能，可更高版本的主机 OS 可能无法使用这些虚拟机更新的配置版本前上可用。

作为一般原则，我们建议更新的配置版本后已成功升级到较新版本的 Windows 虚拟化主机，并确信不需要回滚。 使用时[群集操作系统滚动升级](https://docs.microsoft.com/windows-server/failover-clustering/Cluster-Operating-System-Rolling-Upgrade)功能，则通常会在更新群集功能级别后。 这样一来，将从新特性和内部更改以及优化也获益。

>[!NOTE]
>更新虚拟机配置版本之后, VM 将无法在不支持更新的配置版本的主机上启动。

下表显示了使用某些 HYPER-V 功能所需的最小虚拟机配置版本。

|功能|最小虚拟机配置版本|
|---|---|
|热添加/删除内存|6.2|
|Linux VM 的安全启动|6.2|
|生产检查点|6.2|
|PowerShell Direct |6.2|
|虚拟机分组|6.2|
|虚拟受信任的平台模块 (vTPM)|7.0|
|虚拟机多队列 (VMMQ)|7.1|
|XSAVE 支持|8.0|
|密钥存储驱动器|8.0|
|来宾基于虚拟化的安全性支持 (VBS)|8.0|
|嵌套虚拟化|8.0|
|虚拟处理器计数|8.0|
|较大内存的虚拟机|8.0|
|增加到 64 每个设备 （例如网络和分配的设备） 的虚拟设备的默认最大数目|8.3|
|Perfmon 允许额外的处理器功能|9.0|
|自动公开[同时进行多线程处理](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#background)使用的主机上运行的 Vm 的配置[Core 计划程序](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler)|9.0|
|休眠支持|9.0|

有关这些功能的详细信息，请参阅[什么是 Windows Server 上的 HYPER-V 中的新增功能](../What-s-new-in-Hyper-V-on-Windows.md)。


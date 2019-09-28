---
title: 在 Windows 10 或 Windows Server 上的 Hyper-v 中升级虚拟机版本
description: 提供有关升级虚拟机版本的说明和注意事项
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 897f2454-5aee-445c-a63e-f386f514a0f6
author: jasongerend
ms.author: jgerend
ms.date: 05/22/2019
ms.openlocfilehash: 96678dfab2a3d5b6f503d8ce9d00850a3c437b35
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392932"
---
# <a name="upgrade-virtual-machine-version-in-hyper-v-on-windows-10-or-windows-server"></a>在 Windows 10 或 Windows Server 上的 Hyper-v 中升级虚拟机版本

>适用于：Windows 10、Windows Server 2019、Windows Server 2016、Windows Server （半年频道）

通过升级配置版本，使最新的 Hyper-v 功能在虚拟机上可用。 在以下时间之前不要执行此操作：

- 将 Hyper-v 主机升级到最新版本的 Windows 或 Windows Server。
- 升级群集功能级别。
- 你确定不需要将虚拟机移回运行 Windows 或 Windows Server 以前版本的 Hyper-v 主机。

有关详细信息，请参阅[群集操作系统滚动升级](../../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)和[在 VMM 中执行 hyper-v 主机群集的滚动升级](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade)。

## <a name="step-1-check-the-virtual-machine-configuration-versions"></a>第 1 步：检查虚拟机配置版本

1. 在 Windows 桌面上，单击“开始”按钮并键入名称 **Windows PowerShell** 的任一部分。
2. 右键单击 "Windows PowerShell" 并选择 "以**管理员身份运行**"。
3. 使用[GET VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm)cmdlet。 运行以下命令，获取虚拟机的版本。

```PowerShell
Get-VM * | Format-Table Name, Version
```

还可以通过选择虚拟机并查看 "**摘要**" 选项卡来查看 Hyper-v 管理器中的配置版本。

## <a name="step-2-upgrade-the-virtual-machine-configuration-version"></a>步骤 2：升级虚拟机配置版本

1. 关闭 Hyper-v 管理器中的虚拟机。
2. 选择操作 > 升级配置版本。 如果此选项不适用于虚拟机，则该选项已是 Hyper-v 主机支持的最高配置版本。

若要使用 Windows PowerShell 升级虚拟机配置版本，请使用[VMVersion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) cmdlet。 运行以下命令，其中 vmname 是虚拟机的名称。

```PowerShell
Update-VMVersion <vmname>
```

## <a name="supported-virtual-machine-configuration-versions"></a>支持的虚拟机配置版本

运行 PowerShell cmdlet [VMHostSupportedVersion](https://docs.microsoft.com/powershell/module/hyper-v/get-vmhostsupportedversion) ，查看 hyper-v 主机支持的虚拟机配置版本。 当你创建虚拟机时，将使用默认配置版本创建它。 若要查看默认值，请运行以下命令。

```PowerShell
Get-VMHostSupportedVersion -Default
```

如果需要创建可移至运行旧版 Windows 的 Hyper-v 主机的虚拟机，请使用带有-version 参数的[新 VM](https://docs.microsoft.com/powershell/module/hyper-v/new-vm) cmdlet。 例如，若要创建可以移动到运行 Windows Server 2012 R2 的 Hyper-v 主机的虚拟机，请运行以下命令。 此命令将创建一个名为 "WindowsCV5" 的虚拟机，其配置版本为5.0。

```PowerShell
New-VM -Name "WindowsCV5" -Version 5.0
```

>[!NOTE]
>你可以导入为运行较旧版本的 Windows 的 Hyper-v 主机创建的虚拟机，或从备份还原它们。 如果下表中的 Hyper-v 主机操作系统未列出 VM 的配置版本，则必须先更新 VM 配置版本，然后才能启动 VM。

### <a name="supported-vm-configuration-versions-for-long-term-servicing-hosts"></a>支持长期服务主机的 VM 配置版本

下表列出了运行长期服务版本 Windows 的主机支持的 VM 配置版本。

| Hyper-v 主机 Windows 版本 | 9.1 | 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
|Windows Server 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 企业版 LTSC 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 企业版 2016 长期服务|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 企业版 2015 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|
|Windows Server 2012 R2|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|
|Windows 8.1|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|

### <a name="supported-vm-configuration-versions-for-semi-annual-channel-hosts"></a>半年频道主机支持的 VM 配置版本

下表列出了运行当前支持的半年频道版本 Windows 的主机的 VM 配置版本。 若要获取有关半年频道版本 Windows 的详细信息，请访问[Windows Server](../../../get-started-19/servicing-channels-19.md)和[windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview#servicing-channels)的以下页面

| Hyper-v 主机 Windows 版本 | 9.1 | 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
| Windows 10 可能会2019更新（版本1903） |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
| Windows Server 版本 1903 |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
|Windows Server 版本 1809|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 十月2018更新（版本1809）|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 版本 1803|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 2018 年4月更新（版本1803）|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 秋季创意者更新（版本1709）|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 创意者更新（版本1703）|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 周年更新（版本1607）|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="why-should-i-upgrade-the-virtual-machine-configuration-version"></a>为什么应升级虚拟机配置版本？

将虚拟机移动或导入到运行 Windows Server 2019、Windows Server 2016 或 Windows 10 上的 Hyper-v 的计算机时，不会自动更新虚拟机的配置。 这意味着，你可以将虚拟机移回运行 Windows 或 Windows Server 以前版本的 Hyper-v 主机。 但这也意味着，在手动更新配置版本之前，你无法使用某些新的虚拟机功能。 升级虚拟机配置版本后，不能将其降级。

虚拟机配置版本表示虚拟机的配置、已保存状态和快照文件与 Hyper-v 版本的兼容性。 更新配置版本时，将更改用于存储虚拟机配置和检查点文件的文件结构。 还需要将配置版本更新为该 Hyper-v 主机支持的最新版本。 升级后的虚拟机使用新的配置文件格式，该文件格式旨在提高读取和写入虚拟机配置数据的效率。 升级还减少了存储失败时数据损坏的可能性。

下表列出了用于新的或升级的虚拟机的每种文件类型的说明、文件扩展名和默认位置。

 |虚拟机文件类型 | 描述|
 |---|---|
|配置 |以二进制文件格式存储的虚拟机配置信息。 <br /> 文件扩展名：. .vmcx <br /> 默认位置：C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Machines|
 |运行时状态|以二进制文件格式存储的虚拟机运行时状态信息。 <br />文件扩展名：. .vmrs 和. vmgs <br />默认位置：C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Machines|
|虚拟硬盘|存储虚拟机的虚拟硬盘。 <br /> 文件扩展名： .vhd 或 .vhdx <br />默认位置：C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual 硬盘|
 |自动虚拟硬盘 |用于虚拟机检查点的差异磁盘文件。 <br /> 文件扩展名：. .avhdx <br /> 默认位置：C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual 硬盘|
 |Checkpoint|检查点存储在多个检查点文件中。 每个检查点都会创建一个配置文件和运行时状态文件。 <br /> 文件扩展名：. .vmrs 和. .vmcx <br />默认位置：C:\ProgramData\Microsoft\Windows\Snapshots|

## <a name="what-happens-if-i-dont-upgrade-the-virtual-machine-configuration-version"></a>如果不升级虚拟机配置版本，会发生什么情况？

如果你有使用早期版本的 Hyper-v 创建的虚拟机，则在更新配置版本之前，更高版本的主机操作系统上可用的某些功能可能无法使用这些虚拟机。

通常，我们建议在将虚拟化主机成功升级到较新版本的 Windows 后更新配置版本，并确信不需要回滚。 使用[群集操作系统滚动升级](https://docs.microsoft.com/windows-server/failover-clustering/Cluster-Operating-System-Rolling-Upgrade)功能时，通常会在更新群集功能级别后进行。 这样一来，你将从新功能和内部更改和优化中获益。

>[!NOTE]
>VM 配置版本更新后，VM 将无法在不支持更新的配置版本的主机上启动。

下表显示了使用一些 Hyper-v 功能所需的最低虚拟机配置版本。

|功能|最低 VM 配置版本|
|---|---|
|热添加/删除内存|6.2|
|Linux VM 的安全启动|6.2|
|生产检查点|6.2|
|PowerShell Direct |6.2|
|虚拟机分组|6.2|
|虚拟受信任的平台模块 (vTPM)|7.0|
|虚拟机多队列（VMMQ）|7.1|
|XSAVE 支持|8.0|
|密钥存储驱动器|8.0|
|基于来宾虚拟化的安全支持（VBS）|8.0|
|嵌套虚拟化|8.0|
|虚拟处理器计数|8.0|
|大内存 Vm|8.0|
|将虚拟设备的默认最大数目增加到每个设备64（例如网络和分配的设备）|8.3|
|允许为 Perfmon 提供额外的处理器功能|9.0|
|使用[核心计划程序](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler)自动为在主机上运行的 vm 公开[同步多线程](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#background)配置|9.0|
|休眠支持|9.0|

有关这些功能的详细信息，请参阅[Windows Server 上的 hyper-v 中的新增](../What-s-new-in-Hyper-V-on-Windows.md)功能。


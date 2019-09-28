---
ms.assetid: 07d6b251-c492-4d9f-bcc4-031023695b24
title: 安装和启用重复数据删除
ms.technology: storage-deduplication
ms.prod: windows-server
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 05/09/2017
description: 如何在 Windows Server 上安装重复数据删除，如何确定工作负荷是否适合进行重复数据删除，以及如何在卷上启用重复数据删除。
ms.openlocfilehash: 36c9894fd8916643340134698f36af3bd50c34d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402328"
---
# <a name="install-and-enable-data-deduplication"></a>安装和启用重复数据删除
> 适用于 Windows Server（半年频道）、Windows Server 2016

本主题说明了如何安装[重复数据删除](overview.md)、评估进行重复数据删除的工作负荷以及在特定卷上启用重复数据删除。

> [!Note]  
> 如果你打算在故障转移群集中运行重复数据删除，则群集中的每个节点必须安装重复数据删除服务器角色。

## <a id="install-dedup"></a>安装重复数据删除
> [!Important]  
> [KB4025334](https://support.microsoft.com/kb/4025334) 包含重复数据删除的累积的修补程序（包括重要的可靠性修补程序），我们强烈建议在将重复数据删除与 Windows Server 2016 配合使用时安装该修补程序。

### <a id="install-dedup-via-server-manager"></a>使用服务器管理器安装重复数据删除
1. 从“添加角色和功能”向导，选择“**服务器角色**”，然后选择“**重复数据删除**”。  
通过服务器管理器 @no__t 重复数据删除：从服务器角色中选择重复数据删除 @ no__t-1
2. 单击 **“下一步”** 直到 **“安装”** 按钮处于活跃状态，然后单击 **“安装”** 。  
通过服务器管理器 @no__t 重复数据删除：单击 install @ no__t-1

### <a id="install-dedup-via-powershell"></a>使用 PowerShell 安装重复数据删除
要安装重复数据删除，请以管理员身份运行以下 PowerShell 命令：  
`Install-WindowsFeature -Name FS-Data-Deduplication`

在 Nano Server 安装中安装重复数据删除：

1. 根据 [Nano Server 入门](../../get-started/getting-started-with-nano-server.md)中所述步骤，创建已安装“存储”的 Nano Server 安装。
2. 从在任何模式下运行 Windows Server 2016 的服务器（Nano Server 除外）或从已安装[远程服务器管理工具](https://www.microsoft.com/download/details.aspx?id=45520) (RSAT) 的 Windows PC，通过显式引用 Nano Server 实例来安装重复数据删除（将“MyNanoServer”替换为 Nano Server 实例的真实名称）：  
    ```PowerShell
    Install-WindowsFeature -ComputerName <MyNanoServer> -Name FS-Data-Deduplication
    ```  
    <br />
    <strong>--或--</strong>
    @ NO__T-2<br />
    使用 PowerShell 远程处理远程连接到 Nano Server 实例并使用 DISM 安装重复数据删除：  
    
    ```PowerShell
    Enter-PSSession -ComputerName MyNanoServer 
    dism /online /enable-feature /featurename:dedup-core /all
    ```

## <a id="enable-dedup"></a>启用重复数据删除
### <a id="enable-dedup-candidate-workloads"></a>确定哪些工作负荷是重复数据删除的候选项
通过减少冗余数据占用的磁盘空间量，重复数据删除可以有效地将服务器应用程序的数据使用成本降至最低。 在启用重复数据删除之前，请务必了解你的工作负荷特征，以确保获取存储的最佳性能。 有两类工作负荷需要考虑：

* *建议的工作负荷* - 已证实具有高度受益于重复数据删除的数据集并且具有与重复数据删除的后续处理模型兼容的资源消耗模式。 对于这些工作负荷，建议始终[启用重复数据删除](install-enable.md#enable-dedup-lights-on)：
    * 一般用途文件服务器 (GPFS) 提供共享服务，如团队共享、用户主页文件夹、工作文件夹和软件开发共享。
    * 虚拟桌面基础结构 (VDI) 服务器。
    * 虚拟化备份应用程序，如 [Microsoft Data Protection Manager (DPM)](https://technet.microsoft.com/library/hh758173.aspx)。
* 可能受益于重复数据删除但并非始终适合进行重复数据删除的工作负荷。 例如，以下工作负荷可以很好地使用重复数据删除，但你应该首先评估重复数据删除的好处：
    * 一般用途 Hyper-V 主机
    * SQL Server
    * 业务线 (LOB) 服务器

### <a id="enable-dedup-evaluating-sometimes-workloads"></a>评估重复数据删除的工作负荷
> [!Important]  
> 如果当前运行的是建议的工作负荷，则可以跳过此部分并转到工作负荷的[启用重复数据删除](install-enable.md#enable-dedup-lights-on)。

要确定工作负荷是否适用于重复数据删除，请回答以下问题。 如果你对某个工作负荷不确定，请考虑针对工作负荷在测试数据集上执行重复数据删除试验部署，以查看执行情况如何。

1. **工作负荷的数据集是否有足够的重复项来受益于启用重复数据删除？**  
    在针对工作负荷启用重复数据删除之前，通过使用重复数据删除节省评估工具（即 DDPEval）调查工作负荷的数据集重复程度如何。 在安装重复数据删除后，可以在 `C:\Windows\System32\DDPEval.exe` 处找到此工具。 DDPEval 可以针对直接连接的卷（包括本地驱动器或群集共享卷）和映射或未映射的网络共享评估优化的可能性。  
    &nbsp;   
    运行 DDPEval.exe 会返回一个输出，类似以下所示：  
    &nbsp;  
    `Data Deduplication Savings Evaluation Tool`  
    `Copyright 2011-2012 Microsoft Corporation.  All Rights Reserved.`    
    &nbsp;   
    `Evaluated folder: E:\Test`     
    `Processed files: 34`  
    `Processed files size: 12.03MB`  
    `Optimized files size: 4.02MB`  
    `Space savings: 8.01MB`  
    `Space savings percent: 66`  
    `Optimized files size (no compression): 11.47MB`  
    `Space savings (no compression): 571.53KB`  
    `Space savings percent (no compression): 4`  
    `Files with duplication: 2`  
    `Files excluded by policy: 20`  
    `Files excluded by error: 0`  

2. @no__t 0What 是否将工作负荷的 i/o 模式设置为其数据集外观？我的工作负荷有哪些性能？ **  
     重复数据删除会将优化文件作为定期作业，而不是在将文件写入磁盘时才进行优化。 因此，要检查的重要事项就是相对于已删除重复卷的工作负荷的预期读取模式。 因为重复数据删除会将文件内容移动到区块存储中，并尝试尽可能多地按文件整理区块存储，当应用于连续的文件范围时，读取操作性能最佳。  

    与顺序读取模式相比，类似于数据库的工作负荷的读取模式通常更具随机性，因为数据库通常不保证数据库布局都最适合所有可能运行的查询。 因为区块存储的分区可能遍布整个卷，在区块存储中访问数据范围进行数据库查询时可能会导致额外延迟。 高性能工作负荷对此额外延迟特别敏感，但是，其他类似于数据库的工作负荷可能就不太介意。

    > [!Note]  
    > 这些问题主要适用于由传统旋转存储介质组成的存储工作负荷（也称为硬盘驱动器或 HDD）。 所有闪存存储基础结构（也称为固态磁盘驱动器或 SSD）不太受随机 I/O 模式影响，因为闪存介质的属性之一就是对介质上所有位置的访问时间相同。 因此，针对所有闪存介质上存储的工作负荷数据集执行读取操作时，重复数据删除不会引入与传统旋转存储介质相同的延迟时间。

3. **我的工作负荷在服务器上的资源需求是什么？**  
    由于重复数据删除使用后处理模型，重复数据删除需要定期具有足够的系统资源，才能完成其[优化和其他作业](understand.md#job-info)。 这意味着有空闲时间的工作负荷（如在夜间或周末情况下）都是重复数据删除的绝佳候选项，而工作负荷可能不会全天候运行。 没有空闲时间的工作负荷仍可能是重复数据删除的良好候选项（如果工作负荷在服务器上不具有高资源要求）。

### <a id="enable-dedup-lights-on"></a>启用重复数据删除
在启用重复数据删除之前，必须选择最类似于你的工作负荷的“[使用类型](understand.md#usage-type)”。 重复数据删除包含三种使用类型。

* [默认](understand.md#usage-type-default) - 优化专门用于一般用途文件服务器
* [HYPER-V](understand.md#usage-type-hyperv) -优化专门用于 VDI 服务器
* [备份](understand.md#usage-type-backup) - 优化专门用于虚拟备份应用程序，如 [Microsoft DPM](https://technet.microsoft.com/library/hh758173.aspx)

#### <a id="enable-dedup-via-server-manager"></a>使用服务器管理器启用重复数据删除
1. 选择“服务器管理器”中的“**文件和存储服务**”。  
@no__t 0Click 文件和存储服务 @ no__t-1
2. 从“**文件和存储服务**”中选择“**卷**”。  
![Click 卷 @ no__t-1
3. 右键单击所需的卷，并选择“**配置重复数据删除**”。  
@no__t 0Click 配置重复数据删除 @ no__t-1
4. 从下拉框中选择所需的“**使用类型**”，然后选择“**确定**”。  
@no__t-从下拉 @ no__t 中0Select 所需的使用类型
5. 如果运行的是建议的工作负荷，则操作完成。 对于其他工作负荷，请查看“[其他注意事项](#enable-dedup-sometimes-considerations)”。

> [!Note]  
> 有关排除文件扩展或文件夹以及选择重复数据删除计划（包括为何想要执行此操作）的详细信息，可在[配置重复数据删除](advanced-settings.md)中找到。

#### <a id="enable-dedup-via-powershell"></a>使用 PowerShell 启用重复数据删除
1. 在管理员上下文中，运行以下 PowerShell 命令：  
    ```PowerShell
    Enable-DedupVolume -Volume <Volume-Path> -UsageType <Selected-Usage-Type>
    ```

2. 如果运行的是建议的工作负荷，则操作完成。 对于其他工作负荷，请查看“[其他注意事项](#enable-dedup-sometimes-considerations)”。

> [!Note]  
> 通过向 CIM 会话追加 `-CimSession` 参数可远程运行重复数据删除 PowerShell cmdlet（包括 [`Enable-DedupVolume`](https://technet.microsoft.com/library/hh848441.aspx)）。 这特别适用于针对 Nano Server 实例远程运行重复数据删除 PowerShell cmdlet。 要创建新的 CIM 会话，请运行 [`New-CimSession`](https://technet.microsoft.com/library/jj590760.aspx)。

#### <a id="enable-dedup-sometimes-considerations"></a>其他注意事项
> [!Important]  
> 如果你运行的是建议的工作负荷，则可以跳过此部分。

* 重复数据删除的使用类型为建议的工作负荷提供合理的默认值，但还为所有工作负荷提供很好的起点。 对于建议的工作负荷以外的工作负荷，可能需要修改[重复数据删除的高级设置](advanced-settings.md)以提高重复数据删除性能。
* 如果你的工作负荷在服务器上具有高资源要求，应将重复数据删除作业[安排在工作负荷的预期空闲时间段内运行](advanced-settings.md#modifying-job-schedules-change-schedule)。 在超聚合主机上运行重复数据删除时这一点特别重要，因为在预期工作时间内运行重复数据删除会影响虚拟机。
* 如果你的工作负荷没有高资源要求，或者更重要的是优化作业完成而不是为工作负荷请求提供服务，[可以调整内存、CPU 和重复数据删除作业优先级](advanced-settings.md#modifying-job-schedules)。

## <a id="faq"></a>常见问题（FAQ）
@no__t 0 I 要在 X 工作负荷的数据集上运行重复数据删除。是否支持此功能？ **  
除了[已知不与重复数据删除互操作](interop.md)的工作负荷，针对任何工作负荷，我们完全支持重复数据删除的数据完整性。 Microsoft 还支持建议的工作负荷以实现更佳性能。 其他工作负荷的性能极大取决于它们正在你的服务器上执行哪些操作。 你必须确定重复数据删除对你的工作负荷具有哪些性能影响，并且确定这对于此工作负荷来说是否可以接受。

**删除了重复数据的卷的卷大小要求是什么？**  
在 Windows Server 2012 和 Windows Server 2012 R2 中，卷必须进行仔细大小调整，以确保重复数据删除可以与卷上的改动保持一致。 这通常意味着高改动工作负荷的删除重复卷的平均最大大小为 1-2 TB，且绝对最大建议大小为 10 TB。 在 Windows Server 2016 中，已取消这些限制。 有关详细信息，请参阅[重复数据删除中的新增功能](whats-new.md#large-volume-support)。

**是否需要修改建议的工作负荷的计划或其他重复数据删除设置？**  
不需要，提供的“[用法类型](understand.md#usage-type)”是为建议的工作负荷提供合理的默认值而创建的。

**重复数据删除的内存要求有哪些？**  
对于每 1 TB 的逻辑数据，重复数据删除应至少具备 300 MB + 50 MB 内存。 例如，如果要优化 10 TB 卷，需要分配最少 800 MB 内存进行重复数据删除 (`300 MB + 50 MB * 10 = 300 MB + 500 MB = 800 MB`)。 尽管重复数据删除可以优化较低内存量的卷，而此类约束资源会降低重复数据删除作业的速度。

在最佳情况下，对于每 1 TB 的逻辑数据，重复数据删除应具有 1 GB 的内存。 例如，如果要优化 10 TB 卷，最适合分配 10 GB 内存进行重复数据删除 (`1 GB * 10`)。 此比率将确保重复数据删除作业的最大性能。

**重复数据删除的存储要求有哪些？**  
在 Windows Server 2016 中，重复数据删除可以支持的卷大小高达 64 TB。 有关详细信息，请查看[重复数据删除中的新增功能](whats-new.md#large-volume-support)。

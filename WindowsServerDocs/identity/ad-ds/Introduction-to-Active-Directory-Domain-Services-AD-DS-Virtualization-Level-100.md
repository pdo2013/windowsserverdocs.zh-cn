---
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
title: 介绍 Active Directory 域服务 (AD DS) 虚拟化（级别 100）
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b818ba5a58db38bdb3c0f630a8d9d2daa1494403
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878088"
---
# <a name="introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100"></a>介绍 Active Directory 域服务 (AD DS) 虚拟化（级别 100）

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Active Directory 域服务 (AD DS) 环境的虚拟化多年来一直在进行。 从 Windows Server 2012 开始，AD DS 提供了更多虚拟化域控制器，通过引入虚拟化安全功能的支持。

## <a name="safe-virtualization-of-domain-controllers"></a>域控制器的安全虚拟化

虚拟环境对取决于基于逻辑时钟的复制方案的分布式工作负荷，提出了独特的挑战。 例如，AD DS 复制使用分配给每个域控制器上的事务的单调递增的值（称为 USN 或更新序列号）。 每个域控制器的数据库实例还提供一个标识，称为 InvocationID。 域控制器的 InvocationID 和其 USN 一起作为一个唯一标识符，该标识符与在每个域控制器上执行的每个写入事务相关联，并且在林中必须是唯一的。

AD DS 复制使用每个域控制器上的 InvocationID 和 USN 来确定需要复制到其他域控制器的更改。 如果域控制器域控制器感知之外及时回滚，并且 USN 重复用于完全不同的事务，因为其他域控制器会认为他们已经收到了更新，复制将无法聚合与该 InvocationID 的上下文下重复使用的 USN 相关联。

例如，下图显示了当在 VDC2（在虚拟机上运行的目标域控制器）上检测到 USN 回滚时在 Windows Server 2008 R2 和更早版本的操作系统上所发生事件的顺序。 在此图中，USN 回滚的检测时，发生 VDC2 上复制伙伴检测到 VDC2 已发送了以前看到的复制伙伴，这表示，VDC2 的数据库已及时回滚最新 USN 值不正确。

![对 AD DS 简介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

虚拟机 (VM) 可以轻松为虚拟机监控程序管理员回滚域控制器的 Usn （其逻辑时钟），例如，应用于域控制器感知之外的快照。 有关 USN 和 USN 回滚的详细信息，包括演示未检测到的 USN 回滚实例的另一个图，请参阅 [USN 和 USN 回滚](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback)。

从 Windows Server 2012 开始，将公开名为 VM 生成 ID 的标识符的虚拟机监控程序平台上托管的 AD DS 虚拟域控制器可以检测，并采取必要的安全措施保护 AD DS 环境，如果虚拟机将会回滚返回在应用程序的虚拟机快照的时间。 因为虚拟机生成 ID 设计使用虚拟机监控程序供应商独立机制在来宾虚拟机地址空间中显示此标识符，所以安全虚拟化体验对支持虚拟机生成 ID 的任何虚拟机监控程序来说始终可用。 如果虚拟机已及时回滚，则此标识符可以通过在虚拟机内运行的服务和应用程序进行采样加以检测。

### <a name="BKMK_HowSafeguardsWork"></a>这些虚拟化安全措施如何工作？
域控制器安装期间，AD DS 最初将存储的 VM 生成 Id 标识符作为在其数据库 （通常称为目录信息树或 DIT） 中的域控制器的计算机对象上的 msDS-GenerationID 属性的一部分。 虚拟机内的 Windows 驱动程序独立地跟踪虚拟机生成 ID。

当管理员从以前的快照还原虚拟机时，来自虚拟机驱动程序的虚拟机生成 ID 的当前值要与 DIT 中的值进行比较。

如果这两个值不相同，则重置 invocationID 并放弃 RID 池，从而防止重复使用 USN。 如果这两个值相同，则正常提交事务。

每次域控制器重新启动时，AD DS 也将来自虚拟机的虚拟机生成 ID 的当前值与 DIT 中的值进行比较，如果这两个值不相同，则它将重置 invocationID，放弃 RID 池，并使用新值更新 DIT。 它也非权威地同步 SYSVOL 文件夹，以完成安全还原。 这使安全措施扩展到关闭的虚拟机上快照的应用程序。 在 Windows Server 2012 中引入的这些安全措施使 AD DS 管理员能够从部署和管理虚拟化环境中的域控制器的独特优势中受益。

下图显示在支持 VM 生成 Id 的虚拟机监控程序运行 Windows Server 2012 的虚拟化的域控制器上检测到相同的 USN 回滚时如何应用虚拟化安全措施。

![对 AD DS 简介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

在这种情况下，当虚拟机监控程序检测到虚拟机生成 ID 值更改时，将触发虚拟化安全措施，包括重置虚拟化 DC 的 InvocationID（在前面的示例中从 A 到 B），更新保存在虚拟机上的虚拟机生成 ID 值以匹配虚拟机监控程序存储的新值 (G2)。 安全措施确保两个域控制器的复制聚合。

与 Windows Server 2012 AD DS 使用托管在 VM 生成 Id 感知的虚拟机监控程序上的虚拟域控制器上的安全措施，并确保意外的快照或其他此类虚拟机监控程序启用的机制，无法回滚的虚拟应用程序计算机的状态 （通过防止 USN 泡沫等复制问题或延迟对象） 不会中断 AD DS 环境。 但是，不建议将通过应用虚拟机快照还原域控制器作为备份域控制器为一种替代机制。 建议你继续使用 Windows Server Backup 或其他基于 VSS 书写器的备份解决方案。

> [!CAUTION]
> 如果在生产环境中的域控制器意外地还原到快照，则建议您向供应商咨询有关验证的状态后这些程序的指南，该虚拟机上承载的应用程序及服务快照还原。

有关详细信息，请参阅[虚拟化域控制器安全还原体系结构](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)。

## <a name="virtualized_dc_cloning"></a>虚拟化的域控制器克隆
从 Windows Server 2012 开始，管理员可以轻松安全地部署副本域控制器通过复制现有的虚拟域控制器。 在虚拟环境中，管理员不再需要重复部署使用 Sysprep.exe 准备的服务器映像，将服务器提升为域控制器，然后完成附加配置要求来部署每个副本域控制器。

> [!NOTE]
> 管理员需要遵循现有的流程来部署域中的第一个域控制器，如使用 sysprep.exe 来准备服务器虚拟硬盘 (VHD)，将服务器提升为域控制器，然后完成所有附加的配置要求。 在灾难恢复方案中，使用最新的服务器备份来还原域中的第一个域控制器。

### <a name="scenarios-that-benefit-from-virtual-domain-controller-cloning"></a>受益于虚拟域控制器克隆的方案

-   在新域中快速部署其他域控制器

-   通过使用克隆快速部署域控制器，还原 AD DS 功能，在灾难恢复过程中快速还原业务连续性

-   利用域控制器的弹性配置优化私有云部署，以适应增加的缩放要求

-   在生产部署之前，启用部署和测试新特性和功能来快速设置测试环境

-   通过克隆分支机构中现有的域控制器，迅速满足分支机构中不断增长的容量需求

迅速部署大量域控制器时，要继续遵循现有的程序，在安装完成后验证每个域控制器的运行状况。 以合理的规模批次部署域控制器，以便在每批安装完成后，你可以验证它们的运行状况。 推荐的批次大小为 10 个。 有关详细信息，请参阅[部署克隆虚拟化域控制器的步骤](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc)。

### <a name="clear-separation-of-responsibilities"></a>明确职责分离
要在 AD DS 管理员控制之下授权克隆虚拟化域控制器。 为了让虚拟机监控程序管理员通过复制虚拟域控制器来部署其他域控制器，AD DS 管理员必须选择和授权一个域控制器，然后运行准备步骤，使它作为克隆源。

由于通常在虚拟机监控程序管理员的权限下配置虚拟机，所以虚拟机监控程序管理员可以通过复制已由 AD DS 管理员授权并准备进行克隆的虚拟化域控制器来配置副本域控制器虚拟机。

> [!WARNING]
> 允许管理托管虚拟域控制器的虚拟机监控程序的任何人都必须在环境中高度受信任且要进行审核。

### <a name="how-does-virtual-domain-controller-cloning-work"></a>虚拟域控制器克隆如何工作？
克隆过程涉及复制的现有虚拟域控制器的 VHD （或更复杂的配置，域控制器 VM），授权它在 AD DS 中克隆和创建克隆配置文件。 通过消除其它重复部署任务，这将减少部署副本虚拟域控制器中包含的步骤数和时间。

克隆域控制器使用以下条件来检测它是另一个域控制器的副本：

1.  虚拟机提供的虚拟机生成 ID 值与存储在 DIT 中虚拟机生成 ID 值不同。

    > [!NOTE]
    > 虚拟机监控程序平台必须支持 VM 生成 ID （Windows Server 2012 HYPER-V 支持 VM 生成 ID）。

2.  在下列位置之一存在称为 DCCloneConfig.xml 的文件：

    -   DIT 所在的目录

    -   %windir%\NTDS

    -   可移动媒体驱动器的根目录

一旦满足条件，它就会进行克隆以将其本身配置为副本域控制器。

克隆域控制器使用源域控制器 （域控制器表示其副本） 的安全上下文联系 Windows Server 2012 主域控制器 (PDC) 模拟器操作主机角色担任者 （也称为灵活单主机操作或 FSMO）。 PDC 仿真器必须运行 Windows Server 2012，但它不需要在虚拟机监控程序上运行。

> [!NOTE]
> 如果你有一个引用源域控制器属性的架构扩展，且该属性是在上一个对象（计算机对象，NTDS 设置对象）上，可以通过复制该对象来创建克隆，则该属性将不复制和不更新，以引用克隆域控制器。

在验证请求的域控制器得到克隆授权之后，PDC 仿真器将创建新机标识，包括新帐户、SID、名称和密码，用于标识此机为副本域控制器，并将此信息发送回克隆。 然后，克隆域控制器将准备 AD DS 数据库文件，将其用作副本，它还将清理计算机状态。

有关详细信息，请参阅 [Virtualized domain controller cloning architecture](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch)。

### <a name="cloning-components"></a>克隆组件
克隆组件包括用于 Windows PowerShell 的 Active Directory 模块中的新 cmdlet 和相关的 XML 文件：

-   **New-addccloneconfigfile** "此 cmdlet 创建，并将 DCCloneConfig.xml 放在适当的位置，以确保它是可用来触发克隆。 它还执行先决条件检查，以确保成功克隆。 它包括在用于 Windows PowerShell 的 Active Directory 模块中。 你可以在准备进行克隆的虚拟化域控制器上本地运行它，你也可以使用 –offline 选项远程运行它。 你可以指定克隆域控制器的设置，如其名称、站点和 IP 地址。

    执行的先决条件检查是：

    > [!NOTE]
    > 先决条件检查不是执行时"使用 offline 选项。 有关详细信息，请参阅[在脱机模式下运行 New-ADDCCloneConfigFile](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode)。

    -   正准备的 DC 得到克隆授权（是**Cloneable Domain Controllers**组的成员）

    -   PDC 仿真器运行 Windows Server 2012。

    -   从运行的 **Get-ADDCCloningExcludedApplicationList** 列出的任何程序或服务都包含在 CustomDCCloneAllowList.xml 中（在此克隆组件列表的末尾有更详细的解释）。

-   **DCCloneConfig.xml** "若要成功克隆虚拟化的域控制器，此文件必须存在于 DIT 所在的目录 *%windir%\NTDS*，或可移动媒体驱动器的根目录。 除了用作一个触发器以检测和启动克隆外，它还提供指定克隆域控制器的配置设置的方法。

    在所有 Windows Server 2012 计算机上存储的架构和 DCCloneConfig.xml 文件的示例文件：

    -   %windir%\system32\DCCloneConfigSchema.xsd

    -   %windir%\system32\SampleDCCloneConfig.xml

    建议使用 New-ADDCCloneConfigFile cmdlet 来创建 DCCloneConfig.xml 文件。 虽然你也可以在 XML 感知的编辑器中使用架构文件来创建此文件，但手动编辑该文件会增加出错的可能性。 如果你要编辑该文件，那么你必须使用 XML 感知的编辑器，如 Visual Studio、 [XML 记事本](https://www.microsoft.com/download/details.aspx?displaylang=en&id=7973)或第三方应用程序（不使用记事本）。

-   **Get-ADDCCloningExcludedApplicationList** "运行此 cmdlet 在源域控制器克隆过程来确定哪些服务或已安装的程序不支持的默认列表中，在开始之前DefaultDCCloneAllowList.xml 或用户定义的包含列出名为的 CustomDCCloneAllowList.xml 文件，并从而未被用于克隆的影响评估。

    此 cmdlet 搜索源域控制器，查找服务控制管理器中的服务和在 **HKLM\Software\Microsoft\Windows\CurrentVersion\Uninstall** 下列出的已安装的程序，它们未指定在默认列表 (DefaultDCCloneAllowList.xml) 或者用户定义的包含列表（CustomDCCloneAllowList.xml 文件）（若有）中。 运行该 cmdlet 会返回应用程序和服务列表，该列表反映了在 DefaultDCCloneAllowList.xml 或 CustomDCCloneAllowList.xml 文件中已提供内容和根据在源域控制器上安装的内容在运行时构造的列表之间的差异。 如果你确定可以安全地克隆服务和程序，则可以将来自 Get-ADDCCloningExcludedApplicationList 的服务和程序输出添加到 CustomDCCloneAllowList.xml 文件中。 若要确定是否可以安全地克隆某个服务或已安装的程序，请评估下列条件：

    -   服务或已安装的程序是否受计算机标识（如名称、SID、密码等等）影响？

    -   在本地计算机上，该服务或已安装的程序是否存储了可能影响其克隆功能的任何状态？

    你必须与应用程序的软件供应商合作，以确定是否可以安全地克隆服务和程序。

    > [!NOTE]
    > 在 CustomDCCloneAllowList.xml 文件中配置其他服务或程序之前，请验证你是否具有复制该虚拟机包含的软件的必需许可证。

    如果应用程序不可以克隆，则应在创建克隆媒体之前，从源域控制器中将其删除。 如果应用程序出现在 cmdlet 输出中，但不包含在 CustomDCCloneAllowList.xml 文件中，则克隆将会失败。 为成功克隆，cmdlet 输出不应列出任何服务或程序。 换句话说，应用程序应要么包含在 CustomDCCloneAllowList.xml 文件中，要么从源域控制器中删除。

    下表说明了运行 Get-ADDCCloningExcludedApplicationList 的选项。

    |||
    |-|-|
    |参数|说明|
    |*<no argument specified>*|在控制台上显示还没有考虑克隆的服务或程序列表。 如果在任何允许的位置已存在 CustomDCCloneAllowList.XML，则将使用该文件来显示剩余的服务和程序（如果列表匹配，则可能没有剩余）。|
    |-GenerateXml|创建 CustomDCCloneAllowList.XML 文件，并填写控制台上列出的服务和程序。|
    |-Force|覆盖现有的 CustomDCCloneAllowList.XML 文件。|
    |-Path|创建 CustomDCCloneAllowList.XML 的文件夹路径。|

-   **DefaultDCCloneAllowList.xml** "此文件是否存在默认情况下，在每个 Windows Server 2012 域控制器 in *%windir%\system32*。 在默认情况下，此文件会列出可以安全克隆的服务和已安装的程序。 你不得更改此文件的位置和内容，否则克隆将会失败。

-   **CustomDCCloneAllowList.xml** "如果你有服务或已安装的程序驻留在你的源域控制器上在 DefaultDCCloneAllowList.xml 文件中列出的内容之外，这些服务和程序必须包含在此文件。 若要查找没有在 DefaultDCCloneAllowList.xml 文件中列出的服务或已安装的程序，请运行 **Get -ADDCCloningExcludedApplicationList** cmdlet。 应使用 **"GenerateXml**参数来生成 XML 文件。

    克隆过程会检查以下位置来查找此文件，并使用最先找到的 XML 文件，不管其他文件夹中的内容：

    1.  以下注册表项：

        ```
        HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters
        AllowListFolder (REG_SZ)
        ```

    2.  DSA 工作目录

    3.  %systemroot%\NTDS

    4.  按照驱动器号顺序排列的可移动读/写媒体，位于驱动器根目录下

### <a name="deployment-scenarios"></a>部署方案
以下部署方案支持虚拟域控制器克隆：

-   源域控制器的虚拟硬盘 (vhd) 文件的副本，从而部署克隆域控制器。

-   通过使用虚拟机监控程序的导出/导入语义，复制源域控制器虚拟机，部署克隆域控制器。

> [!NOTE]
> 部分中的步骤[部署克隆虚拟化的域控制器的步骤](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc)演示复制使用导出/导入功能的 Windows Server 2012 的 HYPER-V 虚拟机。

## <a name="steps_deploy_vdc"></a>部署克隆虚拟化的域控制器的步骤

-   [必备条件](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#prerequisites)

-   [步骤 1：授予源虚拟化的域控制器克隆权限](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk4_grant_source)

-   [步骤 2：Run Get-ADDCCloningExcludedApplicationList cmdlet](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet)

-   [步骤 3：运行 New-addccloneconfigfile](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk5_create_insert_dccloneconfig)

-   [步骤 4：导出并导入源域控制器的虚拟机](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk7_export_import_vm_sourcedc)

### <a name="prerequisites"></a>系统必备组件

-   若要完成以下过程中的步骤，你必须是 Domain Admins 组的成员，或者拥有与分配给 Domain Admins 组成员的权限同等的权限。

-   本指南中使用的 Windows PowerShell 命令必须从提升的命令提示符运行。 若要执行此操作，右键单击**Windows PowerShell**图标，，然后单击**以管理员身份运行**。

-   具有安装了 HYPER-V 服务器角色的 Windows Server 2012 服务器 (**HyperV1**)。

-   具有安装了 HYPER-V 服务器角色的第二个 Windows Server 2012 服务器 (**HyperV2**)。

    > [!NOTE]
    > -   如果正使用另一个虚拟机监控程序，应该联系该虚拟机监控程序的供应商，验证虚拟机监控程序是否支持虚拟机生成 ID。 如果虚拟机管理程序不支持虚拟机生成 ID，并且你已经提供了 DCCloneConfig.xml，则新的 VM 将引导到目录服务还原模式 (DSRM)。
    > -   为了提高 AD DS 服务的可用性，本指南建议使用两种不同的 HYPER-V 主机（这有助于防止潜在的单点故障）并提供了相应指导。 但是，你不需要两个 HYPER-V 主机来执行虚拟域控制器克隆。
    > -   在每个 Hyper-V 服务器（**HyperV1** 和 **HyperV2**）上，你必须是本地管理员组成员。
    > -   为了使用 Hyper-V 成功地导入和导出 VHD 文件，这两台 HYPER-V 主机上的虚拟网络交换机应同名。 例如，如果你在名为 VNet 的 **HyperV1** 上有一个虚拟网络交换机，那么，在名为 VNet 的 **HyperV2** 上就需要有一个虚拟网络交换机。
    > -   如果两个 HYPER-V 主机（**HyperV1** 和 **HyperV2**）具有不同的处理器，关闭计划要导出的虚拟机（**VirtualDC1**），那么，请用鼠标右键单击虚拟机，单击**设置**以及**处理器**，然后在**处理器兼容性**下，选择**迁移到具有不同处理器版本的物理计算机**，最后单击**确定**。

-   部署的 Windows Server 2012 域控制器 （虚拟或物理） 承载 PDC 仿真器角色 (**DC1**)。 若要验证的 Windows Server 2012 域控制器上是否承载 PDC 仿真器角色，请运行以下 Windows PowerShell 命令：

    ```
    Get-ADComputer (Get-ADDomainController "Discover "Service "PrimaryDC").name "Property operatingsystemversion | fl
    ```

    OperatingSystemVersion 的值应返回为 6.2 版本。 如有必要，您可以将 PDC 仿真器角色转移到运行 Windows Server 2012 的域控制器。 有关详细信息，请参阅 [使用 Ntdsutil.exe 转移或占用FSMO 角色到域控制器](https://support.microsoft.com/kb/255504)。

-   已部署的 Windows Server 2012 来宾虚拟化的域控制器 (**VirtualDC1**) 与托管 PDC 仿真器角色的 Windows Server 2012 域控制器位于同一域中 (**DC1**)。 这将是用于克隆的源域控制器。 将 Windows Server 2012 的 HYPER-V 服务器上托管来宾虚拟域控制器 (**HyperV1**)。

    > [!NOTE]
    > -   为了成功克隆，对于用来创建克隆的源域控制器，不能来自已降级的 DC，因为已创建了源 VHD 媒体。
    > -   在复制 VM 或其 VHD 之前关闭源域控制器。
    > -   不应克隆 VHD 或还原超过逻辑删除生存期值或已删除对象的生存期值（在启用 Active Directory 回收站情况下）的快照。 如果正在复制现有域控制器的 VHD，则请确定 VHD 文件不会超过逻辑删除生存期值（默认情况下为 60 天）。 对于正在运行的域控制器的 VHD，不应复制它来创建克隆媒体。

    弹出源 DC 可能有的任何虚拟软盘驱动器 (VFD)。 当尝试导入新的 VM 时，这会导致共享问题。

    托管的 VM 生成 Id 虚拟机监控程序的仅 Windows Server 2012 域控制器可以用作任务的源进行克隆。 用于克隆的源 Windows Server 2012 域控制器应处于正常状态。 若要确定源域控制器的状态，请运行 [dcdiag](https://technet.microsoft.com/library/cc731968(WS.10).aspx)。 若要获得更好地了解由 dcdiag 返回的输出，请参阅[DCDIAG 到底做什么...执行操作？](http://blogs.technet.com/b/askds/archive/2011/03/22/what-does-dcdiag-actually-do.aspx).

    如果源域控制器是 DNS 服务器，那么克隆的域控制器也将是 DNS 服务器。 你应选择只托管 Active Directory 集成区域的 DNS 服务器。

    不会克隆 DNS 客户端设置，但会在 DCCloneConfig.xml 文件中指定它。 如果没有指定它们，则在默认情况下，克隆的域控制器将指定自身作为首选的 DNS 服务器。 克隆的域控制器不会有 DNS 委派。 父 DNS 区域的管理员应该按需要更新已克隆域控制器的 DNS 委派。

    > [!WARNING]
    > 虚拟化安全措施并不会扩展到 Active Directory 轻型目录服务 (AD LDS）。 因此，对于托管 AD LDS 实例的 AD DS 域控制器，不应尝试通过将此 AD LDS 实例添加到 CustomDCCloneAllowList.xml 来克隆它。 因为 AD LDS 不能感知虚拟机生成 ID，所以克隆含 AD LDS 的域控制器可能会在 AD LDS 配置集上导致 USN 回滚引起的分歧。

    以下服务器角色不支持克隆：

    -   动态主机配置协议 (DHCP)

    -   Active Directory 证书服务 (AD CS)

    -   Active Directory 轻型目录服务 (AD LDS)

### <a name="bkmk4_grant_source"></a>步骤 1:授予源虚拟化域控制器克隆权限
在此过程中，你通过使用 **Active Directory 管理中心**将源域控制器添加到**Cloneable Domain Controllers**组中，来授予源域控制器克隆权限。

##### <a name="to-grant-the-source-virtualized-domain-controller-the-permission-to-be-cloned"></a>授予源虚拟化域控制器克隆权限

1.  在正在准备克隆域控制器的同域中的任何域控制器 (**VirtualDC1**) 上，打开“Active Directory 管理中心”  (ADAC)、查找虚拟化域控制器对象（域控制器通常位于 ADAC 中的“域控制器”  容器中）、右键单击它、选择“添加到组”  并在“输入要选择的对象名称”  下面键入 **Cloneable Domain Controllers** ，然后单击“确定” 。

    在可以执行克隆之前，在此步骤中执行的组成员身份更新必须复制到 PDC 仿真器。 如果**Cloneable Domain Controllers**找不到组中，PDC 仿真器角色不可能托管在运行 Windows Server 2012 的域控制器上。

    > [!NOTE]
    > 若要在 Windows Server 2012 域控制器上打开 ADAC，打开 Windows PowerShell 并键入**dsac.exe**。

![对 AD DS 简介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 * * *

以下 Windows PowerShell cmdlet 执行与上述步骤中相同的功能：


    Add-ADGroupMember "Identity "CN=Cloneable Domain Controllers,CN=Users, DC=Fabrikam,DC=Com" "Member "CN=VirtualDC1,OU=Domain Controllers,DC=Fabrikam,DC=com"


### <a name="bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet"></a>步骤 2:运行 Get-ADDCCloningExcludedApplicationList cmdlet
在此过程中，在源虚拟域控制器上运行 `Get-ADDCCloningExcludedApplicationList` cmdlet来确定不评估克隆的任何程序或服务。 你需要在 New-ADDCCloneConfigFile cmdlet 之前运行 Get-ADDCCloningExcludedApplicationList cmdlet，因为如果 New-ADDCCloneConfigFile cmdlet 检测到已排除的应用程序，则它将不会创建 DCCloneConfig.xml 文件。

##### <a name="to-identify-applications-or-services-that-run-on-a-source-domain-controller-which-have-not-been-evaluated-for-cloning"></a>若要确定应用程序或服务的源域控制器上运行的未用于克隆评估

1.  在源域控制器 (**VirtualDC1**) 上，依次单击 **“服务器管理器”**、**“工具”** 和 **“用于 Windows PowerShell 的 Active Directory 模块”**，然后键入以下命令：


    Get-ADDCCloningExcludedApplicationList


2.  与软件供应商联系，检查返回的服务和已安装的程序列表，以确定他们是否可以安全地克隆。 如果不能安全地克隆列表中的应用程序或服务，则必须从源域控制器中删除它们，否则克隆将会失败。

3.  获取的服务并确定可以安全克隆的已安装的程序集，运行该命令再次使用 **"GenerateXML**开关来配置这些服务和程序中**CustomDCCloneAllowList.xml**文件。


    Get-ADDCCloningExcludedApplicationList -GenerateXml


### <a name="bkmk5_create_insert_dccloneconfig"></a>步骤 3:运行 New-ADDCCloneConfigFile
在源域控制器上运行 New-ADDCCloneConfigFile，同时可以选择为克隆域控制器指定配置设置（如名称、IP 地址和 DNS 解析程序）。

例如，要用静态 IPv4 地址创建名为 VirtualDC2 的克隆域控制器，请键入：


    New-ADDCCloneConfigFile "Static -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.255.0" -CloneComputerName "VirtualDC2" -IPv4DefaultGateway "10.0.0.3" -SiteName "REDMOND"

> [!NOTE]
> 克隆域控制器将位于与源域控制器相同的站点中，除非在 DCCloneConfig.xml 文件中指定了不同的站点。 建议在 DCCloneConfig.xml 文件中指定合适的站点作为基于其 IP 地址的克隆域控制器。

计算机名为可选项。 如果你没有指定名称，则将基于下面的算法生成一个唯一的名称：

-   前缀是源域控制器计算机名称的前 8 个字符。 例如，如源计算机名为 SourceComputer，则将截断为前缀字符串 SourceCo。

-   唯一的命名后缀格式""CL*nnnn*"追加到的前缀字符串位置*nnnn*是 PDC 确定当前未使用的 0001-9999 之间的下一个可用值。 例如，如果 0047 是在允许的范围内的下一个可用编号，则利用前面示例中的计算机名称前缀 SourceCo，会将用于克隆计算机的派生名称设置为 SourceCo-CL0047。

> [!NOTE]
> 对于 New-ADDCCloneConfigFile cmdlet，需要使用全局编录 (GC) 服务器才能顺利运行。 中的源域控制器的成员身份**Cloneable Domain Controllers**组必须反映到 GC 上。 GC 不必是与 PDC 仿真器相同的域控制器，但它最好处于同一站点中。 如果 GC 不可用，该命令将失败并出现错误"服务器不可操作。" 有关详细信息，请参阅 [Virtualized Domain Controller Troubleshooting](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md)。

要创建名为 Clone1 的、使用静态 IPv4 设置的克隆域控制器，同时指定首选和备用 WINS 服务器，则请键入：


    New-ADDCCloneConfigFile "CloneComputerName "Clone1" "Static -IPv4Address "10.0.0.5" "IPv4DNSResolver "10.0.0.1" "IPv4SubnetMask "255.255.0.0" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


> [!NOTE]
> 如果指定了 WINS 服务器，则必须指定这两 **"PreferredWINSServer**并 **"AlternateWINSServer**。 如果仅指定上述参数之一，则克隆将失败，并在 dcpromo.log 中记录错误代码 0x80041005。

若要创建名为 Clone2 的、使用动态 IPv4 设置的克隆域控制器，则请键入：


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" 


> [!NOTE]
> 在此情况下，在环境中应该有 DHCP 服务器，这样克隆才可以访问和获得 IP 地址和其他相关网络设置。

要创建名为 Clone2 的、使用动态 IPv4 设置的克隆域控制器，同时指定首选和备用 WINS 服务器，请键入：


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" -SiteName "REDMOND" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


要创建使用动态 IPv6 设置的克隆域控制器，请键入：


    New-ADDCCloneConfigFile -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


要创建使用静态 IPv6 设置的克隆域控制器，请键入：


    New-ADDCCloneConfigFile "Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


> [!NOTE]
> 在指定 IPv6 设置时，静态与动态设置之间的唯一区别是包含 **-静态**切换。 包含的 **-静态**开关时，它指定至少一个**IPv6DNSResolver**。应通过无状态地址自动配置 (SLAAC) 与路由器分配前缀配置静态 IPv6 地址。 使用动态 IPv6，则 DNS 解析程序是可选的但应克隆，可以联系上的子网获取 IPv6 地址和 DNS 配置信息的启用 IPv6 的 DHCP 服务器。

#### <a name="BKMK_OfflineMode"></a>在脱机模式下运行 New-addccloneconfigfile
如果拥有已准备克隆 （意味着源域控制器得到克隆授权，且 Get-ADDCCloningExcludedApplicationList cmdlet 已经运行等等） 源域控制器媒体的多个副本，并且要为每个媒体副本指定不同的设置，则可以在脱机模式下运行 New-ADDCCloneConfigFile。 这比单独准备每个 VM（例如，通过导入每个副本）效率更高。

在这种情况下，域管理员可以装入脱机磁盘并使用远程服务器管理工具 (RSAT) 运行 New-addccloneconfigfile cmdlet 与-offline 参数以便将 XML 文件，这允许工厂式的自动化使用新添加包括在 Windows Server 2012 的 Windows PowerShell 选项。 有关如何装入脱机磁盘以便在脱机模式下运行 New-ADDCCloneConfigFile cmdlet 的详细信息，请参阅 [Adding XML to the Offline System Disk](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_Offline)。

在源媒体上，应首先以本地模式运行 cmdlet，确保该先决条件检查通过。 不能在脱机模式下执行先决条件检查，因为对于非来自同域或已加入域的计算机，均可从中运行此 cmdlet。 在以本地方式运行 cmdlet 后，它将创建 DCCloneConfig.xml 文件。 如果随后计划使用脱机模式，则可以删除在本地创建的 DCCloneConfig.xml。

若要创建的克隆域控制器名为 CloneDC1 在脱机模式下，名为 REDMOND 站点中"带有静态 IPv4 地址，类型：


    New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS


要在脱机模式下，使用静态 IPv4 和 IPv6 的静态设置创建名为 Clone2 的克隆域控制器，请键入：


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS


要在脱机模式下，使用静态 IPv4 和 动态 IPv6 设置创建克隆域控制器，同时指定针对 DNS 解析程序设置的 DNS 服务器，请键入：


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS 


要在脱机模式下用动态 IPv4 和静态 IPv6 设置创建名为 Clone1克隆域控制器，请键入：


    New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS


要在脱机模式下用动态 IPv4 和动态 IPv6 设置创建克隆域控制器，请键入：


    New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS


### <a name="bkmk7_export_import_vm_sourcedc"></a>步骤 4:导出之后再导入源域控制器虚拟机
在这一过程中，导出源虚拟域控制器的虚拟机，然后导入虚拟机。 此操作可在域中创建克隆虚拟域控制器。

需要成为每个 Hyper-V 主机上本地管理员组的成员才能操作。 如果为每个服务器使用不同的凭据，请运行 Windows PowerShell cmdlet 来在不同的 Windows PowerShell 会话中导出和导入 VM。

如果源域控制器上有快照，应该在源域控制器被导出之前删除它们，因为如果快照带有与目标  hyper-v 主机不兼容的处理器设置，则 VM 将不会导入。 如果源和目标 hyper-v 主机之间的处理器设置能够兼容，那么无需事先删除快照即可导出并复制来源。 然而导入后，在克隆 VM 启动前必须从中删除快照。

##### <a name="to-copy-a-virtual-domain-controller-by-exporting-and-then-importing-the-virtualized-source-domain-controller"></a>通过导出然后导入虚拟源域控制器来复制虚拟域控制器

1.  在 **HyperV1** 上，关闭源域控制器 (**VirtualDC1**)。

    ![对 AD DS 简介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 * * *

    停止 VM-命名 VirtualDC1-ComputerName HyperV1


2.  在 **HyperV1** 上，删除快照然后将源域控制器 (VirtualDC1) 导出到 c:\CloneDCs 目录。

> [!NOTE]
> 你应该删除所有相关快照，因为每次拍摄快照时，会创建一个新的 AVHD 文件作为不同的磁盘。 这样可创建一个连锁效应。 如果你已经拍摄了快照并将 DCCLoneConfig.xml 文件插入 VHD，你可以结束从旧版 DIT 创建一个克隆或者将配置文件插入错误的 VHD 文件中。 删除该快照会将这些 AVHD 全部合并到基本 VHD 中。

![对 AD DS 简介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 * * *


    Get-VMSnapshot VirtualDC1 | Remove-VMSnapshot -IncludeAllChildSnapshots
    Export-VM -Name VirtualDC1 -ComputerName HyperV1 -Path c:\CloneDCs\VirtualDC1


3.  将 **virtualdc1** 文件夹复制到 **HyperV2** 的 c:\Import 目录。

4.  在 **HyperV2**上，使用 **Hyper-V 管理器**，从文件夹 **c:\Import\virtualdc1** 导入虚拟机（使用 **Hyper-V 管理器**中的 **导入虚拟机** 向导）并删除所有相关的 **快照**。

导入虚拟机时，使用**复制虚拟机（创建新的唯一 ID）** 选项。

![对 AD DS 简介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 * * *

    $path = Get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual Machines"
    $vm = Import-VM -Path $path.fullname -Copy -GenerateNewId
    Rename-VM $vm VirtualDC2


从同一源域控制器创建多个克隆域控制器：

  -   UI：在 **导入虚拟机** 向导中，为 **虚拟机配置文件夹**、 **快照存储**、 **智能分页文件夹**指定新位置，以及一个面向虚拟硬盘虚拟机的不同的 **位置** 。

  -   Windows PowerShell： 使用以下参数指定的虚拟机的新位置`Import-VM`cmdlet:

        $path = Get-childitem"C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual 机"导入-VM-路径 $path.fullname-复制-GenerateNewId-ComputerName HyperV2-VhdDestinationPath"路径"-SnapshotFilePath"路径"-SmartPagingFilePath"路径"-VirtualMachinePath"路径"


> [!NOTE]
> 推荐使用 10 作为同时创建多个克隆域控制器的批处理大小。 最大数量受到出站复制连接最大数量的限制，后者的默认设置是分布式文件系统复制 (DFSR) 为 16，而文件复制服务 (FRS) 为 10。 应该部署不超过推荐数量的同时克隆域控制器，除非已经全面测试过该数量适合你的环境。

5.  在 **HyperV1**上，重启源域控制器 (**(VirtualDC1**) 使其重新上线。

![对 AD DS 简介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 * * *

    Start-VM -Name VirtualDC1 -ComputerName HyperV1


6.  在 **HyperV2**上，启动虚拟机 (**VirtualDC2**) 将其作为域中的克隆域控制器上线。

![对 AD DS 简介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 * * *


    Start-VM -Name VirtualDC2 -ComputerName HyperV2

> [!NOTE]
> PDC 仿真器必须一直运行，保证克隆成功。 如果关机，要确保它已经启动并执行初始同步，以便它能够认识到承担着 PDC 仿真器角色。 有关详细信息，请参阅 Microsoft [知识库文章 305476](https://support.microsoft.com/kb/305476)。

克隆完成后，验证克隆计算机的名称以确保克隆操作的成功。 验证 VM 没有在目录服务还原模式 (DSRM) 中启动。 如果你尝试登录并接收到一个说明无登录服务器可用的错误，请尝试在 DSRM 中登录。 如果 DC 没有成功克隆而且已经在 DSRM 中启动，请检查 “事件查看器” 中的日志和  %systemroot%/debug 文件夹中的 dcpromo 日志。

克隆的域控制器将成为**Cloneable Domain Controllers**组的成员，因为它从源域控制器复制了成员资格。 作为最佳实践，你应该使**Cloneable Domain Controllers**组保持空白，直到你准备执行克隆操作为止，而且你应该在克隆操作完成之后删除成员。

如果源域控制器可以存储备份媒体，那么克隆的域控制器也能够存储。 你可以运行 `wbadmin get versions` 来在克隆的域控制器上显示备份媒体。 Domain Admins 组的成员应该在克隆的域控制器上删除备份媒体，以防止其被意外恢复。 有关如何使用 wbadmin.exe 删除系统状态备份的详细信息，请参阅 [Wbadmin delete systemstatebackup](https://technet.microsoft.com/library/cc742081(v=WS.10).aspx)。

## <a name="troubleshooting"></a>故障排除
如果克隆域控制器 (**VirtualDC2**) 在目录服务恢复模式 (DSRM) 中启动，那么在下次重启时它不会自行返回正常模式。 若要登录在 DSRM 中启动的域控制器，使用 **.\Administrator** 并指定 DSRM 密码。

修改克隆错误的原因，验证 dcpromo.log 没有表明不能再次运行克隆。 如果克隆不能被再次运行，请安全丢弃媒介。 如果克隆可以被再次运行，为了再次尝试克隆，你必须删除 DS 恢复模式标记。

1.  打开 Windows Server 2012 （右侧单击 Windows Server 2012 和选择以管理员身份运行），提升的命令，然后按**msconfig**。

2.  在 **启动** 选项卡的 **启动选项**中，清除 **安全启动** （已经选择 **Active Directory 修复已启用**选项）。

3.  得到提示后点击 **确定** 然后重新启动。

有关虚拟域控制器的更多疑难解答信息，请参阅[虚拟域控制器疑难解答](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md)。



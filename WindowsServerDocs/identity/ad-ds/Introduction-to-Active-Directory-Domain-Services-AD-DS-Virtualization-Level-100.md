---
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
title: "简介 Active Directory 域服务 (广告 DS) 虚拟化 (级别 100)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3c7fdc2e20bc4a27f2555af54f396a15f664d6c7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100"></a>简介 Active Directory 域服务 (广告 DS) 虚拟化 (级别 100)

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

多年一直持续虚拟化 Active Directory 域服务 (广告 DS) 环境。 广告 DS 自 Windows Server 2012 起，提供更广泛的支持于虚拟通过引入了虚拟化安全功能，并启用快速部署到复制的虚拟域控制器域控制器。 这些新的虚拟化功能提供公开并专用云霞的更出色支持，混合环境部分广告 DS 存在本地并在云中进行构建，完全驻留的广告 DS 基础结构本地。

**本文档**

-   [安全虚拟化域控制器](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#safe_virt_dc)

-   [复制虚拟化的域控制器](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#virtualized_dc_cloning)

-   [部署克隆虚拟化的域控制器的步骤](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc)

-   [故障排除](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#troubleshooting)

## <a name="safe_virt_dc"></a>安全虚拟化域控制器
虚拟环境出示到分布式取决于逻辑时钟基于复制方案的工作负载的唯一难题。 广告 DS 复制，例如，使用赋予交易记录每个该域控制器上一直越来越值（称为 USN 或更新序列号）。 每个域控制器数据库实例也是授予权限称为 InvocationID 的身份。 域控制器 InvocationID 和其 USN 一起作为执行每个该域控制器上与每写入交易关联的唯一标识符，并且必须森林中的唯一。

广告 DS 复制使用 InvocationID 和 Usn 域控制器上来确定需要进行复制到其他域控制器的更改。 如果 USN 完全不同的交易重复使用域控制器回滚的域控制器感知之外的时间，因为将认为其他域控制器，它们已经收到更新重复使用在该 InvocationID 上下文 USN 与相关联，将不汇聚复制。

例如下, 图显示发生事件的序列在 Windows Server 2008 R2 和之前的操作系统 USN 回退 VDC2、虚拟机运行的目的地域控制器上检测到时。 在此示例中，USN 回退检测时发生上 VDC2 复制合作伙伴检测到 VDC2 已发送所看到的那样之前复制合作伙伴，指示该 VDC2 的数据库具有回滚的时间不正确的最新 USN 值。

![广告 DS 简介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

虚拟机 (VM) 便于虚拟机监控程序管理员回滚域控制器的 Usn（它的逻辑时钟），例如，应用之外的域控制器感知快照。 有关 USN 和 USN 的详细信息回退，包括其他图演示的 USN 回退未检测到的实例看到[USN 和 USN 回退](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback)。

开始与 Windows Server 2012、广告 DS 虚拟域控制器托管了虚拟机监控程序公开标识符称为 VM-Generation ID 的平台可以检测并采取必要的安全措施来保护广告 DS 环境，如果虚拟机回滚的时间 VM 快照的应用上。 VM-GenerationID 设计用于虚拟机监控程序供应商独立机制公开地址空间中的访客虚拟机，此标识符，以便安全虚拟化体验的支持 VM-GenerationID 任何虚拟机监控程序始终可用。 通过运行虚拟机中的应用程序检测如果虚拟机已回退时间和服务，可以采样此标识符。

### <a name="BKMK_HowSafeguardsWork"></a>这些虚拟化安全措施如何工作？
域控制器在安装期间，广告 DS 最初存储 VM GenerationID 标识符作为上的域控制器（通常称为目录信息树或 DIT）与其数据库中的计算机对象 msDS GenerationID 特性的一部分。 VM GenerationID 独立由 Windows 驱动程序虚拟机中跟踪。

当管理员还原以前的快照从虚拟机时，虚拟机驱动程序从 VM GenerationID 的当前值将比较 DIT 中的值。

如果这两个值不同，invocationID 被重置和 RID 池丢弃重新使用从而防止 USN。 如果这些值正确相同，交易致力正常。

广告 DS 还每次域控制器重新启动后，如果并不同，它将重置 invocationID，丢弃 RID 池更新 DIT 使用新值比较 VM GenerationID 从根据 DIT 中的值虚拟机的当前值。 它还非-授权才能完成安全还原同步 SYSVOL 文件夹。 这使扩展到快照上已关机虚拟机的功能的应用程序的安全措施。 引入 Windows Server 2012 这些安全措施启用广告 DS 管理员从中受益部署和管理域控制器在虚拟化环境中的唯一的优势。

下图显示相同 USN 回退虚拟化的域控制器在虚拟机监控程序支持 VM-GenerationID 上运行 Windows Server 2012 上检测到时如何应用虚拟化安全措施。

![广告 DS 简介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

在此情况下，当虚拟机监控程序检测到更改为 VM-GenerationID 值，触发，包括虚拟化直流（从 A 到在上例中 B) InvocationID 重置保护虚拟化和更新 VM-GenerationID 值保存在 VM 匹配虚拟机监控程序存储的新值 (G2) 上。 安全措施确保这两个域控制器汇聚复制。

与 Windows Server 2012、广告 DS 使用 VM-GenerationID 感知管理程序上托管虚拟域控制器上的安全措施，而确保意外的应用程序的快照或其他此类虚拟机监控程序启用机制可能回退广告 DS 环境（通过阻止复制的问题，如 USN 气泡或逗留对象）不会虚拟机的状态中断。 但是，通过应用虚拟机快照还原域控制器不建议作为备份域控制器替代机制。 我们建议你继续使用 Windows Server 备份或其他 VSS-writer 基于备份解决方案。

> [!CAUTION]
> 如果意外还原为快照生产环境中的域控制器，建议，请供应商参考的应用程序，并且该虚拟机，以获取有关后快照验证这些程序的状态指导上托管服务还原。

有关详细信息，请参阅[虚拟化域控制器安全还原体系结构](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)。

## <a name="virtualized_dc_cloning"></a>复制虚拟化的域控制器
开始与 Windows Server 2012、管理员可以轻松安全地部署副本域控制器复制现有虚拟域控制器。 在虚拟环境中，不会再管理员必须反复部署准备好使用 sysprep.exe 服务器映像，请升级到某个域控制器服务器，，然后完成部署每个副本域控制器其他配置要求。

> [!NOTE]
> 管理员必须遵守现有进程部署在域中，如使用 sysprep.exe 准备服务器虚拟硬盘 (VHD) 升级到某个域控制器服务器，然后完成任何其他配置要求的第一个域控制器。 在灾难恢复方案中，使用新的服务器备份还原域中的第一个域控制器。

### <a name="scenarios-that-benefit-from-virtual-domain-controller-cloning"></a>受益于虚拟域控制器复制的方案

-   快速部署新的域中的其他域控制器

-   快速通过还原通过使用复制的域控制器快速部署广告 DS 容量还原灾难恢复过程业务连续性

-   通过利用域控制器，以便容纳更高的比例要求弹性预配优化专用云部署

-   快速预配启用部署和的新功能和功能生产 rollout 之前测试测试环境

-   快速满足增加的容量需求通过复制 branch 办公室中的现有域控制器 branch 办公室中

快速部署时大量的域控制器，继续遵循验证运行状况的每个域控制器安装完成后你现有的过程。 部署适中批量域控制器，以便每一批安装完成后，你可以验证它们的运行状况。 推荐的批大小为 10。 有关详细信息，请参阅[部署克隆虚拟化的域控制器步骤](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc)。

### <a name="clear-separation-of-responsibilities"></a>职责清除分离
要复制的虚拟化的域控制器授权位于广告 DS 管理员的控制。 为了使虚拟机监控程序管理员可以通过将复制虚拟域控制器部署其他域控制器，广告 DS 管理员必须选择和授权域控制器，然后运行准备的步骤，以将其启用作为复制的源。

对虚拟机预配通常在虚拟机监控程序管理员范围，虚拟机监控程序管理员可以通过复制授权，并且在准备好复制由广告 DS 管理员的虚拟化的该域控制器配置副本域控制器虚拟机。

> [!WARNING]
> 允许管理虚拟机监控程序承载虚拟域控制器的任何人都必须强烈受信任并审核环境中。

### <a name="how-does-virtual-domain-controller-cloning-work"></a>虚拟域控制器复制的工作方式是怎样的？
过程：复制涉及到使副本的现有的虚拟域控制器 VHD（或者有关更复杂配置，域控制器 VM），在授权它用于广告 DS 中复制和创建克隆配置文件。 这减少步数，以及时间涉及通过否则消除部署副本虚拟域控制器重复部署任务。

克隆域控制器使用检测到它是一份另一个域控制器以下条件：

1.  提供的虚拟机的 VM-Generation ID 为不同于存储在 DIT VM-Generation ID 的值。

    > [!NOTE]
    > 虚拟机监控程序平台必须支持 VM-Generation ID（Windows Server 2012 Hyper-V 支持 VM-Generation ID）。

2.  存在文件称为 DCCloneConfig.xml，在以下位置：

    -   DIT 所在的目录

    -   %windir%\NTDS

    -   根的可移动媒体驱动器

一旦满足条件，它将经过复制到规定作为副本域控制器的过程。

克隆域控制器使用源域控制器（的域控制器其副本，则表示）的安全上下文联系 Windows Server 2012 主域控制器 (PDC) 仿真器运营主机角色持有人（也称为灵活一个主机操作或 FSMO）。 PDC 模拟器必须运行 Windows Server 2012，但它不需要在虚拟机监控程序上运行。

> [!NOTE]
> 如果你有具有参考源域控制器的特性的方案扩展，并且特性位于复制对象计算机对象（NTDS 设置对象）创建克隆之一，将不复制或更新，以参考克隆域控制器该属性。

验证后，将请求的域控制器授权进行复制，PDC 仿真器将创建新计算机标识包括新的帐户、SID、名称和密码标识为副本域控制器这台计算机和回克隆发送此信息。 克隆域控制器将然后准备广告 DS 数据库文件用作副本，并且还清理计算机的状态。

有关详细信息，请参阅[虚拟化域控制器复制体系结构](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch)。

### <a name="cloning-components"></a>复制组件
克隆组件的 Windows PowerShell 和关联的 XML 文件 Active Directory 模块中包括新 cmdlet:

-   **新 ADDCCloneConfigFile** "此 cmdlet 创建并 DCCloneConfig.xml 在适当的位置，以确保它已可用于复制触发的位置。 它还执行先决条件检查以确保成功复制。 它包含在 Active Directory 模块的 Windows PowerShell。 你可以在运行该本地虚拟化的域控制器，正在准备用于复制，也可以运行它使用远程-离线状态下的选项。 你可以指定克隆域控制器，如其名称、站点和 IP 地址设置。

    它执行先决条件检查是：

    > [!NOTE]
    > 先决条件检查不时执行"使用离线状态下的选项。 有关详细信息，请参阅[在离线模式下运行 New-ADDCCloneConfigFile](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode)。

    -   复制授权 DC 正在准备 (属于**克隆域控制器**组)

    -   PDC 模拟器运行 Windows Server 2012。

    -   从运行列出的任何程序或服务**获取 ADDCCloningExcludedApplicationList**纳入 CustomDCCloneAllowList.xml（复制组件此列表的末尾更详细地解释）。

-   **DCCloneConfig.xml** "要成功复制虚拟化的域控制器，该文件必须 DIT 所在的目录中存在*%windir%\NTDS*，或可移动媒体驱动器根。 除了用于正团结一心触发器检测和启动复制，它还会提供一种指定克隆域控制器配置设置的方法。

    方案和一个示例文件 DCCloneConfig.xml 文件在所有 Windows Server 2012 计算机上存储：

    -   %windir%\system32\DCCloneConfigSchema.xsd

    -   %windir%\system32\SampleDCCloneConfig.xml

    建议你使用 New-ADDCCloneConfigFile cmdlet 创建 DCCloneConfig.xml 文件。 虽然也可能使用与识别 XML 编辑器架构文件创建该文件，手动编辑的文件的可能性的错误。 如果编辑的文件，它必须使用进行识别 XML 编辑器，Visual Studio 中，如[XML 记事本](https://www.microsoft.com/download/details.aspx?displaylang=en&id=7973)，或第三方应用（不使用记事本）。

-   **获取 ADDCCloningExcludedApplicationList** "此 cmdlet 运行克隆的过程，以确定哪些服务或安装的程序未默认支持列表上，DefaultDCCloneAllowList.xml，在开始之前源域控制器上或用户定义的包含列表命名的 CustomDCCloneAllowList.xml 文件，并因此不过复制影响的评估。

    此 cmdlet 搜索服务源域控制器在服务控制管理器中，并下列出的已安装的程序**HKLM\Software\ Microsoft \Windows\CurrentVersion\Uninstall**的默认列表 (DefaultDCCloneAllowList.xml) 中并非指定或者，如果提供，用户定义包含列表 (CustomDCCloneAllowList.xml file)。 通过运行 cmdlet 返回的应用程序和服务列表是什么已 DefaultDCCloneAllowList.xml 或 CustomDCCloneAllowList.xml 文件和构建运行时，根据源直流上安装的列表中已提供之间的区别。 如果你确定，可以安全地复制程序和服务可以到 CustomDCCloneAllowList.xml 文件添加了从 Get-ADDCCloningExcludedApplicationList 程序和服务的输出。 若要确定是否可以安全地复制服务或安装的程序，评估满足以下条件：

    -   是服务或安装的程序影响的计算机的身份，如姓名、SID、密码和等等？

    -   该服务或程序官方商城任何状态本地计算机上安装，从而可能影响其功能克隆上？

    你必须处理软件供应商的应用程序以确定是否可以安全地复制该服务或该计划。

    > [!NOTE]
    > 之前预配其他服务或 CustomDCCloneAllowList.xml 文件中的程序，验证你是否有必要复制该虚拟机上包含该软件的许可证。

    如果不克隆提供的应用程序，请从源域控制器将其删除之前创建了克隆媒体。 如果应用程序显示在 cmdlet 输出中，但不是包括在 CustomDCCloneAllowList.xml 文件，复制将失败。 为复制成功，cmdlet 输出应未列出任何服务或应用程序。 换言之，应用应该会 CustomDCCloneAllowList.xml 文件中包含或从源域控制器中删除。

    下表介绍了有关运行 Get-ADDCCloningExcludedApplicationList 的选项。

    |||
    |-|-|
    |参数|说明|
    |*<no argument specified>*|显示服务或应用程序的列表，不为复制计算在内在主机上。 如果已在所有允许位置 CustomDCCloneAllowList.XML，它将使用该文件（这可能是执行任何操作，如果匹配列表）的显示剩余的服务和程序。|
    |-GenerateXml|创建填入服务 CustomDCCloneAllowList.XML 文件和程序在主机上列出。|
    |-强制|将覆盖现有 CustomDCCloneAllowList.XML 文件。|
    |-路径|若要创建 CustomDCCloneAllowList.XML 的文件夹路径。|

-   **DefaultDCCloneAllowList.xml** "存在默认情况下，在此文件，每个 Windows Server 2012 域控制器在*%windir%\system32*。 它列出的服务，并安装可以安全地复制默认程序。 您不得更改该文件的内容的位置或复制将失败。

-   **CustomDCCloneAllowList.xml** "如果你有服务或安装位于你的源域控制器的之外这些列入 DefaultDCCloneAllowList.xml 文件的程序，这些程序和服务必须包含在此文件。 若要查找服务或中未列出的已安装的程序在 DefaultDCCloneAllowList.xml 文件中，运行**获取 ADDCCloningExcludedApplicationList** cmdlet。 你应使用**"GenerateXml**参数生成的 XML 文件。

    克隆过程检查以下顺序此文件的位置，并使用找到，无论为该文件夹的内容的第一个 XML 文件：

    1.  以下注册表项：

        ```
        HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters
        AllowListFolder (REG_SZ)
        ```

    2.  工作目录 DSA

    3.  %systemroot%\NTDS

    4.  可移动读取/写入媒体、的驱动器号，根部驱动器的订单

### <a name="deployment-scenarios"></a>部署方案
支持以下部署方案虚拟域控制器复制：

-   部署克隆域控制器进行源域控制器 (vhd) 的虚拟硬盘文件的副本。

-   通过将复制的源代码域控制器使用涉及的虚拟机监控程序导出月导入语义虚拟机部署克隆域控制器。

> [!NOTE]
> 部分中的步骤[部署克隆虚拟化的域控制器步骤](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc)演示复制使用导出月导入功能的 Windows Server 2012 Hyper-V 虚拟机。

## <a name="steps_deploy_vdc"></a>部署克隆虚拟化的域控制器的步骤

-   [先决条件](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#prerequisites)

-   [第 1 步：授予源虚拟化的域控制器要复制的权限](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk4_grant_source)

-   [第 2 步：用 Get-ADDCCloningExcludedApplicationList cmdlet](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet)

-   [第 3 步：运行 New-ADDCCloneConfigFile](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk5_create_insert_dccloneconfig)

-   [第 4 步：导出，然后导入虚拟机的源域控制器](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk7_export_import_vm_sourcedc)

### <a name="prerequisites"></a>先决条件

-   若要完成以下过程中的步骤，必须域管理员组成员的或等效权限分配给它。

-   使用本指南中的 Windows PowerShell 命令必须从提升了权限的命令提示符中运行。 若要执行此操作，右键单击**Windows PowerShell**图标，然后依次单击**以管理员身份运行**。

-   安装 Hyper-V server 角色与 Windows Server 2012 服务器 (**HyperV1**)。

-   第二个安装 Hyper-V server 角色与 Windows Server 2012 服务器 (**HyperV2**)。

    > [!NOTE]
    > -   如果你使用的另一个虚拟机监控程序，您应联系该虚拟机监控程序供应商，若要验证是否虚拟机监控程序支持 VM-Generation id。 如果您已提供 DCCloneConfig.xml 虚拟机监控程序不支持 VM-Generation ID，新 VM 将启动到目录服务还原模式 (DSRM)。
    > -   为了提高可用性的广告 DS 服务，本指南建议，并且提供使用两种不同的 Hyper-V 主机，这有助于防止失败可能单点的说明进行操作。 但是，你无需执行虚拟域控制器复制的两个 Hyper-V 主机。
    > -   你需要使用的每个 Hyper-V 服务器上的本地管理员组成员 (**HyperV1**和**HyperV2**)。
    > -   为了成功导入和导出 VHD 文件使用 Hyper-V，这两个 Hyper-V 主机上的虚拟网络交换机的成员都应拥有相同的名称。 例如，如果你拥有一个虚拟网络切换**HyperV1**名为 VNet 然后需要切换虚拟网络**HyperV2**名为 VNet。
    > -   如果两个 Hyper-V 主机 (**HyperV1**和**HyperV2**) 具有不同的处理器、关机虚拟机 (**VirtualDC1**) 导出、右键单击 VM、单击你打算**设置**，单击**处理器**下,**处理器的兼容性**选择**迁移到具有不同的处理器版本物理计算机**单击**确定**。

-   A 部署 Windows Server 2012 承载域控制器（虚拟化或物理）PDC 模拟器角色 (**DC1**)。 若要验证是否 Windows Server 2012 域控制器上托管 PDC 模拟器角色，运行以下 Windows PowerShell 命令：

    ```
    Get-ADComputer (Get-ADDomainController "Discover "Service "PrimaryDC").name "Property operatingsystemversion | fl
    ```

    OperatingSystemVersion 值应返回作为 6.2 版本。 如有必要，您可以向运行 Windows Server 2012 的域控制器传输 PDC 模拟器角色。 有关详细信息，请参阅[使用 Ntdsutil.exe 转移或占用到某个域控制器域控制器](https://support.microsoft.com/kb/255504)。

-   A 部署 Windows Server 2012 来宾虚拟化的域控制器 (**VirtualDC1**) 处于与 Windows Server 2012 域控制器举办 PDC 模拟器角色相同的域 (**DC1**)。 这将用于复制源域控制器。 将 Windows Server 2012 Hyper-V 服务器上托管来宾虚拟域控制器 (**HyperV1**)。

    > [!NOTE]
    > -   对于复制成功，用于创建克隆源域控制器无法从已创建源 VHD 媒体以来降级 DC。
    > -   关闭之前复制 VM 或其 VHD 源域控制器。
    > -   你不应复制 VHD 或还原早于的 tombstone 生存期值（或如果启用了 Active Directory 回收站的已删除的对象寿命值）的快照。 如果你要复制的现有的域控制器 VHD，请确保 VHD 文件不较旧的 tombstone 生存期重视（默认情况下，60 天）。 不应将复制运行的域控制器 VHD 创建克隆媒体。

    弹出任何虚拟软盘驱动器 (VFD) DC 可能有源。 尝试将导入的新虚拟机时，这可能导致共享的问题。

    Windows Server 2012 域控制器上 VM-GenerationID 虚拟机监控程序托管可用于作为源复制。 源 Windows Server 2012 域控制器用于复制应该处于良好状态。 若要确定运行的源域控制器状态[dcdiag](https://technet.microsoft.com/library/cc731968(WS.10).aspx)。 若要获得更好地了解 dcdiag 返回的输出，请参阅[用途 DCDIAG 实际上…执行操作？](http://blogs.technet.com/b/askds/archive/2011/03/22/what-does-dcdiag-actually-do.aspx).

    如果源域控制器 DNS 服务器，复制的域控制器还将 DNS 服务器。 你应该选择 DNS 服务器该主机仅集成 Active Directory 的区域。

    DNS 客户端设置不复制，但改用指定 DCCloneConfig.xml 文件中。 如果它们未指定，复制的域控制器将指向本身首选 DNS 服务器作为默认情况。 复制的域控制器没有 DNS 委派。 家长 DNS 区域管理员应更新复制的域控制器根据需要 DNS 委派。

    > [!WARNING]
    > 不会对 Active Directory 轻型目录服务 (广告 LDS) 扩展的虚拟化安全措施。 因此你不应尝试复制加此广告 LDS 实例 CustomDCCloneAllowList.xml 承载广告 LDS 实例广告 DS 域控制器。 广告 LDS 不知道 VM-Generation ID，因为复制与广告 LDS 域控制器上可能会导致 USN 回退引起分歧该广告 LDS 配置设置。

    复制不支持以下服务器的角色：

    -   动态主机配置协议(DHCP)

    -   Active Directory 证书服务（广告客户服务）

    -   Active Directory 轻型目录服务 (广告 LDS)

### <a name="bkmk4_grant_source"></a>第 1 步：授予源虚拟化的域控制器要复制的权限
在此过程中，您授予源域控制器要复制的使用权限**Active Directory 管理中心**添加到源代码域控制器**克隆域控制器**组。

##### <a name="to-grant-the-source-virtualized-domain-controller-the-permission-to-be-cloned"></a>要授予权限，要复制的源虚拟化的域控制器

1.  复制正准备在该域控制器相同的域中的任何域控制器上 (**VirtualDC1**)，请打开**Active Directory 管理中心**(ADAC)，找到虚拟化的域控制器对象 (通常位于域控制器**域控制器**中 ADAC 容器)、右键单击它、选择**添加到组**下**输入要选择的对象名称**类型**克隆域控制器**，然后单击**确定**。

    执行此步骤中的组成员更新必须先执行复制到 PDC 模拟器复制。 如果**克隆域控制器**找不到组，可能不在运行 Windows Server 2012 的域控制器中托管 PDC 模拟器角色。

    > [!NOTE]
    > 若要打开 Windows Server 2012 域控制器上 ADAC，打开 Windows PowerShell 和类型**dsac.exe**。

![广告 DS 简介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***

以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤：


    Add-ADGroupMember "Identity "CN=Cloneable Domain Controllers,CN=Users, DC=Fabrikam,DC=Com" "Member "CN=VirtualDC1,OU=Domain Controllers,DC=Fabrikam,DC=com"


### <a name="bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet"></a>第 2 步：用 Get-ADDCCloningExcludedApplicationList cmdlet
在此过程中，运行`Get-ADDCCloningExcludedApplicationList`cmdlet 上源虚拟化的域控制器在发现任何程序或未计算复制的服务。 你需要运行 Get-ADDCCloningExcludedApplicationList cmdlet New-ADDCCloneConfigFile cmdlet 之前，因为如果 New-ADDCCloneConfigFile cmdlet 检测到排除的应用程序，它将创建一个 DCCloneConfig.xml 文件。

##### <a name="to-identify-applications-or-services-that-run-on-a-source-domain-controller-which-have-not-been-evaluated-for-cloning"></a>识别的应用程序或源域控制器运行的尚未评估为复制的服务

1.  源该域控制器上 (**VirtualDC1**)，请单击**服务器管理器**，单击**工具**，单击**Active Directory 的 Windows PowerShell 模块**，然后键入以下命令：


    Get-ADDCCloningExcludedApplicationList


2.  检查软件供应商，以确定是否他们可以安全地复制返回的服务和已安装的程序的列表。 如果无法安全复制应用程序或服务，在列表中，你必须将其从源域控制器删除或复制将失败。

3.  对于被确定为安全地要复制的已安装的程序和服务组中，运行再次使用命令**"GenerateXML**切换预配这些服务，并在程序**CustomDCCloneAllowList.xml**文件。


    Get-ADDCCloningExcludedApplicationList GenerateXml


### <a name="bkmk5_create_insert_dccloneconfig"></a>第 3 步：运行 New-ADDCCloneConfigFile
运行 New-ADDCCloneConfigFile 源域控制器，并选择指定配置克隆域控制器，如姓名、IP 地址和 DNS 解析程序的设置。

例如，若要创建了静态 IPv4 地址命名 VirtualDC2 克隆域控制器，键入：


    New-ADDCCloneConfigFile "Static -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.255.0" -CloneComputerName "VirtualDC2" -IPv4DefaultGateway "10.0.0.3" -SiteName "REDMOND"

> [!NOTE]
> 克隆域控制器将位于同一来源的域控制器站点，除非 DCCloneConfig.xml 文件中指定不同的站点。 建议你指定根据其 IP 地址的克隆域控制器 DCCloneConfig.xml 文件中的适合的网站。

是可选的计算机名称。 如果你未指定之一，将根据以下算法生成一个唯一的名称：

-   前缀是源域控制器计算机名称的首次 8 个字符。 例如，SourceComputer 源计算机的名称将为前缀 SourceCo string 被截断。

-   格式唯一命名职务""CL*nnnn*"附加到前缀字符串了*nnnn* 0001-9999 PDC 确定当前未使用的下一个可用值。 例如，如果 0047 允许的范围内的下一个可用的编号，使用之前的示例中的计算机名称前缀 SourceCo，用于克隆计算机派生的名称将设置为 SourceCo CL0047。

> [!NOTE]
> 全球目录服务器 (GC)，才能进行 New-ADDCCloneConfigFile cmdlet 成功工作。 源域控制器中的成员**克隆域控制器**必须 gc 反映组。 无需将 PDC 模拟器，同时也域控制器 GC，但最好应在相同的站点。 如果 GC 不可用，命令将失败并显示错误"服务器不可操作。" 有关详细信息，请参阅[虚拟化域控制器疑难解答](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md)。

若要创建克隆域控制器命名 Clone1 静态 IPv4 设置，并指定首选和备用 WINS 服务器，键入：


    New-ADDCCloneConfigFile "CloneComputerName "Clone1" "Static -IPv4Address "10.0.0.5" "IPv4DNSResolver "10.0.0.1" "IPv4SubnetMask "255.255.0.0" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


> [!NOTE]
> 指定 WINS 服务器时，你必须指定都**"PreferredWINSServer**和**"AlternateWINSServer**。 如果你指定的这些参数仅，复制失败，错误代码 0x80041005 出现在 dcpromo.log。

若要创建动态 IPv4 设置命名 Clone2 克隆域控制器，键入：


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" 


> [!NOTE]
> 在此情况下，应该会有 DHCP 服务器环境克隆访问权限，也可以获取 IP 地址和其他相关的网络设置。

若要创建克隆域控制器命名 Clone2 动态 IPv4 设置，并指定首选和备用 WINS 服务器，键入：


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" -SiteName "REDMOND" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


若要创建克隆域控制器动态 IPv6 设置中，键入：


    New-ADDCCloneConfigFile -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


若要创建克隆域控制器静态 IPv6 设置中，键入：


    New-ADDCCloneConfigFile "Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


> [!NOTE]
> 指定 IPv6 设置时, 唯一静态和动态设置之间的区别是包含**-静态**切换。 包含**-静态**切换使得强制若要在至少有一个指定**IPv6DNSResolver**。预计静态 IPv6 地址通过无地址自动配置 (SLAAC) 分配路由器前缀配置。 动态 IPv6，DNS 解析程序是可选的但应克隆可以访问在来获取 IPv6 地址和配置信息的 DNS 子网 IPv6 启用 DHCP 服务器。

#### <a name="BKMK_OfflineMode"></a>在离线模式下运行 New-ADDCCloneConfigFile
如果你有源域控制器媒体已准备复制（即源域控制器授权复制的、已 Get-ADDCCloningExcludedApplicationList cmdlet 运行，等等）的多个副本，并且你想要指定其他设置的媒体每个副本，你可以在离线模式下运行 New-ADDCCloneConfigFile。 这可以更高效比单独使每个 VM，例如，通过导入的每个副本做好准备。

在此情况下，域管理员可以装载离线状态下的磁盘和使用远程服务器管理工具 (RSAT) 运行 New-ADDCCloneConfigFile cmdlet-脱机参数以添加允许使用新的 Windows PowerShell 选项包含在 Windows Server 2012 出厂类似自动化 XML 文件。 有关如何才能在离线模式下运行 New-ADDCCloneConfigFile cmdlet 装载离线状态下的磁盘的详细信息，请参阅[添加到离线状态下的系统磁盘的 XML](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_Offline)。

你首先应源媒体以确保该先决条件检查 pass 上本地运行 cmdlet。 先决条件检查不会在离线模式下执行，因为 cmdlet 无法运行的计算机上可能不会从同一域或已加入域的计算机。 本地运行 cmdlet 后，它将创建一个 DCCloneConfig.xml 文件。 你可能会删除本地如果您计划使用离线模式随后创建 DCCloneConfig.xml。

若要创建克隆域控制器名为 CloneDC1 在离线模式下，在雷德蒙名为某个站点"静态 IPv4 地址，类型：


    New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS


若要创建克隆域控制器名称 Clone2 离线模式静态 IPv4 和静态 IPv6 设置中，键入：


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS


若要指定 DNS 解析程序设置多个 DNS 服务器静态 IPv4 和动态 IPv6 设置离线模式中创建克隆域控制器，键入：


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS 


若要创建克隆域控制器名称 Clone1 离线模式动态 IPv4 和静态 IPv6 设置中，键入：


    New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS


若要创建克隆域控制器在离线模式下，与动态 IPv4 和动态 IPv6 设置中，键入：


    New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS


### <a name="bkmk7_export_import_vm_sourcedc"></a>第 4 步：导出，然后导入虚拟机的源域控制器
在此过程中，导出虚拟机的源虚拟化的域控制器，然后再将导入虚拟机。 此操作会在域中创建克隆虚拟化的域控制器。

你需要的每个 Hyper-V 主机上的本地管理员组成员。 如果你为每个服务器使用不同的凭据，运行的 Windows PowerShell cmdlet 导出和导入在不同的 Windows PowerShell 会话中的 VM。

如果有源该域控制器上的快照，应之前源域控制器导出因为 VM 如果快照具有与目标 hyper v 主机不兼容的处理器设置不会导入删除它们。 如果处理器设置之间的源代码和目标 hyper v 主机兼容，你可能导出并复制源代码，而无需事先了解删除快照。 导入后，但是，快照之前，必须删除从克隆 VM 启动电脑。

##### <a name="to-copy-a-virtual-domain-controller-by-exporting-and-then-importing-the-virtualized-source-domain-controller"></a>若要通过导出，然后导入虚拟化的源域控制器复制虚拟域控制器

1.  在**HyperV1**，关闭源域控制器 (**VirtualDC1**)。

    ![广告 DS 简介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***

    Stop-VM-命名 VirtualDC1-ComputerName HyperV1


2.  在**HyperV1**删除的快照，然后将其源域控制器 (VirtualDC1) 导出到 c:\CloneDCs 目录。

> [!NOTE]
> 因为拍摄快照，每次 AVHD 创建新文件，它就像差异磁盘，你应该删除所有关联的快照。 这将创建链影响。 如果你有拍摄快照并且插入 VHD DCCLoneConfig.xml 文件时，你最终可能会从较早的 DIT 版本创建克隆或插入错误 VHD 文件的配置文件。 删除快照将所有这些 AVHDs 合并到 VHD 库中。

![广告 DS 简介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***


    Get-VMSnapshot VirtualDC1 | Remove-VMSnapshot -IncludeAllChildSnapshots
    Export-VM -Name VirtualDC1 -ComputerName HyperV1 -Path c:\CloneDCs\VirtualDC1


3.  复制该文件夹**virtualdc1**到的目录 c:\Import **HyperV2**。

4.  在**HyperV2**，并使用**Hyper-V 管理器**，导入虚拟机 (使用**导入虚拟机**向导中**Hyper-V 管理器**) 从该文件夹**c:\Import\virtualdc1**和所有关联删除**快照**。

使用**复制虚拟机（创建新的唯一 ID）**导入虚拟机时选项。

![广告 DS 简介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***

    $path = Get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual Machines"
    $vm = Import-VM -Path $path.fullname -Copy -GenerateNewId
    Rename-VM $vm VirtualDC2


若要从同一来源域控制器创建多个克隆域控制器：

  -   UI：在**导入虚拟机**向导中，指定的新位置**配置虚拟机的文件夹**，**快照官方商城**，**智能分页文件夹**，并不同**位置**虚拟机虚拟硬盘。

  -   Windows PowerShell：通过使用以下参数指定虚拟机的新位置`Import-VM`cmdlet:

        $path = Get-ChildItem"C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual 计算机"Import-VM-路径 $path.fullname-副本-GenerateNewId-ComputerName HyperV2-VhdDestinationPath"路径"-SnapshotFilePath"路径"-SmartPagingFilePath"路径"-VirtualMachinePath"路径"


> [!NOTE]
> 创建多个克隆域控制器同时的推荐的批大小为 10。 最大数目受限制的最大站复制连接数，默认情况下是 16 为分布式文件系统复制 (DFSR) 和 10 文件复制服务 (FRS)。 你应该不建议克隆域控制器的数量多于同时部署除非你已为您的环境全面测试该号码。

5.  在**HyperV1**，重新启动源域控制器 (**(VirtualDC1**) 以将其重新联机。

![广告 DS 简介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***

    Start-VM -Name VirtualDC1 -ComputerName HyperV1


6.  在**HyperV2**，启动虚拟机 (**VirtualDC2**) 以将其联机作为克隆域控制器在域中。

![广告 DS 简介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***


    Start-VM -Name VirtualDC2 -ComputerName HyperV2

> [!NOTE]
> 必须运行 PDC 模拟器复制成功。 如果它已关闭，请确保它已启动和执行初始同步，因此它已注意到，它是保存 PDC 模拟器角色。 有关详细信息，请参阅 Microsoft[知识库文章 305476](https://support.microsoft.com/kb/305476)。

复制完成后，可验证成功以确保克隆操作克隆计算机的名称。 验证 VM 未启动中目录服务还原模式 (DSRM)。 如果你在尝试登录时收到错误，指出没有登录服务器不可用，请尝试日志记录 DSRM 中。 如果未成功复制直流，所做的那样，DSRM 在引导时检查事件查看器中的日志和 dcpromo 日志 %systemroot%/debug 文件夹中。

复制的域控制器中将成为的成员**克隆域控制器**分组，因为它将从源域控制器复制的会员。 作为最佳做法，你应何时出发**克隆域控制器**组空只有在你准备好进行克隆操作，并且克隆操作完成后，你应该将删除成员。

如果源域控制器存储的备份媒体，复制的域控制器同时将存储备份的媒体。 你可以运行`wbadmin get versions`复制的域控制器上显示备份的媒体。 组成员的域管理员应该删除以防止意外还原复制的域控制器上备份的媒体。 有关如何删除系统状态备份使用 wbadmin.exe 的详细信息，请参阅[Wbadmin 删除 systemstatebackup](https://technet.microsoft.com/library/cc742081(v=WS.10).aspx)。

## <a name="troubleshooting"></a>故障排除
如果克隆域控制器 (**VirtualDC2**) 启动中目录服务还原模式 (DSRM)，它不返回到正常模式在接下来自行重启。 若要登录到 DSRM 在启动时，域控制器上，使用**。\Administrator**和指定 DSRM 密码。

更正复制失败的原因，然后确认 dcpromo.log 并不表示复制无法重试。 如果无法重新尝试复制、安全地放弃媒体。 复制可重新尝试，如果必须要删除 DS 恢复模式下启动标志以尝试再次复制。

1.  打开提升了权限的命令（右键单击 Windows Server 2012 和选择以管理员身份运行），然后按的 Windows Server 2012 **msconfig**。

2.  在**启动**选项卡下**启动选项**，清除**安全启动**(已所选的选项**Active Directory 修复启用**)。

3.  单击**确定**并在出现提示时重新启动。

有关更多疑难解答信息虚拟化的域控制器，请参阅[虚拟化域控制器疑难解答](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md)。



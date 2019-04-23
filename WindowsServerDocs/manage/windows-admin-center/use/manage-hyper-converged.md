---
title: 管理超聚合基础结构与 Windows Admin Center
description: 管理超聚合基础结构与 Windows Admin Center (项目 Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 02/11/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4d849120d2daaa40cb797cc5e7d4c23c74da5bb7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874258"
---
# <a name="manage-hyper-converged-infrastructure-with-windows-admin-center"></a>管理超聚合基础结构与 Windows Admin Center

>适用于：Windows Admin Center，Windows Admin Center 预览版

## <a name="what-is-hyper-converged-infrastructure"></a>什么是 Hyper-Converged 基础结构

软件定义的计算、 存储和网络连接到一个群集以提供高性能、 经济高效，并可轻松缩放的虚拟化将合并超聚合基础结构。 此功能在 Windows Server 2016 中引入[存储空间直通](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)并[HYPER-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)。

> [!Tip]
> 为希望获得 Hyper-Converged 基础结构？ Microsoft 建议这些[Windows Server 软件定义](https://microsoft.com/wssd)从我们的合作伙伴解决方案。 它们是设计、 组装和根据我们的参考体系结构，以确保兼容性和可靠性，因此你和快速运行验证。

> [!IMPORTANT]
> 一些本文中所述的功能是仅在 Windows Admin Center 预览版中可用。 [如何获取此版本？](http://aka.ms/windowsadmincenter)

## <a name="what-is-windows-admin-center"></a>什么是 Windows Admin Center

[Windows Admin Center](../understand/windows-admin-center.md)下一代管理工具是一个适用于 Windows Server，传统的"现成"工具类似于服务器管理器中的后续版本。 它是免费和可以安装和使用未连接到 Internet。 可以使用 Windows Admin Center 来管理和监视运行 Windows Server 2016 或 Windows Server 2019 Insider Preview 内部版本 Hyper-Converged 基础结构。

![超聚合群集仪表板](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## <a name="key-features"></a>关键功能

Windows Admin Center Hyper-Converged 基础结构的亮点包括：

- **统一的单一窗格-的-不受限的计算、 存储和推出的网络。** 查看你的虚拟机、 主机服务器、 卷、 驱动器和一个专门构建的、 一致的互连体验的详细信息。
- **创建和管理存储空间和 HYPER-V 虚拟机。** 从根本上简单工作流来创建、 打开、 调整大小，并删除卷;和创建、 启动、 连接和移动; 的虚拟机以及更多。
- **功能强大的整个群集监视。** 在仪表板关系图内存和 CPU 使用率、 存储容量、 IOPS、 吞吐量和延迟时间以实时的跨群集中，清除警报时有问题并在每个服务器。
- **软件定义网络 (SDN) 支持。（Windows Admin Center 预览版中的新增功能）** 管理和监视虚拟网络，子网，将虚拟机连接到虚拟网络和监视 SDN 基础结构。

Hyper-Converged 基础结构的 Windows Admin Center 是由 Microsoft 积极开发。 它将接收频繁更新，以改善现有功能添加新功能。

## <a name="before-you-start"></a>开始之前

若要管理你的群集作为 Hyper-Converged Windows Admin Center 中的基础结构，它需要运行 Windows Server 2016 或 Windows Server 2019 一个预览版本，并已 HYPER-V 和存储空间直通启用。

> [!Tip]
> Windows Admin Center 还提供了通用管理体验的 Windows Server 2012 和更高版本支持的任何工作负荷，可用任何群集。 如果这听起来更好地满足你的群集添加到 Windows Admin Center 时，选择[**故障转移群集**](manage-failover-clusters.md)而不是**Hyper-Converged 群集**。

### <a name="prepare-your-windows-server-2016-cluster-for-windows-admin-center"></a>为 Windows Admin Center 准备你的 Windows Server 2016 群集

Windows Admin Center Hyper-Converged 基础结构取决于管理 Windows Server 2016 发布后添加的 Api。 您可以管理 Windows Server 2016 群集与 Windows Admin Center 之前，你将需要执行这两个步骤：

1. 验证是否安装在群集中的每个服务器[2018年-05 适用于 Windows Server 2016 (KB4103723) 的累积更新](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723)或更高版本。 若要下载并安装此更新，请转到**设置** > **更新和安全** > **Windows 更新**，然后选择**联机检查 Microsoft Update 的更新**。
2. 在群集上以管理员身份运行以下 PowerShell cmdlet:

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> 只需一次，在群集中的任何服务器上运行 cmdlet。 可以在 Windows PowerShell 中本地运行它或使用凭据安全服务提供程序 (CredSSP) 来远程运行。 具体取决于您的配置，可能无法在 Windows Admin Center 内运行此 cmdlet。

> [!Important]
> 对于非英语区域设置中的部署，Windows Admin Center，以防止在仪表板加载 （仅限首次） 版本 1804年中没有的已知的问题。 解决方法是运行`Add-ClusterResource -Name 'SDDC Management' -Group 'Cluster Group' -ResourceType 'SDDC Management'`替换为*群集组*具有本地化名称，例如，*组 du cluster*法语。 将在下一次更新中解决此问题。
>
> **更新：** 这是现在 Windows Admin Center 预览版本 1806年中修复。

### <a name="prepare-your-windows-server-2019-cluster-for-windows-admin-center"></a>为 Windows Admin Center 准备你的 Windows Server 2019 群集

如果群集运行 Windows Server 2019 的 Insider Preview 版本，则不需要上述步骤。 只需将群集添加到 Windows Admin Center 下一步部分中所述，准备就绪 ！ [下载最新的预览版本的 Windows Server 2019](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver)。

### <a name="configure-software-defined-networking-optional"></a>配置软件定义网络 （可选） ###

可以配置 Hyper-Converged 基础结构运行 Windows Server 2016 或 2019年软件定义网络 (SDN) 中使用以下步骤：

1. 准备好操作系统上的超聚合基础结构主机安装的同一 OS 的 VHD。 此 VHD 将用于所有 NC/SLB/GW Vm。
2. 下载所有文件夹和文件下从 SDN Express [ https://github.com/Microsoft/SDN/tree/master/SDNExpress ](https://github.com/Microsoft/SDN/tree/master/SDNExpress)。
3. 准备使用部署控制台的不同 VM。 此 VM 应能够访问 SDN 主机。 此外，VM 应具有已安装 RSAT 的 HYPER-V 工具。
4. 将复制你下载到部署控制台 VM 的 SDN Express 的所有内容。 分享**SDNExpress**文件夹。 请确保每个主机可以访问**SDNExpress**共享文件夹，如配置文件行 8 中所定义：
```
    \\$env:Computername\SDNExpress
```
5. 将复制到 OS 的 VHD**映像**下的文件夹**SDNExpress**部署控制台 VM 上的文件夹。
6. 修改你的环境设置的 SDN Express 配置。 修改基于您环境的信息的 SDN Express 配置后，请完成以下两个步骤。
7. 使用管理员权限来部署 SDN 运行 PowerShell:

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose 
```

部署将需要大约 30 到 45 分钟。

## <a name="get-started"></a>立即开始行动

Hyper-Converged 基础结构部署后，你可以使用管理 Windows Admin Center。

### <a name="install-windows-admin-center"></a>安装 Windows Admin Center

如果你尚未准备好，下载并安装 Windows Admin Center。 最快方式才能启动且正在运行是在 Windows 10 计算机上安装它，然后远程管理服务器。 此时间不超过五分钟。 [立即下载](https://aka.ms/windowsadmincenter)或[了解有关其他安装选项的详细信息](../deploy/install.md)。

### <a name="add-hyper-converged-cluster"></a>添加超聚合群集

若要将群集添加到 Windows Admin Center:

1. 单击 **+ 添加**下所有连接。
2. 选择添加**Hyper-Converged 群集连接**。
3. 键入群集的名称，如果系统提示，要使用的凭据。
4. 单击**添加**来完成。

群集将添加到连接列表。 单击它以启动仪表板。

![添加超聚合群集连接](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### <a name="add-sdn-enabled-hyper-converged-cluster-windows-admin-center-preview"></a>添加启用了 SDN 的超聚合群集 （Windows Admin Center 预览版）

最新 Windows Admin Center 预览版 Hyper-Converged 基础结构支持软件定义的网络管理。 通过将网络控制器 REST URI 添加到你的超聚合群集的连接，您可以使用超聚合群集管理器管理 SDN 资源，并监视 SDN 基础结构。

1. 单击 **+ 添加**下所有连接。
2. 选择添加**Hyper-Converged 群集连接**。
3. 键入群集的名称，如果系统提示，要使用的凭据。
4. 检查**配置网络控制器**以继续。
5. 输入**网络控制器 URI**然后单击**验证**。
6. 单击**添加**来完成。

群集将添加到连接列表。 单击它以启动仪表板。

![添加启用了 SDN 的超聚合群集连接](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> 南向通信的 Kerberos 身份验证的 SDN 环境目前不支持。

## <a name="frequently-asked-questions"></a>常见问题解答

### <a name="are-there-differences-between-managing-windows-server-2016-and-windows-server-2019-insider-preview"></a>是否有管理 Windows Server 2016 和 Windows Server 2019 Insider Preview 之间的差异？

是。 Hyper-Converged 基础结构的 Windows Admin Center 接收频繁更新，提高 Windows Server 2016 和 Windows Server 2019 Insider Preview 的体验。 但是，某些新功能将仅用于 Insider Preview – 例如，用于删除重复和压缩的切换开关。

### <a name="can-i-use-windows-admin-center-to-manage-storage-spaces-direct-for-other-use-cases-not-hyper-converged-such-as-converged-scale-out-file-server-sofs-or-microsoft-sql-server"></a>可以使用 Windows Admin Center 来管理存储空间直通的其他使用情况 （未超聚合），例如聚合横向扩展文件服务器 (SoFS) 或 Microsoft SQL Server？

Windows Admin Center Hyper-Converged 基础结构不提供管理或专用于存储空间直通的其他用例的监视选项-例如，它不能创建文件共享。 但是，仪表板和核心功能，例如创建卷或驱动器，替换为适用于任何存储空间直通的群集。

### <a name="whats-the-difference-between-a-failover-cluster-and-a-hyper-converged-cluster"></a>故障转移群集和 Hyper-Converged 群集之间的区别是什么？

一般情况下，的术语"超聚合"是指在同一个运行 HYPER-V 和存储空间直通群集服务器虚拟化计算和存储资源。 Windows Admin Center，当您单击的上下文中 **+ 添加**从连接列表中，您可以选择添加之间**故障转移群集连接**或**Hyper-Converged 群集连接**:

- **故障转移群集连接**是故障转移群集管理器桌面应用程序的后续版本。 它用于任何支持的任何工作负荷，包括 Microsoft SQL Server 的群集提供了熟悉、 常规用途的管理体验。 它是适用于 Windows Server 2012 及更高版本。

- **Hyper-Converged 群集连接**为存储空间直通和 HYPER-V 量身定制的全新体验。 它采用仪表板并重点使用图表和警报进行监视。 它不用于 Windows Server 2016 和 preview 内部版本的 Windows Server 2019。

### <a name="why-do-i-need-the-latest-cumulative-update-for-windows-server-2016"></a>为什么需要 Windows Server 2016 的最新的累积更新？

Windows Admin Center Hyper-Converged 基础结构取决于管理 Api 开发自 Windows Server 2016 已发布。 这些 Api 中添加[2018年-05 适用于 Windows Server 2016 (KB4103723) 的累积更新](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723)、 截至 2018 年 5 月 8 日之前可用。

### <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>使用 Windows Admin Center 需要多少费用？

Windows Admin Center 不会在 Windows 以外产生额外费用。

您可以使用在无需额外付费的有效许可证的 Windows Server 或 Windows 10 的 Windows Admin Center （可作为单独的下载）-它 Windows 补充 EULA 的许可。

### <a name="does-windows-admin-center-require-system-center"></a>Windows Admin Center 是否需要 System Center？

否。

### <a name="does-it-require-an-internet-connection"></a>它是否需要 Internet 连接？

否。

尽管 Windows Admin Center 提供了功能强大和便利集成的 Microsoft Azure 云、 核心管理和监视体验 Hyper-Converged 基础结构完全是在本地。 可以安装和使用未连接到 Internet。

## <a name="things-to-try"></a>要尝试的操作

如果你刚入门，下面是一些快速教程，帮助您了解 Windows Admin Center Hyper-Converged 基础结构进行组织和工作原理。 请执行良好的判断并谨慎使用生产环境。 使用 Windows Admin Center 版本 1804年和 Windows Server 2019 Insider Preview 内部版本记录了这些视频。

### <a name="manage-storage-spaces-direct-volumes"></a>管理存储空间直通的卷

<ul>
               <li>(0:37)<a href="https://youtu.be/o66etKq70N8">如何创建三向镜像卷</a></li>
               <li>(1:17)<a href="https://youtu.be/R72QHudqWpE">如何创建镜像加速奇偶校验卷</a></li>
               <li>(1:02)<a href="https://youtu.be/j59z7ulohs4">如何打开卷，并将文件添加</a></li>
               <li>(0:51)<a href="https://youtu.be/PRibTacyKko">如何启用重复数据删除和压缩</a></li>
               <li>(0:47)<a href="https://youtu.be/hqyBzipBoTI">如何扩展卷</a></li>
               <li>(0:26)<a href="https://youtu.be/DbjF8r2F6Jo">如何删除卷</a></li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>创建卷，三向镜像</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>创建卷，镜像加速奇偶校验</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/R72QHudqWpE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>打开卷，并将文件添加</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/j59z7ulohs4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>启用重复数据删除和压缩</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/PRibTacyKko" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>扩展卷</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/hqyBzipBoTI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>删除卷</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### <a name="create-a-new-virtual-machine"></a>创建新的虚拟机

1. 单击**虚拟机**从左侧导航窗格中的工具。
2. 在虚拟机工具的顶部，选择**清单**选项卡，然后单击**新建**若要创建新的虚拟机。
3. 输入虚拟机名称，第 1 和 2 代虚拟机之间进行选择。
4. 对你来说易于然后可以选择哪些主机最初在创建虚拟机或使用建议的主机。
5. 选择虚拟机文件的路径。 从下拉列表中选择一个卷，或单击**浏览**选择使用文件夹选取器的文件夹。 虚拟机配置文件和虚拟硬盘文件将保存在一个文件夹下`\Hyper-V\[virtual machine name]`选定的卷或路径的路径。
6. 是否需要启用嵌套虚拟化、 配置内存设置、 网络适配器、 虚拟硬盘和选择是否想要从.iso 映像文件或从网络安装操作系统，请选择虚拟处理器的数。
7. 单击 **“创建”** 创建虚拟机。
8. 虚拟机创建并显示虚拟机列表中，您可以启动虚拟机。
9. 虚拟机启动后，你可以连接到通过 VMConnect 以安装操作系统的虚拟机的控制台。 从列表中选择虚拟机中，单击**更多** > **Connect**下载.rdp 文件。 在远程桌面连接应用中打开.rdp 文件。 由于这连接到虚拟机的控制台，你需要输入的 HYPER-V 主机的管理员凭据。

[要详细了解虚拟机管理与 Windows Admin Center](manage-virtual-machines.md)。

### <a name="pause-and-safely-restart-a-server"></a>暂停并安全地重新启动服务器

1. 从**仪表板**，选择**服务器**从左侧和右侧或通过单击导航栏**查看服务器 >** 仪表板的右下角中的磁贴的链接.
2. 在顶部，从切换**摘要**到**清单**选项卡。
3. 通过单击其名称以打开选择的服务器**Server**详细信息页。
4. 单击**暂停服务器以进行维护**。 如果可以安全地继续操作，这将将虚拟机移至群集中的其他服务器。 服务器将有状态排出而发生这种情况。 如果你想，您可以观看上移动的虚拟机**虚拟机 > 库存**页，其中其主机服务器清楚地显示在网格中。 服务器状态时移动的所有虚拟机后，将为**已暂停**。
5. 单击**管理服务器**访问 Windows Admin Center 中的所有每个服务器管理工具。
6. 单击**重新启动**，然后**是**。 您将重新启动到连接列表。
7. 重新**仪表板**，服务器已关闭时是以红色显示。
8. 重新启动后，再次导航**服务器**页上，单击**恢复服务器从维护**到只需最设置服务器状态。 中的时间虚拟机将返回的-不不需要任何用户操作。

### <a name="replace-a-failed-drive"></a>替换发生故障的驱动器

1. 当驱动器失败时，警报显示在左上角**警报**区域中的**仪表板**。
2. 您还可以选择**驱动器**从左侧和右侧上单击导航栏**视图驱动器 >** 右下角以浏览驱动器并亲自查看它们的状态中的磁贴的链接。 在中**清单**选项卡上，网格支持排序、 分组和关键字搜索。
3. 从**仪表板**，单击警报以查看详细信息，如驱动器的物理位置。
4. 若要了解详细信息，请单击**转到驱动器**快捷方式**驱动器**详细信息页。
5. 如果您的硬件支持，则可以单击**开启/关闭 light**来控制驱动器的指示灯。
6. 存储空间直通自动停用并时撤离故障的驱动器。 当发生此情况时，驱动器状态已停用，并且其存储的容量限制为空。
7. 删除故障的驱动器，并插入其更换部件。
8. 在中**驱动器 > 清单**，将显示该新驱动器。 中时，将清除警报，返回到正常状态，将修复卷和存储到新的驱动器上会重新平衡 – 不不需要任何用户操作。

### <a name="manage-virtual-networks-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>管理虚拟网络 （SDN 启用 HCI 群集使用 Windows Admin Center 预览）

1. 选择**虚拟网络**从左侧的导航。
2. 单击**新建**若要创建新的虚拟网络和子网，或选择现有的虚拟网络，然后单击**设置**来修改其配置。
3. 单击以查看 VM 连接到虚拟网络子网和访问控制列表应用于虚拟网络子网的现有虚拟网络。

![管理虚拟网络](../media/manage-hyper-converged/manage-virtual-networks.png)

### <a name="connect-a-virtual-machine-to-a-virtual-network-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>虚拟机连接到虚拟网络 （SDN 启用 HCI 群集使用 Windows Admin Center 预览）

1. 选择**虚拟机**从左侧的导航。
2. 选择现有的虚拟机 > 单击**设置**> 打开**网络**选项卡中**设置**。
3. 配置**虚拟网络**并**虚拟子网**字段以将虚拟机连接到虚拟网络。

创建虚拟机时，还可以配置虚拟网络。

![虚拟机连接到虚拟网络](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### <a name="monitor-software-defined-networking-infrastructure-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>监视软件定义的网络基础结构 （使用 Windows Admin Center 预览 SDN 启用 HCI 群集）

1. 选择**SDN 监视**从左侧的导航。
2. 查看有关运行状况的网络控制器、 软件负载平衡器虚拟网关的详细的信息和监视虚拟网关池、 公共和专用 IP 池使用情况和 SDN 主机状态。

![监视器 SDN 基础结构](../media/manage-hyper-converged/sdn-monitoring.png)

## <a name="feedback"></a>反馈

只需决定你的反馈 ！ 频繁更新的最大优势是听到什么在起作用，哪些方面需要能够改进。 下面是一些方法，让我们知道您正在想什么：

- [提交并在 UserVoice 上发出功能请求投票](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [加入 Windows Admin Center 论坛上 Microsoft 技术社区](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- 对推文 `@servermgmt`

### <a name="see-also"></a>请参阅

- [Windows Admin Center](../understand/windows-admin-center.md)
- [存储空间直通](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [软件定义的网络](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)

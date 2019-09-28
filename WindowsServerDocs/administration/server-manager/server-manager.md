---
title: 服务器管理器
description: 服务器管理器
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d996ef40-8bcc-42b0-b6ae-806b828223f6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 373e2063622317905686b1c5fc74425943abd9ec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383064"
---
# <a name="server-manager"></a>服务器管理器

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

服务器管理器是 Windows Server 中的管理控制台，可帮助 IT 专业人员从其桌面预配和管理基于 Windows 的本地和远程服务器，而无需物理访问服务器或启用远程桌面协议（rdP）连接到每台服务器。 虽然 Windows Server 2008 R2 和 Windows Server 2008 中提供了服务器管理器，但服务器管理器已在 Windows Server 2012 中更新，以支持远程、多服务器管理，并帮助增加管理员可管理的服务器数量。

在我们的测试中，Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中的服务器管理器可用于管理最多100服务器，具体取决于服务器运行的工作负荷。 可以使用单一服务器管理器控制台管理的服务器数量可能取决于从托管服务器请求的数据量，以及可用于运行服务器管理器的计算机的硬件和网络资源的量。 当你想要显示的数据量接近该计算机的资源容量时，服务器管理器的响应可能变慢，刷新可能延迟完成。 若要使用服务器管理器管理更多服务器，我们建议使用**配置事件数据**对话框中的设置，限制服务器管理器从你的托管服务器获取的事件数据。 “配置事件数据”可从“事件” 磁贴中的“任务” 菜单打开。 如果你需要管理组织中服务器的企业级数量，我们建议评估 [Microsoft System Center 套件](https://go.microsoft.com/fwlink/p/?LinkId=239437)中的产品。

本主题及其子主题提供了有关如何在服务器管理器控制台中使用功能的信息。 本主题包含以下部分。

-   [查看初始注意事项和系统要求](#review-initial-considerations-and-system-requirements)

-   [可在中执行的任务服务器管理器](#tasks-that-you-can-perform-in-server-manager)

-   [开始服务器管理器](#start-server-manager)

-   [重新启动远程服务器](#restart-remote-servers)

-   [将服务器管理器设置导出到其他计算机](#export-server-manager-settings-to-other-computers)

## <a name="review-initial-considerations-and-system-requirements"></a>查看初步注意事项和系统要求
以下部分列出了需要查看的一些初始注意事项，以及服务器管理器的硬件和软件要求。

### <a name="hardware-requirements"></a>硬件要求
默认情况下，Windows Server 2016 的所有版本都安装服务器管理器。 服务器管理器不存在任何其他硬件要求。

### <a name="software-and-configuration-requirements"></a>软件和配置要求
默认情况下，Windows Server 2016 的所有版本都安装服务器管理器。 你可以使用 Windows Server 2016 中的服务器管理器来管理在远程计算机上运行的 Windows Server 2016、Windows Server 2012 和 Windows Server 2008 R2 的[服务器核心安装选项](https://go.microsoft.com/fwlink/p/?LinkID=241573)。 服务器管理器在 Windows Server 2016 的服务器核心安装选项上运行。

服务器管理器在最精简服务器图形界面中运行;也就是说，如果未安装服务器图形 Shell 功能，则为。 默认情况下，在 Windows Server 2016 上不安装服务器图形 Shell 功能。 如果未运行服务器图形 Shell，则服务器管理器控制台将运行，但控制台中提供的某些应用程序或工具不可用。 Internet 浏览器无法在没有服务器图形 Shell 的情况下运行，因此无法打开 HTML 帮助（例如，mmc F1 帮助）等网页和应用程序。 如果未安装服务器图形 Shell，则无法打开用于配置 Windows 自动更新和反馈的对话框;在服务器管理器控制台中打开这些对话框的命令会被重定向到运行**sconfig.cmd**。

若要管理运行早于 Windows Server 2016 的 Windows Server 版本的服务器，请安装以下软件和更新，以使用 Windows Server 2016 中的服务器管理器来管理较旧版本的 Windows Server。

|操作系统|所需软件|
|----------|-----------|
| Windows Server 2012 R2 或 Windows Server 2012 |-   [.NET Framework 4.6](https://www.microsoft.com/download/details.aspx?id=45497)<br />@no__t 0[Windows Management Framework 5.0](https://go.microsoft.com/fwlink/?LinkID=395058)。 Windows Management Framework 5.0 下载包更新 Windows Server 2012 R2 和 Windows Server 2012 上的 Windows Management Instrumentation （WMI）提供程序。 更新的 WMI 提供程序让服务器管理器收集有关在托管服务器上安装的角色和功能的信息。 在应用更新之前，运行 Windows Server 2012 R2 或 Windows Server 2012 的服务器的可管理性状态为 "**无法访问**"。<br />-在运行 Windows Server 2012 R2 或 Windows Server 2012 的服务器上不再需要与[知识库文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487)关联的性能更新。|
| Windows Server 2008 R2 |-   [.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)<br />@no__t 0[Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881)。 Windows Management Framework 4.0 下载包更新 Windows Management Instrumentation （WMI）提供程序（在 Windows Server 2008 R2 上）。 更新的 WMI 提供程序让服务器管理器收集有关在托管服务器上安装的角色和功能的信息。 在应用更新之前，运行 Windows Server 2008 R2 的服务器的可管理性状态为 "**无法访问**"。<br />-与[知识库文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487)相关的性能更新允许服务器管理器从 Windows Server 2008 R2 收集性能数据。|
| Windows Server 2008 |-   [.NET Framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)<br />@no__t 0[Windows Management framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) ： Windows management framework 3.0 下载包更新 windows Server 2008 上的 WINDOWS MANAGEMENT INSTRUMENTATION （WMI）提供程序。 更新的 WMI 提供程序让服务器管理器收集有关在托管服务器上安装的角色和功能的信息。 在应用更新之前，运行 Windows Server 2008 的服务器的可管理性状态为 "**无法访问"-验证早期版本是否运行 Windows Management Framework 3.0**。<br />-与[知识库文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487)相关的性能更新允许服务器管理器从 Windows Server 2008 收集性能数据。|

#### <a name="manage-remote-computers-from-a-client-computer"></a>从客户端计算机管理远程计算机
Windows 10[远程服务器管理工具](https://go.microsoft.com/fwlink/?LinkID=404281)包含服务器管理器控制台。 请注意，当远程服务器管理工具安装在客户端计算机上时，你无法使用服务器管理器来管理本地计算机;服务器管理器不能用于管理运行 Windows 客户端操作系统的计算机或设备。 只能使用服务器管理器来管理基于 Windows 的服务器。

|服务器管理器源操作系统|面向 Windows Server 2016|面向 Windows Server 2012 R2 |面向 Windows Server 2012 |面向 Windows Server 2008 R2 或 Windows Server 2008 |针对 Windows Server 2003|
|-------------------------------|--------------------------------------------|---------------------------------------|------------------------------------|-----------------------------------------------------------------------|------------------|
|Windows 10 或 Windows Server 2016|完全支持|完全支持|完全支持|在满足[软件和配置需求](#software-and-configuration-requirements)后，可以执行大多数管理任务，但无法进行角色或功能安装或卸载|不支持|
|Windows 8.1 或 Windows Server 2012 R2 |不支持|完全支持|完全支持|在满足[软件和配置需求](#software-and-configuration-requirements)后，可以执行大多数管理任务，但无法进行角色或功能安装或卸载|有限支持；仅在线和离线状态|
|Windows 8 或 Windows Server 2012 |不支持|不支持|完全支持|在满足[软件和配置需求](#software-and-configuration-requirements)后，可以执行大多数管理任务，但无法进行角色或功能安装或卸载|有限支持；仅在线和离线状态|

###### <a name="to-start-server-manager-on-a-client-computer"></a>在客户端计算机上启动服务器管理器的步骤

1.  按照[远程服务器管理工具](../../remote/remote-server-administration-tools.md)安装适用于 Windows 10 的远程服务器管理工具中的说明进行操作。

2.  在 "**开始**" 屏幕上，单击 "**服务器管理器**"。 安装远程服务器管理工具后，可使用“服务器管理器” 磁贴。

3.  如果安装远程服务器管理工具之后，**管理工具**和**服务器管理器**磁贴都未显示在 "**开始**" 屏幕上，并且在 "**开始**" 屏幕上搜索服务器管理器不显示结果，验证是否启用了 "**显示管理工具**" 设置。 若要查看此设置，请将鼠标光标悬停在 "**开始**" 屏幕的右上角，然后单击 "**设置**"。 如果“显示管理工具”已关闭，请打开该设置，显示已作为远程服务器管理工具一部分安装的工具。

有关运行 Windows 10 远程服务器管理工具以管理远程服务器的详细信息，请参阅 TechNet Wiki 上的[远程服务器管理工具](https://go.microsoft.com/fwlink/?LinkID=221055)。

#### <a name="configure-remote-management-on-servers-that-you-want-to-manage"></a>在你想要进行管理的服务器上配置远程管理

> [!IMPORTANT]
> 默认情况下，在 Windows Server 2016 中启用服务器管理器和 Windows PowerShell 远程管理。

若要使用服务器管理器在远程服务器上执行管理任务，必须将你要管理的远程服务器配置为允许使用服务器管理器和 Windows PowerShell 进行远程管理。 如果在 Windows Server 2012 R2 或 Windows Server 2012 上禁用了远程管理，并且你想要再次启用它，请执行以下步骤。

##### <a name="to-configure-server-manager-remote-management-on--windows-server-2012-r2--or--windows-server-2012--by-using-the-windows-interface"></a>使用 Windows 界面在 Windows Server 2012 R2 或 Windows Server 2012 上配置服务器管理器远程管理

1.  > [!NOTE]
    > "**配置远程管理**" 对话框所控制的设置不会影响使用 DCOM 进行远程通信的服务器管理器部分。

    执行下列操作之一，打开服务器管理器（如果尚未打开）。

    -   在 Windows 任务栏上，单击 "服务器管理器" 按钮。

    -   在 "**开始**" 屏幕上，单击 "**服务器管理器**"。

2.  在 "**本地服务器**" 页的 "**属性**" 区域中，单击 "**远程管理**" 属性的超链接值。

3.  执行下列操作之一，然后单击“确认”。

    -   若要阻止使用服务器管理器（或 Windows PowerShell，如果已安装）远程管理此计算机，请清除 "**从其他计算机启用此服务器的远程管理**" 复选框。

    -   若要通过使用服务器管理器或 Windows PowerShell 来远程管理此计算机，请选择 "**从其他计算机启用此服务器的远程管理**"。

##### <a name="to-enable-server-manager-remote-management-on--windows-server-2012-r2--or--windows-server-2012--by-using-windows-powershell"></a>使用 Windows PowerShell 在 Windows Server 2012 R2 或 Windows Server 2012 上启用服务器管理器远程管理

1.  请执行以下操作之一。

    -   若要从 "**开始**" 屏幕以管理员身份运行 windows powershell，请右键单击 " **windows powershell** " 磁贴，然后单击 "以**管理员身份运行**"。

    -   若要从桌面以管理员身份运行 Windows PowerShell，请在任务栏中右键单击**Windows powershell**快捷方式，然后单击 "以**管理员身份运行**"。

2.  键入以下项，然后按**enter**启用所有必需的防火墙规则例外。

    **Configure-smremoting.exe-Enable**

    > [!NOTE]
    > 此命令也适用于在提升的用户权限（“以管理员身份运行”）打开的命令提示符。

    如果启用远程管理失败，请参阅 Microsoft TechNet 上的[about_remote_Troubleshooting](https://go.microsoft.com/fwlink/p/?LinkID=135188) ，以获取故障排除提示和最佳做法。

###### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-older-operating-systems"></a>在较早版本操作系统上启用服务器管理器和 Windows PowerShell 远程管理的步骤

-   请执行以下操作之一。

    -   若要在运行 Windows Server 2008 R2 的服务器上启用远程管理，请参阅 Windows Server 2008 R2 帮助中的[使用服务器管理器进行远程管理](https://go.microsoft.com/fwlink/?LinkID=137378)。

    -   若要在运行 Windows Server 2008 的服务器上启用远程管理，请参阅[在 Windows PowerShell 中启用和使用远程命令](https://go.microsoft.com/fwlink/p/?LinkId=242565)。

## <a name="tasks-that-you-can-perform-in-server-manager"></a>可在服务器管理器中执行的任务
通过允许管理员使用单个工具执行下表中的任务，服务器管理器使服务器管理更高效。 在 Windows Server 2012 R2 和 Windows Server 2012 中，服务器的标准用户和管理员组的成员都可以在服务器管理器执行管理任务，但默认情况下，会阻止标准用户执行某些任务，如下表。

管理员可以使用服务器管理器 cmdlet 模块中的两个 Windows PowerShell cmdlet [disable-servermanagerstandarduserremoting](https://technet.microsoft.com/library/jj205470.aspx)和[disable-servermanagerstandarduserremoting](https://technet.microsoft.com/library/jj205468.aspx)，进一步控制对某些其他数据。 **Disable-servermanagerstandarduserremoting** cmdlet 可向一个或多个标准、非管理员用户提供对事件、服务、性能计数器以及角色和功能清单数据的访问权限。

> [!IMPORTANT]
> 服务器管理器无法用于管理较新版本的 Windows Server 操作系统。 服务器管理器在 Windows Server 2012 或 Windows 8 上运行的不能用于管理运行 Windows Server 2012 R2 的服务器。

|任务描述|管理员（包括内置管理员帐户）|标准服务器用户|
|----------|----------------------------------|-------------|
|将远程服务器添加到服务器管理器可用于管理的服务器池。|是|否|
|创建和编辑自定义服务器组，如位于特定地理位置的服务器，或者用于特定用途的服务器。|是|是|
|在运行 Windows Server 2012 R2 或 Windows Server 2012 的本地或远程服务器上安装或卸载角色、角色服务和功能。 有关角色、角色服务和功能的定义，请参阅[角色、角色服务和功能](https://go.microsoft.com/fwlink/p/?LinkId=239558)。|是|否|
|查看和更改本地或远程服务器上安装的服务器角色及功能。 **注意：** 在服务器管理器中，角色和功能数据以系统的基本语言（也称为系统默认 GUI 语言）或操作系统安装过程中选择的语言显示。|是|标准用户可以查看并管理角色和功能，并执行如查看角色事件之类的任务，但无法添加或删除角色服务。|
|启动管理工具，例如 Windows PowerShell 或 mmc 管理单元。您可以通过右键单击 "**服务器**" 磁贴中的服务器，然后单击 " **Windows powershell**"，启动一个针对远程服务器的 Windows powershell 会话。 可以从服务器管理器控制台的 "**工具**" 菜单启动 mmc 管理单元，然后在管理单元打开后将 mmc 指向远程计算机。|是|是|
|通过右键单击“服务器” 磁贴中的服务器，然后单击“管理形式”，使用不同的凭据管理远程服务器。 可将“管理形式”用于常规服务器和“文件和存储服务”管理任务|是|否|
|执行与服务器运行生命周期关联的管理任务，如启动或停止服务;然后启动其他工具来配置服务器的网络设置、用户和组以及远程桌面连接。|是|标准用户无法启动或停止服务。 他们可以更改本地服务器的名称、工作组或域成员身份和 "远程桌面" 设置，但 "用户帐户控制" 会提示你提供管理员凭据，然后才能完成这些任务。 他们无法更改远程管理设置。|
|执行与服务器上安装的角色运行生命周期关联的管理任务，包括扫描某些角色，看其是否符合最佳做法。|是|标准用户无法运行最佳做法分析器扫描。|
|确定服务器状态，标识关键事件，分析并解决配置问题和故障。|是|是|
|自定义事件、性能数据、服务以及要在服务器管理器仪表板上向其发出警报的最佳做法分析器结果。|是|是|
|重新启动服务器。|是|否|
|刷新有关托管服务器的服务器管理器控制台中显示的数据。|是|否|

> [!NOTE]
> 服务器管理器不能用于将角色和功能添加到运行 Windows Server 2008 R2 或 Windows Server 2008 的服务器。

## <a name="start-server-manager"></a>开始服务器管理器
当 Administrators 组的成员登录到服务器时，默认情况下，服务器管理器将在运行 Windows Server 2016 的服务器上自动启动。 如果关闭服务器管理器，请通过以下方法之一重启它。 本部分还包含更改默认行为和阻止服务器管理器自动启动的步骤。

#### <a name="to-start-server-manager-from-the-start-screen"></a>从 "开始" 屏幕启动服务器管理器

-   在 Windows 的 "**开始**" 屏幕上，单击 "**服务器管理器**" 磁贴。

#### <a name="to-start-server-manager-from-the-windows-desktop"></a>从 Windows 桌面打开服务器管理器的步骤

-   在 Windows 任务栏上，单击“服务器管理器”。

#### <a name="to-prevent-server-manager-from-starting-automatically"></a>阻止服务器管理器自动启动的步骤

1.  在服务器管理器控制台中，在 "**管理**" 菜单上单击 "**服务器管理器属性**"。

2.  在“服务器管理器属性” 对话框中，选中“在登录时不自动启动服务器管理器”的复选框。 单击 **“确定”** 。

3.  或者，你可以通过启用组策略设置来阻止服务器管理器自动启动，**而不会在登录时自动启动服务器管理器**。 此策略设置在 "本地组策略编辑器" 控制台中的路径是 "计算机配置 \ 管理模板" 系统 "Manager"。

## <a name="restart-remote-servers"></a>重新启动远程服务器
可以从服务器管理器中角色或组页面的 "**服务器**" 磁贴重新启动远程服务器。

> [!IMPORTANT]
> 重新启动远程服务器迫使服务器重新启动，即使用户仍然登录远程服务器，即使数据未保存的程序仍打开，也是如此。 此行为不同于关闭或重新启动本地计算机，在后一种情况下，会提示你保存未保存的程序数据，并确认要强制已登录用户登出。 确认你可以强制其他用户登出远程服务器，并且可以放弃运行在远程服务器上程序的未保存数据。
> 
> 如果在管理的服务器关闭和重新启动时服务器管理器中发生自动刷新，则托管服务器可能会发生刷新和可管理性状态错误，因为服务器管理器无法连接到远程服务器，直到其完成重新.

#### <a name="to-restart-remote-servers-in-server-manager"></a>在服务器管理器中重新启动远程服务器的步骤

1.  在服务器管理器中打开角色或服务器组主页。

2.  选择一个或多个已添加到服务器管理器的远程服务器。 按住 Ctrl 并一次性单击选择多个服务器。 有关如何将服务器添加到服务器管理器服务器池的详细信息，请参阅[将服务器添加到服务器管理器](add-servers-to-server-manager.md)。

3.  右键单击选中的服务器，然后单击“重新启动服务器”。

## <a name="export-server-manager-settings-to-other-computers"></a>将服务器管理器设置导入其他计算机
在服务器管理器中，托管服务器的列表、对服务器管理器控制台设置的更改和已创建的自定义组存储在以下两个文件中。 你可以在运行同一版本服务器管理器（或安装了远程服务器管理工具的 Windows 10）的其他计算机上重复使用这些设置。 远程服务器管理工具必须在基于 Windows 客户端的计算机上运行，才能将服务器管理器设置导出到这些计算机。

-   %*appdata*% \ Microsoft\Windows\ServerManager\Serverlist.xml

-   %*appdata*% \ Local\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

> [!NOTE]
> -   你服务器池中服务器的“管理形式”（或替代）凭据不被存储在漫游配置文件中。 服务器管理器用户必须将凭据添加到想要管理的每台计算机上。
> -   网络共享漫游配置文件在用户登录到网络之后才会创建，然后第一次将其注销。 **Serverlist.xml** 文件在此时创建。

可以通过以下两种方式之一导出服务器管理器设置、使服务器管理器的设置可移植，或在其他计算机上使用它们。

-   若要将设置导出到另一台加入域的计算机，请在 "active directory 用户和计算机" 中将服务器管理器用户配置为使用漫游配置文件。 只有域管理员才能更改 "active directory 用户和计算机" 中的用户属性。

-   若要将设置导出到工作组中的另一台计算机，请使用服务器管理器将上述两个文件复制到要从中管理的计算机上的同一位置。

#### <a name="to-export-server-manager-settings-to-other-domain-joined-computers"></a>将服务器管理器设置导入其他加入域的计算机的步骤

1.  在 "active directory 用户和计算机" 中，打开服务器管理器用户的 "**属性**" 对话框。

2.  在 "**配置文件**" 选项卡上，添加一个指向网络共享的路径，以存储用户的配置文件。

3.  请执行以下操作之一。

    -   在美国英语（zh-cn）生成，对**Serverlist**文件所做的更改会自动保存到配置文件中。 继续进行下一步。

    -   在其他版本上，从运行服务器管理器的计算机将以下两个文件复制到作为用户漫游配置文件一部分的网络共享。

        -   %*appdata*% \ Microsoft\Windows\ServerManager\Serverlist.xml

        -   %*localappdata*% \ Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

4.  单击“确定” 保存你的更改，然后关闭“属性” 对话框。

#### <a name="to-export-server-manager-settings-to-computers-in-workgroups"></a>导出“服务器管理器”设置到工作组计算机的方法

-   在要从中管理远程服务器的计算机上，使用运行服务器管理器的另一台计算机中的相同文件覆盖以下两个文件，并使用所需的设置。

    -   %*appdata*% \ Microsoft\Windows\ServerManager\Serverlist.xml

    -   %*localappdata*% \ Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config




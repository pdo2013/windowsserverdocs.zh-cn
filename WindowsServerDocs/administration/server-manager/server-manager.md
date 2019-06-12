---
title: 服务器管理器
description: 服务器管理器
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e3f3abeec3d4ecbe5e80d08a99a00b43a408c4ac
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811284"
---
# <a name="server-manager"></a>服务器管理器

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

服务器管理器是 Windows Server，这样有助于 IT 专业人员预配和管理本地和远程基于 Windows 的服务器从其桌面，而无需物理访问服务器，或者需要启用远程桌面中的管理控制台每个服务器协议 (rdP) 连接。 尽管服务器管理器是在 Windows Server 2008 R2 和 Windows Server 2008 中可用，但服务器管理器已更新在 Windows Server 2012 中支持远程、 多服务器管理，并帮助增加管理员可以管理的服务器数量。

在我们的测试，Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 中的服务器管理器可以用于管理最多 100 台服务器，具体取决于服务器正在运行的工作负荷。 可以使用单一服务器管理器控制台管理的服务器数量可能取决于从托管服务器请求的数据量，以及可用于运行服务器管理器的计算机的硬件和网络资源的量。 当你想要显示的数据量接近该计算机的资源容量时，服务器管理器的响应可能变慢，刷新可能延迟完成。 若要使用服务器管理器管理更多服务器，我们建议使用**配置事件数据**对话框中的设置，限制服务器管理器从你的托管服务器获取的事件数据。 “配置事件数据”可从“事件”  磁贴中的“任务”  菜单打开。 如果你需要管理组织中服务器的企业级数量，我们建议评估 [Microsoft System Center 套件](https://go.microsoft.com/fwlink/p/?LinkId=239437)中的产品。

本主题及其子主题提供有关如何使用服务器管理器控制台中的功能的信息。 本主题包含以下部分。

-   [查看初步注意事项和系统要求](#review-initial-considerations-and-system-requirements)

-   [你可以执行服务器管理器中的任务](#tasks-that-you-can-perform-in-server-manager)

-   [启动服务器管理器](#start-server-manager)

-   [重新启动远程服务器](#restart-remote-servers)

-   [服务器管理器设置导出到其他计算机](#export-server-manager-settings-to-other-computers)

## <a name="review-initial-considerations-and-system-requirements"></a>查看初步注意事项和系统要求
以下部分列出了一些需要检查，以及硬件和软件要求的服务器管理器的初步注意事项。

### <a name="hardware-requirements"></a>硬件要求
默认情况下，所有版本的 Windows Server 2016 安装服务器管理器。 没有额外的硬件要求存在于服务器管理器。

### <a name="software-and-configuration-requirements"></a>软件和配置要求
默认情况下，所有版本的 Windows Server 2016 安装服务器管理器。 可以使用 Windows Server 2016 中的服务器管理器来管理[服务器核心安装选项](https://go.microsoft.com/fwlink/p/?LinkID=241573)的 Windows Server 2016、 Windows Server 2012 和 Windows Server 2008 R2 的远程计算机上运行。 服务器管理器中但在 Windows Server 2016 的服务器核心安装选项上运行。

最小的服务器图形界面; 在运行服务器管理器也就是说，如果未安装服务器图形 Shell 功能是。 默认情况下，Windows Server 2016 上未安装服务器图形 Shell 功能。 如果你不运行服务器图形 Shell，服务器管理器控制台运行，但某些应用程序或工具可从控制台不可用。 Internet 浏览器不能运行而无需服务器图形 Shell、 等网页和应用程序，如无法打开 HTML 帮助 （例如 mmc F1 帮助）。 无法打开有关配置 Windows 自动更新和反馈时未安装服务器图形 Shell;在服务器管理器控制台中打开这些对话框的命令经过重定向，以运行**sconfig.cmd**。

若要管理运行 Windows Server 版本早于 Windows Server 2016 的服务器，请安装以下软件和更新，以便较旧版本的 Windows Server 可管理 Windows Server 2016 中使用服务器管理器。

|操作系统|所需软件|
|----------|-----------|
| Windows Server 2012 R2 或 Windows Server 2012 |-   [.NET Framework 4.6](https://www.microsoft.com/download/details.aspx?id=45497)<br />-   [Windows Management Framework 5.0](https://go.microsoft.com/fwlink/?LinkID=395058)。 Windows Management Framework 5.0 下载程序包更新在 Windows Server 2012 R2 和 Windows Server 2012 上的 Windows Management Instrumentation (WMI) 提供程序。 已更新的 WMI 提供程序，服务器管理器收集有关托管服务器上安装的角色和功能的信息。 在应用更新之前运行 Windows Server 2012 R2 的服务器或 Windows Server 2012 的可管理性状态已**无法访问**。<br />的与相关性能更新[知识库文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487)不再需要运行 Windows Server 2012 R2 的服务器或 Windows Server 2012 上。|
| Windows Server 2008 R2 |-   [.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)<br />-   [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881)。 Windows Management Framework 4.0 下载包更新 Windows Server 2008 R2 上的 Windows Management Instrumentation (WMI) 提供程序。 已更新的 WMI 提供程序，服务器管理器收集有关托管服务器上安装的角色和功能的信息。 在应用更新之前运行 Windows Server 2008 R2 的服务器具有的可管理性状态**无法访问**。<br />的与相关性能更新[知识库文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487)允许服务器管理器从 Windows Server 2008 R2 中收集性能数据。|
| Windows Server 2008 |-   [.NET framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)<br />-   [Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) Windows Management Framework 3.0 下载包更新 Windows Server 2008 上的 Windows Management Instrumentation (WMI) 提供程序。 已更新的 WMI 提供程序，服务器管理器收集有关托管服务器上安装的角色和功能的信息。 在应用更新之前运行 Windows Server 2008 的服务器具有的可管理性状态**不可访问-确定早期版本运行 Windows Management Framework 3.0**。<br />的与相关性能更新[知识库文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487)允许服务器管理器从 Windows Server 2008 中收集性能数据。|

#### <a name="manage-remote-computers-from-a-client-computer"></a>从客户端计算机管理远程计算机
服务器管理器控制台是附带[远程服务器管理工具](https://go.microsoft.com/fwlink/?LinkID=404281)适用于 Windows 10。 请注意，客户端计算机上安装远程服务器管理工具时，你无法管理本地计算机，使用服务器管理器;服务器管理器不能用于管理运行 Windows 客户端操作系统的计算机或设备。 服务器管理器仅可用于管理基于 Windows 的服务器。

|服务器管理器源操作系统|针对 Windows Server 2016|针对 Windows Server 2012 R2 |针对 Windows Server 2012 |针对 Windows Server 2008 R2 或 Windows Server 2008 |针对 Windows Server 2003|
|-------------------------------|--------------------------------------------|---------------------------------------|------------------------------------|-----------------------------------------------------------------------|------------------|
|Windows 10 或 Windows Server 2016|完全支持|完全支持|完全支持|在满足[软件和配置需求](#software-and-configuration-requirements)后，可以执行大多数管理任务，但无法进行角色或功能安装或卸载|不支持|
|Windows 8.1 或 Windows Server 2012 R2 |不支持|完全支持|完全支持|在满足[软件和配置需求](#software-and-configuration-requirements)后，可以执行大多数管理任务，但无法进行角色或功能安装或卸载|有限支持；仅在线和离线状态|
|Windows 8 或 Windows Server 2012 |不支持|不支持|完全支持|在满足[软件和配置需求](#software-and-configuration-requirements)后，可以执行大多数管理任务，但无法进行角色或功能安装或卸载|有限支持；仅在线和离线状态|

###### <a name="to-start-server-manager-on-a-client-computer"></a>在客户端计算机上启动服务器管理器的步骤

1.  按照中的说明进行操作[远程服务器管理工具](../../remote/remote-server-administration-tools.md)安装远程服务器管理工具适用于 Windows 10。

2.  上**启动**屏幕上，单击**服务器管理器**。 安装远程服务器管理工具后，可使用“服务器管理器”  磁贴。

3.  如果既没有**管理工具**也不**服务器管理器**磁贴显示在**启动**安装远程服务器管理工具后的屏幕和搜索服务器管理器**启动**屏幕未显示结果中，确保**显示管理工具**设置已打开。 若要查看此设置，鼠标光标悬停在右上角**启动**屏幕上，然后依次**设置**。 如果“显示管理工具”  已关闭，请打开该设置，显示已作为远程服务器管理工具一部分安装的工具。

有关运行远程服务器管理 Windows 10 的工具来管理远程服务器的详细信息，请参阅[远程服务器管理工具](https://go.microsoft.com/fwlink/?LinkID=221055)TechNet Wiki 上。

#### <a name="configure-remote-management-on-servers-that-you-want-to-manage"></a>在你想要进行管理的服务器上配置远程管理

> [!IMPORTANT]
> 默认情况下，Windows Server 2016 中启用服务器管理器和 Windows PowerShell 远程管理。

若要使用服务器管理器远程服务器上执行管理任务，你想要管理的远程服务器必须配置为允许远程管理使用服务器管理器和 Windows PowerShell。 如果你想要再次启用远程管理已禁用 Windows Server 2012 R2 或 Windows Server 2012 上，执行以下步骤。

##### <a name="to-configure-server-manager-remote-management-on--windows-server-2012-r2--or--windows-server-2012--by-using-the-windows-interface"></a>若要配置服务器管理器远程管理 Windows Server 2012 R2 或 Windows Server 2012 上使用 Windows 界面

1.  > [!NOTE]
    > 通过控制的设置**配置远程管理**对话框不会影响部分使用 DCOM 进行远程通信的服务器管理器。

    执行下列操作之一以打开服务器管理器中，如果它不是已打开。

    -   在 Windows 任务栏上，单击服务器管理器按钮。

    -   上**启动**屏幕上，单击**服务器管理器**。

2.  在中**属性**区域中的**本地服务器**页上，单击超链接值**远程管理**属性。

3.  执行下列操作之一，然后单击“确认”  。

    -   若要阻止此计算机远程管理使用服务器管理器 （或如果已安装的 Windows PowerShell），请清除**启用远程管理此服务器从其他计算机**复选框。

    -   若要允许通过服务器管理器或 Windows PowerShell 远程管理此计算机，请选择**启用远程管理此服务器从其他计算机**。

##### <a name="to-enable-server-manager-remote-management-on--windows-server-2012-r2--or--windows-server-2012--by-using-windows-powershell"></a>若要使用 Windows PowerShell 启用服务器管理器远程管理 Windows Server 2012 R2 或 Windows Server 2012 上

1.  请执行以下操作之一。

    -   若要以管理员身份从运行 Windows PowerShell**启动**屏幕上，右键单击**Windows PowerShell**磁贴，，然后单击**以管理员身份运行**。

    -   若要从桌面以管理员身份运行 Windows PowerShell，右键单击**Windows PowerShell**快捷方式在任务栏中，然后单击**以管理员身份运行**。

2.  键入以下内容，，然后按**Enter**启用所有必需的防火墙规则例外。

    **Configure-SMremoting.exe -Enable**

    > [!NOTE]
    > 此命令也适用于在提升的用户权限（“以管理员身份运行”）打开的命令提示符。

    如果启用远程管理失败，请参阅[about_remote_Troubleshooting](https://go.microsoft.com/fwlink/p/?LinkID=135188)进行故障排除提示和最佳实践的 Microsoft TechNet 上。

###### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-older-operating-systems"></a>在较早版本操作系统上启用服务器管理器和 Windows PowerShell 远程管理的步骤

-   请执行以下操作之一。

    -   若要启用远程管理运行 Windows Server 2008 R2 的服务器上，请参阅[远程管理使用服务器管理器](https://go.microsoft.com/fwlink/?LinkID=137378)Windows Server 2008 R2 帮助中。

    -   若要启用远程管理运行 Windows Server 2008 的服务器上，请参阅[启用和使用 Windows PowerShell 中远程命令](https://go.microsoft.com/fwlink/p/?LinkId=242565)。

## <a name="tasks-that-you-can-perform-in-server-manager"></a>可在服务器管理器中执行的任务
服务器管理器允许管理员使用单个工具执行下表中的任务，从而使服务器管理更高效。 在 Windows Server 2012 R2 和 Windows Server 2012 中，服务器的标准用户和管理员组的成员都可以执行管理任务在服务器管理器中，但默认情况下，不允许标准用户执行某些任务，如中所示下表。

管理员可以在服务器管理器 cmdlet 模块中，使用两个 Windows PowerShell cmdlet [Enable-servermanagerstandarduserremoting](https://technet.microsoft.com/library/jj205470.aspx)并[Disable-servermanagerstandarduserremoting](https://technet.microsoft.com/library/jj205468.aspx)到进一步控制标准用户对某些附加数据的访问。 **Enable-servermanagerstandarduserremoting** cmdlet 可以提供对事件、 服务、 性能计数器以及角色和功能清单数据的一个或多个非管理员标准用户访问。

> [!IMPORTANT]
> 服务器管理器无法用于管理较新版本的 Windows Server 操作系统。 在 Windows Server 2012 或 Windows 8 上运行服务器管理器不能用于管理运行 Windows Server 2012 R2 的服务器。

|任务描述|管理员（包括内置管理员帐户）|标准服务器用户|
|----------|----------------------------------|-------------|
|将远程服务器添加到池的服务器的服务器管理器可以是用来管理。|是|否|
|创建和编辑自定义服务器，例如，在特定的地理位置或具有特定用途的服务器组。|是|是|
|安装或卸载角色、 角色服务和功能的本地或远程服务器运行 Windows Server 2012 R2 或 Windows Server 2012 上。 有关角色、 角色服务和功能的定义，请参阅[角色、 角色服务和功能](https://go.microsoft.com/fwlink/p/?LinkId=239558)。|是|否|
|查看和更改本地或远程服务器上安装的服务器角色及功能。 **注意：** 在服务器管理器中，角色和功能的数据显示在系统中，也称为系统默认 GUI 语言或在操作系统安装过程中选择的语言的基本语言。|是|标准用户可以查看并管理角色和功能，并执行如查看角色事件之类的任务，但无法添加或删除角色服务。|
|启动管理工具，如 Windows PowerShell 或 mmc 管理单元。可以启动 Windows PowerShell 会话中右键单击服务器针对远程服务器**服务器**磁贴，，然后单击**Windows PowerShell**。 你可以开始从 mmc 管理单元**工具**菜单服务器管理器控制台，然后点将 mmc 指向远程计算机后管理单元中处于打开状态。|是|是|
|通过右键单击“服务器”  磁贴中的服务器，然后单击“管理形式”  ，使用不同的凭据管理远程服务器。 可将“管理形式”  用于常规服务器和“文件和存储服务”管理任务|是|否|
|执行与服务器，如启动或停止服务; 的运行生命周期相关联的管理任务并启动其他工具，可用于配置服务器的网络设置、 用户和组，以及远程桌面连接。|是|标准用户无法启动或停止服务。 他们可以更改本地服务器的名称、 工作组或域成员身份和远程桌面设置，但用户帐户控制提示提供管理员凭据才能完成这些任务。 他们无法更改远程管理设置。|
|执行与服务器上安装的角色运行生命周期关联的管理任务，包括扫描某些角色，看其是否符合最佳做法。|是|标准用户无法运行最佳做法分析器扫描。|
|确定服务器状态，标识关键事件，分析并解决配置问题和故障。|是|是|
|自定义事件、 性能数据、 服务和你想要在服务器管理器仪表板上收到警报有关的最佳做法分析器结果。|是|是|
|重新启动服务器。|是|否|
|刷新有关托管服务器的服务器管理器控制台中显示的数据。|是|否|

> [!NOTE]
> 服务器管理器不能用于将角色和功能添加到运行 Windows Server 2008 R2 的服务器或 Windows Server 2008。

## <a name="start-server-manager"></a>启动服务器管理器
在运行 Windows Server 2016 时登录到服务器管理员组的成员的服务器上的默认情况下，服务器管理器会自动启动。 如果关闭服务器管理器时，对话框，可以通过以下方式之一重新启动它。 本部分还包含用于更改默认行为，以及阻止自动启动服务器管理器的步骤。

#### <a name="to-start-server-manager-from-the-start-screen"></a>从开始屏幕打开服务器管理器

-   在 Windows 上**启动**屏幕上，单击**服务器管理器**磁贴。

#### <a name="to-start-server-manager-from-the-windows-desktop"></a>从 Windows 桌面打开服务器管理器的步骤

-   在 Windows 任务栏上，单击  “服务器管理器”。

#### <a name="to-prevent-server-manager-from-starting-automatically"></a>阻止服务器管理器自动启动的步骤

1.  在服务器管理器控制台中，在**管理**菜单上，单击**服务器管理器属性**。

2.  在“服务器管理器属性”  对话框中，选中“在登录时不自动启动服务器管理器”  的复选框。 单击 **“确定”** 。

3.  或者，可以阻止服务器管理器启用组策略设置自动启动**在不启动服务器管理器会自动登录**。 此策略设置，在本地组策略编辑器控制台中，路径为计算机配置 \ 管理模板 Templates\System\Server 管理器。

## <a name="restart-remote-servers"></a>重新启动远程服务器
您可以重新启动远程服务器**服务器**磁贴，在服务器管理器中的角色或组页面。

> [!IMPORTANT]
> 重新启动远程服务器迫使服务器重新启动，即使用户仍然登录远程服务器，即使数据未保存的程序仍打开，也是如此。 此行为不同于关闭或重新启动本地计算机，在后一种情况下，会提示你保存未保存的程序数据，并确认要强制已登录用户登出。 确认你可以强制其他用户登出远程服务器，并且可以放弃运行在远程服务器上程序的未保存数据。
> 
> 如果在一次自动刷新时关闭并重新启动时，刷新和可管理性状态的可能发生错误的托管服务器，因为服务器管理器无法连接到远程服务器，直到完成托管的服务器出现在服务器管理器正在重新启动。

#### <a name="to-restart-remote-servers-in-server-manager"></a>在服务器管理器中重新启动远程服务器的步骤

1.  服务器管理器中打开角色或服务器组主页。

2.  选择一个或多个远程服务器添加到服务器管理器。 按住 Ctrl  并一次性单击选择多个服务器。 有关如何将服务器添加到服务器管理器服务器池的详细信息，请参阅[将服务器添加到服务器管理器](add-servers-to-server-manager.md)。

3.  右键单击选中的服务器，然后单击“重新启动服务器”  。

## <a name="export-server-manager-settings-to-other-computers"></a>将服务器管理器设置导入其他计算机
在服务器管理器中，你的托管服务器列表更改为服务器管理器控制台中设置，并且存储在以下两个文件中已创建的自定义组。 可以重复使用正在运行相同版本的服务器管理器 （或使用远程服务器管理工具安装 Windows 10） 的其他计算机上的这些设置。 必须在 Windows 的基于客户端的计算机，将服务器管理器设置导出到这些计算机上运行远程服务器管理工具。

-   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

-   %*appdata*%\Local\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

> [!NOTE]
> -   你服务器池中服务器的“管理形式”（或替代）凭据不被存储在漫游配置文件中。 服务器管理器用户必须将凭据添加到想要管理的每台计算机上。
> -   漫游配置文件的网络共享创建在用户登录到网络，然后注销第一次。 **Serverlist.xml** 文件在此时创建。

可以导出服务器管理器设置，使服务器管理器设置可移植，或在以下两种方式之一在其他计算机上使用它们。

-   若要将设置导出到另一台已加入域的计算机，配置服务器管理器用户具有漫游配置文件中 active directory 用户和计算机。 您必须是域管理员才能更改用户属性在 active directory 用户和计算机。

-   若要将设置导出到另一台计算机在工作组中，请将前面提到的两个文件复制到想要使用服务器管理器管理的计算机上的同一位置。

#### <a name="to-export-server-manager-settings-to-other-domain-joined-computers"></a>将服务器管理器设置导入其他加入域的计算机的步骤

1.  在 active directory 用户和计算机中，打开**属性**服务器管理器用户对话框。

2.  上**配置文件**选项卡上，将路径添加到网络共享来存储用户的配置文件。

3.  请执行以下操作之一。

    -   在美国英语 (en-我们) 版本中，将更改为**Serverlist.xml**文件自动保存到配置文件。 继续进行下一步。

    -   在其他版本中，从正在运行服务器管理器对用户的漫游配置文件的一部分的网络共享的计算机上复制以下两个文件。

        -   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

        -   %*localappdata*%\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

4.  单击“确定”  保存你的更改，然后关闭“属性”  对话框。

#### <a name="to-export-server-manager-settings-to-computers-in-workgroups"></a>导出“服务器管理器”设置到工作组计算机的方法

-   想要管理的计算机上的远程服务器，从另一台计算机正在运行服务器管理器中，且具有所需的设置的相同文件覆盖下面两个文件。

    -   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

    -   %*localappdata*%\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config




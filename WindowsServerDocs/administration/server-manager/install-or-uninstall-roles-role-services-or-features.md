---
title: 安装或卸载角色、角色服务或功能
description: 服务器管理器
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04f16d84-45c2-4771-84c1-1cc973d0ee02
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cca2e4c7ba2658c4d85b14ef61ef5f79fbc96345
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383187"
---
# <a name="install-or-uninstall-roles-role-services-or-features"></a>安装或卸载角色、角色服务或功能

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

在 Windows Server 中，用于服务器管理器的服务器管理器控制台和 Windows PowerShell cmdlet 允许将角色和功能安装到本地或远程服务器或脱机虚拟硬盘（Vhd）。 可以在单个 "添加角色和功能向导" 或 "Windows PowerShell" 会话中的单个远程服务器或脱机 VHD 上安装多个角色和功能。  
  
> [!IMPORTANT]  
> 服务器管理器无法用于管理较新版本的 Windows Server 操作系统。 在 Windows Server 2012 R2 或 Windows 8.1 上运行服务器管理器不能用于在运行 Windows Server 2016 的服务器上安装角色、角色服务和功能。  
  
您必须以管理员身份登录服务器才能安装或卸载角色、角色服务和功能。 如果使用在目标服务器上没有管理员权限的帐户登录到本地计算机，请右键单击 "**服务器**" 磁贴中的目标服务器，然后单击 "**管理身份**" 提供具有管理员权限的帐户. 要装载脱机 VHD 的服务器必须添加到服务器管理器，且你必须具有对该服务器的管理员权限。  
  
有关角色、角色服务和功能的详细信息，请参阅[角色、角色服务和功能](https://go.microsoft.com/fwlink/p/?LinkId=239558)。  
  
本主题包含以下部分。  
  
-   [使用 "添加角色和功能" 向导安装角色、角色服务和功能](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard)  
  
-   [使用 Windows PowerShell cmdlet 安装角色、角色服务和功能](#install-roles-role-services-and-features-by-using-windows-powershell-cmdlets)  
  
-   [使用删除角色和功能向导删除角色、角色服务和功能](#remove-roles-role-services-and-features-by-using-the-remove-roles-and-features-wizard)  
  
-   [使用 Windows PowerShell cmdlet 删除角色、角色服务和功能](#remove-roles-role-services-and-features-by-using-windows-powershell-cmdlets)  
  
-   [通过运行 Windows PowerShell 脚本在多个服务器上安装角色和功能](#install-roles-and-features-on-multiple-servers-by-running-a-windows-powershell-script)  
  
-   [安装 .NET Framework 3.5 和其他按需功能](#install-net-framework-35-and-other-features-on-demand)  
  
## <a name="install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard"></a>使用 "添加角色和功能" 向导安装角色、角色服务和功能  
在添加角色和功能向导中的单个会话中，可以在本地服务器、已添加到服务器管理器的远程服务器或脱机 VHD 上安装角色、角色服务和功能。 有关如何向服务器管理器添加要管理的服务器的详细信息，请参阅[将服务器添加到服务器管理器](add-servers-to-server-manager.md)。  
  
> [!NOTE]  
> 如果在 Windows Server 2016 或 Windows 10 上运行服务器管理器，则可以使用 "添加角色和功能向导" 仅在运行 Windows Server 2016 的服务器和脱机 Vhd 上安装角色和功能。  
  
#### <a name="to-install-roles-and-features-by-using-the-add-roles-and-features-wizard"></a>使用 "添加角色和功能向导" 安装角色和功能  
  
1.  如果服务管理器已经打开，则继续执行下一步。 如果服务器管理器尚未打开，请执行以下任一操作打开它。  
  
    -   在 Windows 桌面上，启动服务器管理器，方法是单击 Windows 任务栏中的“服务器管理器”。  
  
    -   在 Windows 的 "**开始**" 屏幕上，单击 "**服务器管理器**" 磁贴。  
  
2.  在 "**管理**" 菜单上，单击 "**添加角色和功能**"。  
  
3.  在 **“开始之前”** 页面上，确定目标服务器和网络环境已为要安装的角色和功能做好准备。 单击“下一步”。  
  
4.  在“选择安装类型”页面，选择“基于角色或基于功能的安装”以在单台服务器上安装角色或功能的所有部分，或选择“远程桌面服务安装”以安装远程桌面服务的基于虚拟机的桌面基础结构或基于会话的桌面基础结构。 “远程桌面服务安装” 选项可根据管理员的需要将远程桌面服务角色的逻辑部分分布于不同的服务器。 单击“下一步”。  
  
5.  在“选择目标服务器”页面上，从服务器池中选择一台服务器，或选择脱机 VHD。 若要将离线的 VHD 选择为你的目标服务器，则先选择安装 VHD 的服务器，然后选择 VHD 文件。 有关如何将服务器添加到服务器池的信息，请参阅[将服务器添加到服务器管理器](add-servers-to-server-manager.md)。 选择目标服务器后，单击“下一步”。  
  
    > [!NOTE]  
    > 若要在脱机 VHD 上安装角色和功能，目标 VHD 必须符合以下要求。  
    >   
    > -   Vhd 必须运行与正在运行的服务器管理器版本相匹配的 Windows Server 版本。 请参阅开始[使用 "添加角色和功能" 向导安装角色、角色服务和功能](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard)的说明。  
    > -   VHD 不能具备多个系统卷或分区。  
    > -   存储 VHD 文件的网络共享文件夹必须向已选择安装 VHD 的服务器的计算机（或本地系统）帐户授予以下访问权限。 仅用户帐户访问权限是不够的。 该共享可向 **“所有人”** 组授予 **“读取”** 和 **“写入”** 权限，以允许访问 VHD，但出于安全原因，不建议这样做。  
    >   
    >     -   “文件共享”对话框上的“读/写”权限。  
    >     -   "**安全**" 选项卡上的 "文件或文件夹**属性**" 对话框中的 "**完全控制**" 权限。  
  
6.  选择角色、选择该角色的角色服务（如果适用），然后单击“下一步” 以选择功能。  
  
    当你继续操作时，"添加角色和功能向导" 会自动通知你在目标服务器上是否发现了可能阻止选定角色或功能安装或正常操作的冲突。 系统还会提示你添加你已选中的角色或功能所需的所有角色、角色服务或功能。  
  
    此外，如果你计划远程管理角色（从另一台服务器，或从运行远程服务器管理工具的基于 Windows 客户端的计算机），则可以选择不在目标服务器上安装角色的管理工具和管理单元。 默认情况下，在 "添加角色和功能向导" 中，选择 "管理工具" 进行安装。  
  
7.  在“确认安装选择” 页上，检查你的角色、功能和服务器选择。 如果准备好安装，单击 **“安装”** 。  
  
    你还可以将选择导出到基于 XML 的配置文件，该文件可用于通过 Windows PowerShell 进行无人参与的安装。 若要导出在 "添加角色和功能向导" 会话中指定的配置，请单击 "**导出配置设置**"，然后将 XML 文件保存到一个方便的位置。  
  
    使用“确认安装选择”页上的“指定备用源路径” 命令 可以为在选定服务器上安装角色和功能时所必需的文件指定备用源路径。 在 Windows Server 2012 和更高版本的 Windows Server 中，按[需功能](https://go.microsoft.com/fwlink/p/?LinkID=241573)可以通过从以远程方式进行管理的服务器中删除角色和功能文件来减少操作系统使用的磁盘空间量。 如果使用 `Uninstall-WindowsFeature -remove` cmdlet 从服务器中删除了角色和功能文件，日后可通过指定备用源路径，或指定存储必需角色和功能文件的共享，来在服务器上安装角色和功能。 源路径或文件共享必须将 "**读取**" 权限授予 " **Everyone** " 组（出于安全原因，不建议这样做），或授予目标服务器的计算机帐户（*域*\\*SERverNAME*$）;授予用户帐户访问权限是不够的。 若要深入了解按需功能，请参阅 [Windows Server 安装选项](https://go.microsoft.com/fwlink/p/?LinkId=241573)。  
  
    在运行的物理服务器上安装角色、角色服务和功能时，可以将 WIM 文件指定为备用功能文件源。 WIM 文件的源路径应采用以下格式，使用**wim**作为前缀，并且功能文件所在的索引作为后缀：**WIM： e： \sources\install.wim： 4**。 但是，不能直接使用 WIM 文件作为将角色、角色服务和功能安装到脱机 VHD 的源;你必须安装脱机 VHD 并指向其源文件的安装路径，或者必须指向包含 WIM 文件内容副本的文件夹的文件夹。  
  
8.  单击 "**安装**" 之后，"**安装进度**" 页将显示安装进度、结果和消息，如警告、失败或你的角色或功能所需的安装后配置步骤。随. 在 windows Server 2012 和更高版本的 Windows Server 中，你可以在安装仍在进行时关闭 "添加角色和功能向导"，并在服务器管理器顶部的 "**通知**" 区域中查看安装结果或其他消息控制台. 单击 "**通知**" 标志图标可查看有关在服务器管理器中执行的安装或其他任务的更多详细信息。  
  
## <a name="install-roles-role-services-and-features-by-using-windows-powershell-cmdlets"></a>使用 Windows PowerShell cmdlet 安装角色、角色服务和功能  
适用于 Windows PowerShell 的服务器管理器部署 cmdlet 的功能与基于 GUI 的 "添加角色和功能向导" 类似，并且删除角色和功能向导，具有重要的差异。 在 Windows PowerShell 中，与 "添加角色和功能向导" 不同，默认情况下不包括角色的管理工具和管理单元。 要在角色安装中包括管理工具，可在 cmdlet 中添加 `IncludeManagementTools` 参数。 如果要在运行 Windows Server 2012 或更高版本的服务器核心安装选项的服务器上安装角色和功能，则可以将角色的管理工具添加到安装，但无法安装基于 GUI 的管理工具和管理单元在运行 Windows Server 服务器核心安装选项的服务器上。 在服务器核心安装选项上，只能安装命令行和 Windows PowerShell 管理工具。  
  
#### <a name="to-install-roles-and-features-by-using-the-install-windowsfeature-cmdlet"></a>若要使用 Install-WindowsFeature cmdlet 安装角色和功能  
  
1. 使用提升的用户权限执行以下操作之一以打开 Windows PowerShell 会话。  
  
   > [!NOTE]  
   > 如果要在远程服务器上安装角色和功能，则无需使用提升的用户权限运行 Windows PowerShell。  
  
   -   在 Windows 桌面上，右键单击任务栏上的 Windows PowerShell，然后单击“以管理员身份运行”。  
  
   -   在 Windows 的 "**开始**" 屏幕上，右键单击 "windows PowerShell" 磁贴，然后单击应用栏上的 "以**管理员身份运行**"。  
  
2. 键入 **Get-WindowsFeature** ，再按 **Enter** ，以查看本地服务器上可用和安装的角色和功能的列表。 如果本地计算机不是服务器，或者如果你需要有关远程服务器的信息，请运行**get-help-computerName @no__t < computerName** *，其中*computerName 表示远程计算机的名称正在运行 Windows Server 2016。 Cmdlet 的结果包含在步骤4中添加到 cmdlet 的角色和功能的命令名称。  
  
   > [!NOTE]  
   > 在 windows powershell 3.0 及更高版本的 Windows PowerShell 中，在运行作为模块一部分的 cmdlet 之前，无需将服务器管理器 cmdlet 模块导入到 Windows PowerShell 会话中。 在你首次运行 cmdlet（模块的一部分）时，模块被自动导入。 此外，与 cmdlet 一起使用的 Windows PowerShell cmdlet 和功能名称都不区分大小写。  
  
3. 键入**Get-help Install**，然后按**enter**查看 @no__t cmdlet 的语法和接受的参数。  
  
4. 键入以下命令，然后按**enter**，其中*feature_name*表示要安装的角色或功能（在步骤2中获取）的命令名称 *，而计算机*名代表要安装的远程计算机。角色和功能。 使用逗号分隔多个 *feature_name* 值。 如果角色或功能安装需要，则 `Restart` 参数会自动重新启动目标服务器。  
  
   ```  
   Install-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
   ```  
  
   若要在脱机 VHD 上安装角色和功能，请同时添加 `computerName` 参数和 `VHD` 参数。 如果未添加 `computerName` 参数，cmdlet 假定装载了本地计算机来访问 VHD。 `computerName` 参数含有安装 VHD 的服务器名称，`VHD` 参数含有 VHD 在指定服务器上的路径。  
  
   > [!NOTE]  
   > 如果正在运行 Windows 客户端操作系统的计算机上运行该 cmdlet，则必须添加 `computerName` 参数。  
   >   
   > 若要在脱机 VHD 上安装角色和功能，目标 VHD 必须符合以下要求。  
   >   
   > -   Vhd 必须运行与正在运行的服务器管理器版本相匹配的 Windows Server 版本。 请参阅开始[使用 "添加角色和功能" 向导安装角色、角色服务和功能](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard)的说明。  
   > -   VHD 不能具备多个系统卷或分区。  
   > -   存储 VHD 文件的网络共享文件夹必须向已选择安装 VHD 的服务器的计算机（或本地系统）帐户授予以下访问权限。 仅用户帐户访问权限是不够的。 该共享可向 **“所有人”** 组授予 **“读取”** 和 **“写入”** 权限，以允许访问 VHD，但出于安全原因，不建议这样做。  
   >   
   >     -   “文件共享”对话框上的“读/写”权限。  
   >     -   "**安全**" 选项卡上的 "文件或文件夹**属性**" 对话框中的 "**完全控制**" 权限。  
  
   ```  
   Install-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
   ```  
  
   **示例：** 以下 cmdlet 将在远程服务器 ContosoDC1 上安装 active directory 域服务角色和组策略管理功能。 已使用 `IncludeManagementTools` 参数添加管理工具和管理单元，并且目标服务器将自动重新启动（如果安装需要重新启动服务器）。  
  
   ```  
   Install-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
   ```  
  
5. 安装完成后，请通过打开服务器管理器中的 "**所有服务器**" 页，选择安装角色和功能的服务器，然后在选定服务器的页面上查看 "**角色和功能**" 磁贴来验证安装。 你还可以运行以所选服务器（Get-help-*computerName < computerName*>）为目标的 `Get-WindowsFeature` cmdlet，以查看服务器上安装的角色和功能的列表。  
  
## <a name="remove-roles-role-services-and-features-by-using-the-remove-roles-and-features-wizard"></a>使用删除角色和功能向导删除角色、角色服务和功能  
您必须以管理员身份登录服务器才能卸载角色、角色服务和功能。 如果使用在卸载目标服务器上没有管理员权限的帐户登录到本地计算机，请右键单击 "**服务器**" 磁贴中的目标服务器，然后单击 "**管理身份**" 以提供具有管理员权限。 要装载脱机 VHD 的服务器必须添加到服务器管理器，且你必须具有对该服务器的管理员权限。  
  
#### <a name="to-remove-roles-and-features-by-using-the-remove-roles-and-features-wizard"></a>使用删除角色和功能向导删除角色和功能  
  
1.  如果服务管理器已经打开，则继续执行下一步。 如果服务器管理器尚未打开，请执行以下任一操作打开它。  
  
    -   在 Windows 桌面上，启动服务器管理器，方法是单击 Windows 任务栏中的“服务器管理器”。  
  
    -   在 Windows 的 **“开始”** 屏幕上，单击 **“服务器管理器”** 磁贴。  
  
2.  在“管理”菜单上，单击“删除角色和功能”。  
  
3.  在“开始之前”页上，验证是否准备好从服务器中删除角色和功能。 单击“下一步”。  
  
4.  在 "**选择目标服务器**" 页上，从服务器池中选择一个服务器，或选择脱机 VHD。 若要选择离线的 VHD，请选择安装 VHD 的服务器，然后选择 VHD 文件。  
  
    > [!NOTE]  
    > 存储 VHD 文件的网络共享文件夹必须向已选择安装 VHD 的服务器的计算机（或本地系统）帐户授予以下访问权限。 仅用户帐户访问权限是不够的。 该共享可向 **“所有人”** 组授予 **“读取”** 和 **“写入”** 权限，以允许访问 VHD，但出于安全原因，不建议这样做。  
    >   
    > -   “文件共享”对话框上的“读/写”权限。  
    > -   文件或文件夹“属性”对话框中“安全”选项卡上的“完全控制”访问权限。  
  
    有关如何将服务器添加到服务器池的信息，请参阅[将服务器添加到服务器管理器](add-servers-to-server-manager.md)。 选择目标服务器后，单击“下一步”。  
  
    > [!NOTE]  
    > 您可以使用 "删除角色和功能向导" 从运行同一版本 Windows Server 的服务器中删除角色和功能，该版本支持您正在使用的服务器管理器版本。 如果在 Windows Server 2012 R2、Windows Server 2012 或 Windows 8 上运行服务器管理器，则不能从运行 Windows Server 2016 的服务器中删除角色、角色服务或功能。 你不能使用 "删除角色和功能向导" 删除运行 Windows Server 2008 或 Windows Server 2008 R2 的服务器上的角色和功能。  
  
5.  选择角色、选择该角色的角色服务（如果适用），然后单击“下一步” 以选择功能。  
  
    当你继续时，"删除角色和功能向导" 会自动提示你删除无法运行的任何角色、角色服务或功能，而不需要你要删除的角色或功能。  
  
    此外，你可以选择删除目标服务器上的角色管理工具和管理单元。 默认情况下，在 "删除角色和功能" 向导中，选择 "管理工具" 以进行删除。 如果你计划使用选中的服务器管理其他远程服务器上的角色，则可以保留管理工具和管理单元。  
  
6.  在 "**确认删除选择**" 页上，检查你的角色、功能和服务器选择。 如果已准备好删除角色或功能，请单击 "**删除**"。  
  
7.  单击 "**删除**" 之后，"**删除进度**" 页会显示删除进度、结果以及消息（如警告、失败或必需的删除后配置步骤，如重新启动目标服务器）。 在 windows Server 2012 和更高版本的 Windows Server 中，你可以在仍在进行删除时关闭 "删除角色和功能向导"，然后在服务器管理器控制台顶部的 "**通知**" 区域中查看删除结果或其他消息。 单击 "**通知**" 标志可查看有关在服务器管理器中执行的删除或其他任务的更多详细信息。  
  
## <a name="remove-roles-role-services-and-features-by-using-windows-powershell-cmdlets"></a>使用 Windows PowerShell cmdlet 删除角色、角色服务和功能  
Windows PowerShell 的服务器管理器部署 cmdlet 的功能与基于 GUI 的 "删除角色和功能向导" 类似。 在 Windows PowerShell 中，与 "删除角色和功能向导" 不同，默认情况下不会删除角色的管理工具和管理单元。 要在角色删除中包括管理工具，可向 cmdlet 中添加 `IncludeManagementTools` 参数。 如果要从运行 Windows Server 2012 或更高版本的 Windows server 的服务器核心安装选项的服务器卸载角色和功能，则此参数会删除指定角色和功能。  
  
#### <a name="to-remove-roles-and-features-by-using-the-uninstall-windowsfeature-cmdlet"></a>若要使用 Uninstall-WindowsFeature cmdlet 删除角色和功能  
  
1. 使用提升的用户权限执行以下操作之一以打开 Windows PowerShell 会话。  
  
   > [!NOTE]  
   > 如果要从远程服务器中卸载角色和功能，则无需使用提升的用户权限运行 Windows PowerShell。  
  
   -   在 Windows 桌面上，右键单击任务栏上的 Windows PowerShell，然后单击“以管理员身份运行”。  
  
   -   在 Windows 的 "**开始**" 屏幕上，右键单击 "windows PowerShell" 磁贴，然后单击应用栏上的 "以**管理员身份运行**"。  
  
2. 键入 **Get-WindowsFeature** ，再按 **Enter** ，以查看本地服务器上可用和安装的角色和功能的列表。 如果本地计算机不是服务器，或者如果你需要有关远程服务器的信息，请运行**get-help-computerName @no__t < computerName** *，其中*computerName 表示远程计算机的名称正在运行 Windows Server 2016。 Cmdlet 的结果包含在步骤4中添加到 cmdlet 的角色和功能的命令名称。  
  
   > [!NOTE]  
   > 在 windows powershell 3.0 及更高版本的 Windows PowerShell 中，在运行作为模块一部分的 cmdlet 之前，无需将服务器管理器 cmdlet 模块导入到 Windows PowerShell 会话中。 在你首次运行 cmdlet（模块的一部分）时，模块被自动导入。 此外，与 cmdlet 一起使用的 Windows PowerShell cmdlet 和功能名称都不区分大小写。  
  
3. 键入**Get-help Uninstall**，然后按**enter**查看 @no__t cmdlet 的语法和接受的参数。  
  
4. 键入以下项，再按 **Enter**，其中 *feature_name* 表示要删除的角色或功能（在步骤 2 中获取）的命令名称，而 *computer_name* 表示要从中删除角色和功能的远程计算机。 使用逗号分隔多个 *feature_name* 值。 `Restart` 参数会根据角色或功能删除的要求自动重新启动目标服务器。  
  
   ```  
   Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
   ```  
  
   若要从脱机 VHD 中删除角色和功能，请同时添加 `computerName` 参数和 `VHD` 参数。 如果未添加 `computerName` 参数，cmdlet 假定装载了本地计算机来访问 VHD。 `computerName` 参数含有安装 VHD 的服务器名称，`VHD` 参数含有 VHD 在指定服务器上的路径。  
  
   > [!NOTE]  
   > 如果正在运行 Windows 客户端操作系统的计算机上运行该 cmdlet，则必须添加 `computerName` 参数。  
   >   
   > 存储 VHD 文件的网络共享文件夹必须向已选择安装 VHD 的服务器的计算机（或本地系统）帐户授予以下访问权限。 仅用户帐户访问权限是不够的。 该共享可向 **“所有人”** 组授予 **“读取”** 和 **“写入”** 权限，以允许访问 VHD，但出于安全原因，不建议这样做。  
   >   
   > -   “文件共享”对话框上的“读/写”权限。  
   > -   "**安全**" 选项卡上的 "文件或文件夹**属性**" 对话框中的 "**完全控制**" 权限。  
  
   ```  
   Uninstall-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
   ```  
  
   **示例：** 以下 cmdlet 将从远程服务器 ContosoDC1 中删除 active directory 域服务角色和组策略管理功能。 还已删除管理工具和管理单元，并且目标服务器将被自动重新启动（如果删除需要重新启动服务器）。  
  
   ```  
   Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
   ```  
  
5. 删除完成后，通过打开服务器管理器中的 "**所有服务器**" 页，选择从中删除角色和功能的服务器，然后在页面上查看 "**角色和功能**" 磁贴来验证角色和功能是否已删除。选定的服务器。 你还可以运行以所选服务器（Get-help-*computerName < computerName*>）为目标的 `Get-WindowsFeature` cmdlet，以查看服务器上安装的角色和功能的列表。  
  
## <a name="install-roles-and-features-on-multiple-servers-by-running-a-windows-powershell-script"></a>通过运行 Windows PowerShell 脚本在多个服务器上安装角色和功能  
尽管你不能使用 "添加角色和功能向导" 在单个向导会话中的多个目标服务器上安装角色、角色服务和功能，但你可以使用 Windows PowerShell 脚本在多个目标上安装角色、角色服务和功能使用服务器管理器管理的服务器。 用于执行批量部署的脚本（在此过程中调用）指向可通过使用 "添加角色和功能向导" 轻松创建的 XML 配置文件，并单击 "**导出配置设置**"向导添加到 "添加角色和功能向导" 的 "**确认安装选择**" 页。  
  
> [!IMPORTANT]  
> 脚本中指定的所有目标服务器都必须运行与你在本地计算机上运行的服务器管理器版本相匹配的 Windows Server 版本。 例如，如果在 Windows 10 上运行服务器管理器，则可以在运行 Windows Server 2016 的服务器上安装角色、角色服务和功能。 如果将基于 GUI 的管理工具添加到安装中，则安装进程会自动将运行 Windows Server 服务器核心安装选项的目标服务器转换为完全安装选项（具有完整 GUI 的服务器，也称为作为运行服务器图形 Shell）。  
>   
> 本部分中提供的脚本是一个示例，说明如何使用 `Install-WindowsFeature` cmdlet 和 Windows PowerShell 脚本来执行批处理部署。 有其他可能的脚本和方法可执行对多个服务器的批量部署。 若要搜索或提供用于部署角色和功能的其他脚本，请搜索 [脚本中心存储库](https://gallery.technet.microsoft.com/ScriptCenter)。  
  
#### <a name="to-install-roles-and-features-on-multiple-servers"></a>在多个服务器上安装角色和功能  
  
1.  如果尚未这样做，请创建一个 XML 配置文件，其中包含要安装在多个服务器上的角色、角色服务和功能。 你可以通过运行 "添加角色和功能向导"，选择所需的角色、角色服务和功能，并在通过向导前进到 "确认" 后单击 "**导出配置设置**" 来创建此配置文件。 **"安装选择**" 页。 将配置文件保存到某个方便的位置。 如果只是要创建配置文件，则无需单击 "**安装**" 或完成向导。  
  
2.  使用提升的用户权限执行以下操作之一以打开 Windows PowerShell 会话。  
  
    -   在 Windows 桌面上，右键单击任务栏上的 Windows PowerShell，然后单击“以管理员身份运行”。  
  
    -   在 Windows 的 "**开始**" 屏幕上，右键单击 "windows PowerShell" 磁贴，然后单击应用栏上的 "以**管理员身份运行**"。  
  
3.  将以下脚本复制并粘贴到 Windows PowerShell 会话中。  
  
    ```  
    function Invoke-WindowsFeatureBatchDeployment {  
        param (  
            [parameter(mandatory)]  
            [string[]] $computerNames,  
            [parameter(mandatory)]  
            [string] $ConfigurationFilepath  
        )  
  
        # Deploy the features on multiple computers simultaneously.  
        $jobs = @()  
        foreach($computerName in $computerNames) {  
            $jobs += start-Job -Command {  
                Install-WindowsFeature -ConfigurationFilepath $using:ConfigurationFilepath -computerName $using:computerName -Restart  
            }   
        }  
  
        Receive-Job -Job $jobs -Wait | select-Object Success, RestartNeeded, exitCode, FeatureResult  
    }  
    ```  
  
    如果所选角色和功能需要，则会自动重新启动目标服务器。  
  
4.  通过执行以下操作运行该函数。  
  
    1.  创建在其中存储目标计算机名称的变量，用逗号分隔。 在以下示例中，变量 `$ServerNames` 存储目标服务器名称 *Contoso_01* 和 *Contoso_02*。 按 Enter。  
  
        ```  
        # Sample Invocation  
        $ServerNames = 'Contoso_01','Contoso_02'  
        Invoke-WindowsFeatureBatchDeployment -computerNames $ServerNames -ConfigurationFilepath C:\Users\sampleuser\Desktop\DeploymentConfigTemplate.xml  
        ```  
  
    2.  若要运行此函数，请键入以下内容，然后按 Enter，其中 `$ServerNames` 为在上述步骤中所创建变量的示例， *C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml* 为在步骤 1 中所创建配置文件路径的示例。  
  
        **WindowsFeatureBatchDeployment-computerNames $ServerNames-ConfigurationFilepath C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml**  
  
5.  安装完成后，请通过打开服务器管理器中的 "**所有服务器**" 页，选择安装角色和功能的服务器，然后在选定服务器的页面上查看 "**角色和功能**" 磁贴来验证安装。 你还可以运行面向特定服务器（`Get-WindowsFeature -computerName` @ no__t *-2 用户*类型 >）的 `Get-WindowsFeature` cmdlet，以查看服务器上安装的角色和功能的列表。  
  
## <a name="install-net-framework-35-and-other-features-on-demand"></a>安装 .NET Framework 3.5 和其他按需功能  
从 Windows Server 2012 和 Windows 8 开始，默认情况下，在本地计算机上不能使用 .NET Framework 3.5 的功能文件（包括 .NET Framework 2.0 和 .NET Framework 3.0）。 文件已删除。 已在 "按需功能" 配置中删除的功能的文件以及 .NET Framework 3.5 的功能文件均通过 Windows 更新提供。 默认情况下，如果功能文件在运行 Windows Server 2012 或更高版本的目标服务器上不可用，则安装过程会通过连接到 Windows 更新搜索缺少的文件。 无论是使用 "添加角色和功能向导" GUI 还是命令行进行安装，都可以在安装过程中配置组策略设置或指定备用源路径来覆盖默认行为。  
  
通过执行以下一项操作，可以安装 .NET Framework 3.5。  
  
-   使用 [通过运行 Install-WindowsFeature cmdlet 来安装 .NET Framework 3.5](#to-install-net-framework-35-by-running-the-install-windowsfeature-cmdlet) 来添加 `Source` 参数，并指定从中获取 .NET Framework 3.5 功能文件的源。 如果未添加 `Source` 参数，安装进程先确定组策略设置是否指定了功能文件路径，并在找到此类路径后，使用 Windows 更新搜索缺少的功能文件。  
  
-   使用[通过 "添加角色和功能向导" 来安装 .NET Framework 3.5](#to-install-net-framework-35-by-using-the-add-roles-and-features-wizard) ，以在 "添加角色和功能向导" 的 "**确认安装选项**" 页上指定备用源文件位置。  
  
-   使用 [使用 DISM 安装 .NET Framework 3.5](#to-install-net-framework-35-by-using-dism) 来从 Windows 更新中默认获取文件，或通过指定到安装媒体的源路径来实现获取。  
  
如果在本地计算机上找不到功能文件，则使用[在组策略中为功能文件配置备用来源](#configure-alternate-sources-for-feature-files-in-group-policy) 来获取 .NET Framework 3.5 或其他功能。  
  
> [!IMPORTANT]  
> 从远程来源安装功能文件时，源路径或文件共享必须将“读取” 权限授予“任何人” 组（出于安全原因，不建议这样做），或授予目标服务器的计算机（本地系统）帐户；授予用户帐户访问权限并不足够。  
>   
> 即使工作组服务器的计算机帐户具有外部共享“读取” 权限，位于工作组中的服务器也无法访问外部文件共享。 为工作组服务器服务的备用源位置包括安装媒体、Windows 更新和存储在本地工作组服务器上的 VHD 或 WIM 文件。  
>   
> 在运行的物理服务器上安装角色、角色服务和功能时，可以将 WIM 文件指定为备用功能文件源。 WIM 文件的源路径应采用以下格式，使用**wim**作为前缀，并且功能文件所在的索引作为后缀：**WIM： e： \sources\install.wim： 4**。 但是，不能直接使用 WIM 文件作为将角色、角色服务和功能安装到脱机 VHD 的源;你必须安装脱机 VHD 并指向其源文件的安装路径，或者必须指向包含 WIM 文件内容副本的文件夹的文件夹。  
  
### <a name="to-install-net-framework-35-by-running-the-install-windowsfeature-cmdlet"></a>通过运行 Install-WindowsFeature cmdlet 来安装 .NET Framework 3.5  
  
1.  使用提升的用户权限执行以下操作之一以打开 Windows PowerShell 会话。  
  
    > [!NOTE]  
    > 如果要从远程服务器中安装角色和功能，则无需使用提升的用户权限运行 Windows PowerShell。  
  
    -   在 Windows 桌面上，右键单击任务栏上的 Windows PowerShell，然后单击“以管理员身份运行”。  
  
    -   在 Windows 的 "**开始**" 屏幕上，右键单击 "windows PowerShell" 磁贴，然后单击应用栏上的 "以**管理员身份运行**"。  
  
    -   在运行 Windows Server 2012 R2 或 Windows Server 2012 的服务器核心安装选项的服务器上，在命令提示符下键入**PowerShell** ，然后按**enter**。  
  
2.  键入以下命令，然后按**enter**。 在以下示例中，源文件位于驱动器 D 上的安装介质中的并排存储区（简称为 **SxS**）中。  
  
    ```  
    Install-WindowsFeature NET-Framework-Core -Source D:\Sources\SxS  
    ```  
  
    如果想要命令使用 Windows 更新作为缺少的功能文件的源，或已使用组策略配置默认源，则无需添加 `Source` 参数，除非要指定不同源。  
  
### <a name="to-install-net-framework-35-by-using-the-add-roles-and-features-wizard"></a>使用 "添加角色和功能向导" 安装 .NET Framework 3。5  
  
1. 在服务器管理器中的 "**管理**" 菜单上，单击 "**添加角色和功能**"。  
  
2. 选择运行 Windows Server 2016 的目标服务器。  
  
3. 在 "添加角色和功能向导" 的 "**选择功能**" 页上，选择 **.NET Framework 3.5**。  
  
4. 如果组策略设置允许本地计算机这样做，安装进程将尝试使用 Windows 更新获取缺少的功能文件。 单击“安装”；你无需继续执行下一步。  
  
   如果组策略设置不允许这样做，或你想要使用 .NET Framework 3.5 功能文件的其他源，请在向导的 "**确认安装选择**" 页上，单击 "**指定备用源路径**"。  
  
5. 提供并排存储区（称为 **SxS**）在安装介质的路径或 WIM 文件的路径。 在以下示例中，安装介质位于驱动器 D。  
  
   **D:\Sources\SxS @ no__t-1**  
  
   若要指定 WIM 文件，请添加 **WIM:** 前缀，并添加要在 WIM 文件中用作后缀的映像索引，如以下示例所示。  
  
   **WIM： \\ @ no__t-2**<em>server_name</em> **\share\install.wim： 3**  
  
6. 单击“确定”，再单击“安装”。  
  
### <a name="to-install-net-framework-35-by-using-dism"></a>使用 DISM 安装 .NET Framework 3.5  
  
1.  使用提升的用户权限执行以下操作之一以打开 Windows PowerShell 会话。  
  
    > [!NOTE]  
    > 如果要从远程服务器中安装角色和功能，则无需使用提升的用户权限运行 Windows PowerShell。  
  
    -   在 Windows 桌面上，右键单击任务栏上的 Windows PowerShell，然后单击“以管理员身份运行”。  
  
    -   在 Windows 的 "**开始**" 屏幕上，右键单击 "windows PowerShell" 磁贴，然后单击应用栏上的 "以**管理员身份运行**"。  
  
    -   在运行服务器核心安装选项的服务器上，在命令提示符下键入**PowerShell** ，然后按**enter**。  
  
2.  运行以下 DISM 命令之一。  
  
    -   如果计算机有权访问 Windows 更新，或已在组策略中配置了默认源文件位置，请运行以下命令。  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All  
        ```  
  
    -   如果计算机有权访问安装介质，请运行如下所示的命令。 在以下示例中，操作系统安装介质位于驱动器 D 上。@No__t 参数可防止命令尝试联系 Windows 更新或运行 WSUS 的服务器。  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All /LimitAccess /Source:d:\sources\sxs  
        ```  
  
    > [!NOTE]  
    > DISM 命令区分大小写。  
  
### <a name="configure-alternate-sources-for-feature-files-in-group-policy"></a>在组策略中为功能文件配置备用来源  
本节中介绍的组策略设置指定 .NET Framework 3.5 文件及已在按需功能配置中删除的其他功能文件的授权源位置。 策略设置 "**指定可选组件安装和组件修复的设置**" 位于组策略管理控制台或本地组策略中的**计算机配置 \ 管理模板 \ 模板 \ 系统**文件夹中。editor.  
  
> [!NOTE]  
> 你必须是管理员组成员，才能在本地计算机上更改组策略设置。 如果你想要管理的计算机的组策略设置在域级别控制，则你必须是域管理员组成员，才能更改组策略设置。  
  
##### <a name="to-configure-a-default-alternate-source-path-in-group-policy"></a>在组策略中配置默认备用源路径  
  
1. 在 "本地组策略编辑器" 或 "组策略管理控制台中，打开以下策略设置。  
  
   **用于可选组件安装和组件修复的计算机配置 \ \ 设置**  
  
2. 启用**Sselect 以**启用策略设置（如果尚未启用）。  
  
3. 在“选项”区域的“备用源路径”文本框中，指定共享文件夹或 WIM 文件的完全限定路径。 若要将 WIM 文件指定为备用源文件位置，请将前缀 **WIM:** 添加到路径，并添加要在 WIM 文件中用作后缀的映像索引。 以下是你可以指定的值示例：  
  
   - 共享文件夹的路径： **\\ @ no__t-2**<em>server_name</em> **\share @ no__t-5**<em>folder_name</em>  
  
   - WIM 文件的路径，其中**3**表示在其中找到功能文件的映像的索引：**WIM： \\ @ no__t-2**<em>server_name</em> **\share\install.wim： 3**  
  
4. 如果你不希望此策略设置控制的计算机在 Windows 更新中搜索缺少的功能文件，请选择 "**从不尝试从 Windows 更新下载有效负载**。  
  
5. 如果此策略设置控制的计算机通常通过 WSUS 接收更新，但你首选通过 Windows 更新而非 WSUS 查找缺少的功能文件，请选择“联系 Windows 更新直接下载修复内容，而非 Windows Server 更新服务 (WSUS)”。  
  
6. 更改此策略设置后，单击“确定”，再关闭组策略编辑器。  
  
## <a name="see-also"></a>请参阅  
[Windows Server 安装选项](https://go.microsoft.com/fwlink/p/?LinkId=241573)  
[Microsoft .NET Framework 3.5 部署注意事项](https://go.microsoft.com/fwlink/p/?LinkId=248869)  
[如何启用或禁用 Windows 功能](https://go.microsoft.com/fwlink/p/?LinkId=246552)  
  



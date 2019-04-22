---
title: 安装或卸载角色、角色服务或功能
description: 服务器管理器
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 32d356b3ae70b7b15f23a40247e73b4b8f61c3db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822368"
---
# <a name="install-or-uninstall-roles-role-services-or-features"></a>安装或卸载角色、角色服务或功能

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在 Windows Server 的服务器管理器控制台和 Windows PowerShell cmdlet 为服务器管理器允许向本地或远程服务器或脱机虚拟硬盘 (Vhd) 角色和功能的安装。 可以在一台远程服务器上安装多个角色和功能或脱机 VHD 在单个添加角色和功能向导或 Windows PowerShell 会话。  
  
> [!IMPORTANT]  
> 服务器管理器无法用于管理较新版本的 Windows Server 操作系统。 在 Windows Server 2012 R2 或 Windows 8.1 上运行服务器管理器不能用于在运行 Windows Server 2016 的服务器上安装角色、 角色服务和功能。  
  
您必须将登录到服务器以管理员身份才能安装或卸载角色、 角色服务和功能。 如果登录到本地计算机不在目标服务器上具有管理员权限的帐户，右键单击中的目标服务器**服务器**磁贴，然后依次**管理方式**到提供具有管理员权限的帐户。 要装载脱机 VHD 的服务器必须添加到服务器管理器，且你必须具有对该服务器的管理员权限。  
  
有关什么是角色、 角色服务和功能的详细信息，请参阅[角色、 角色服务和功能](https://go.microsoft.com/fwlink/p/?LinkId=239558)。  
  
本主题包含以下部分。  
  
-   [使用添加角色和功能向导安装角色、 角色服务和功能](#BKMK_installarfw)  
  
-   [使用 Windows PowerShell cmdlet 安装角色、角色服务和功能](#BKMK_installwps)  
  
-   [删除角色、 角色服务和功能使用删除角色和功能向导](#BKMK_removerrfw)  
  
-   [使用 Windows PowerShell cmdlet 删除角色、角色服务和功能](#BKMK_removewps)  
  
-   [通过运行 Windows PowerShell 脚本在多个服务器上安装角色和功能](#BKMK_batch)  
  
-   [安装 .NET Framework 3.5 和其他按需功能](#BKMK_FoD)  
  
## <a name="BKMK_installarfw"></a>使用添加角色和功能向导安装角色、 角色服务和功能  
在单个会话中的添加角色和功能向导中，你可以安装角色、 角色服务和功能的本地服务器上，已添加到服务器管理器或离线的 VHD 的远程服务器。 有关如何将服务器添加到服务器管理器管理的详细信息，请参阅[将服务器添加到服务器管理器](add-servers-to-server-manager.md)。  
  
> [!NOTE]  
> 如果 Windows Server 2016 或 Windows 10 上运行服务器管理器，可以使用添加角色和功能向导仅在服务器和运行 Windows Server 2016 的脱机 Vhd 上安装角色和功能。  
  
#### <a name="to-install-roles-and-features-by-using-the-add-roles-and-features-wizard"></a>若要通过使用添加角色和功能向导安装角色和功能  
  
1.  如果服务管理器已经打开，则继续执行下一步。 如果服务器管理器尚未打开，请执行以下任一操作打开它。  
  
    -   在 Windows 桌面上，启动服务器管理器，方法是单击 Windows 任务栏中的“服务器管理器”。  
  
    -   在 Windows 上**启动**屏幕上，单击**服务器管理器**磁贴。  
  
2.  上**管理**菜单上，单击**添加角色和功能**。  
  
3.  在 **“开始之前”** 页面上，确定目标服务器和网络环境已为要安装的角色和功能做好准备。 单击“下一步” 。  
  
4.  在“选择安装类型”页面，选择“基于角色或基于功能的安装”以在单台服务器上安装角色或功能的所有部分，或选择“远程桌面服务安装”以安装远程桌面服务的基于虚拟机的桌面基础结构或基于会话的桌面基础结构。 “远程桌面服务安装”  选项可根据管理员的需要将远程桌面服务角色的逻辑部分分布于不同的服务器。 单击“下一步” 。  
  
5.  在“选择目标服务器”页面上，从服务器池中选择一台服务器，或选择脱机 VHD。 若要将离线的 VHD 选择为你的目标服务器，则先选择安装 VHD 的服务器，然后选择 VHD 文件。 有关如何将服务器添加到服务器池的信息，请参阅[将服务器添加到服务器管理器](add-servers-to-server-manager.md)。 选择目标服务器后，单击“下一步” 。  
  
    > [!NOTE]  
    > 若要在脱机 VHD 上安装角色和功能，目标 VHD 必须符合以下要求。  
    >   
    > -   Vhd 必须运行与服务器管理器的正在运行的版本匹配的 Windows Server 的版本。 请参阅注释的开头[通过使用添加角色和功能向导安装角色、 角色服务和功能](#BKMK_installarfw)。  
    > -   VHD 不能具备多个系统卷或分区。  
    > -   存储 VHD 文件的网络共享文件夹必须向已选择安装 VHD 的服务器的计算机（或本地系统）帐户授予以下访问权限。 仅用户帐户访问权限是不够的。 该共享可向 **“所有人”** 组授予 **“读取”** 和 **“写入”** 权限，以允许访问 VHD，但出于安全原因，不建议这样做。  
    >   
    >     -   “文件共享”对话框上的“读/写”权限。  
    >     -   **完全控制**上访问**安全**选项卡、 文件或文件夹**属性**对话框。  
  
6.  选择角色、选择该角色的角色服务（如果适用），然后单击“下一步”  以选择功能。  
  
    继续操作时，添加角色和功能向导会自动通知你是否可以阻止选定的角色或功能进行安装或正常操作的目标服务器上发现了冲突。 系统还会提示你添加你已选中的角色或功能所需的所有角色、角色服务或功能。  
  
    此外，如果你计划远程管理角色（从另一台服务器，或从运行远程服务器管理工具的基于 Windows 客户端的计算机），则可以选择不在目标服务器上安装角色的管理工具和管理单元。 默认情况下，在添加角色和功能向导中选定要安装的管理工具。  
  
7.  在“确认安装选择”  页上，检查你的角色、功能和服务器选择。 如果准备好安装，单击 **“安装”**。  
  
    您还可以导出到可用于使用 Windows PowerShell 的无人参与安装的基于 XML 的配置文件所做选择。 若要导出在此指定的配置，将添加角色和功能向导会话中，单击**导出配置设置**，然后将 XML 文件保存到一个方便的位置。  
  
    使用“确认安装选择”页上的“指定备用源路径”  命令  可以为在选定服务器上安装角色和功能时所必需的文件指定备用源路径。 在 Windows Server 2012 和更高版本的 Windows Server[按需功能](https://go.microsoft.com/fwlink/p/?LinkID=241573)可用于减少的量使用的磁盘空间由操作系统，从以独占方式管理的服务器中删除角色和功能文件远程。 如果使用 `Uninstall-WindowsFeature -remove` cmdlet 从服务器中删除了角色和功能文件，日后可通过指定备用源路径，或指定存储必需角色和功能文件的共享，来在服务器上安装角色和功能。 源路径或文件共享必须授予**读**权限授予**每个人都**组 （不建议这样做出于安全原因），或计算机帐户 (*域*\\ *SERverNAME*$) 的目标服务器; 授予用户帐户访问权限并不足够。 若要深入了解按需功能，请参阅 [Windows Server 安装选项](https://go.microsoft.com/fwlink/p/?LinkId=241573)。  
  
    为正在运行的物理服务器上安装角色、 角色服务和功能时，可以指定 WIM 文件作为替换功能文件源。 WIM 文件的源路径应采用以下格式，与**WIM**作为前缀，功能文件所在作为后缀索引：**WIM:e:\sources\install.wim:4**。 但是，您不能使用 WIM 文件作为源直接安装到脱机 VHD; 的角色、 角色服务和功能的必须安装脱机 VHD 并指向其装入源文件路径，或必须指向包含 WIM 文件的内容的副本的文件夹。  
  
8.  单击后**安装**，则**安装进度**页显示安装进度、 结果和消息，如警告、 故障或安装后配置步骤所需的角色或功能的安装。 在 Windows Server 2012 和更高版本的 Windows Server 中，你可以关闭添加角色和功能向导安装仍在进行，并查看安装结果或其他消息时**通知**顶部区域在服务器管理器控制台中。 单击**通知**标志图标可查看有关安装或执行服务器管理器中其他任务的更多详细信息。  
  
## <a name="BKMK_installwps"></a>使用 Windows PowerShell cmdlet 安装角色、 角色服务和功能  
服务器管理器部署 cmdlet 类似于基于 GUI 的 Windows PowerShell 函数的添加角色和功能向导，并删除角色和功能向导中，有一个重要区别。 在 Windows PowerShell 中，不同于在添加角色和功能向导中管理工具和管理角色的管理单元不属于默认情况下。 要在角色安装中包括管理工具，可在 cmdlet 中添加 `IncludeManagementTools` 参数。 如果正在运行 Windows Server 2012 或更高版本的服务器核心安装选项的服务器上安装角色和功能，可以将角色的管理工具添加到安装，但不能安装基于 GUI 的管理工具和管理单元在服务器上运行 Windows Server 的服务器核心安装的选项。 只有命令行，并且可以在服务器核心安装选项上安装 Windows PowerShell 管理工具。  
  
#### <a name="to-install-roles-and-features-by-using-the-install-windowsfeature-cmdlet"></a>若要使用 Install-WindowsFeature cmdlet 安装角色和功能  
  
1.  使用提升的用户权限执行以下操作之一以打开 Windows PowerShell 会话。  
  
    > [!NOTE]  
    > 如果在远程服务器上安装角色和功能，您不需要使用提升的用户权限运行 Windows PowerShell。  
  
    -   在 Windows 桌面上，右键单击任务栏上的 Windows PowerShell，然后单击“以管理员身份运行”。  
  
    -   在 Windows 上**启动**屏幕上，右键单击适用于 Windows PowerShell 磁贴，然后在应用栏上，单击**以管理员身份运行**。  
  
2.  键入 **Get-WindowsFeature** ，再按 **Enter** ，以查看本地服务器上可用和安装的角色和功能的列表。 如果本地计算机不是服务器，或者你想远程服务器的信息，运行**Get-windowsfeature-computerName <***computer_name***>**，在其中*computer_name*表示正在运行 Windows Server 2016 的远程计算机的名称。 Cmdlet 的结果包含角色和功能添加到你在步骤 4 中的 cmdlet 的命令的名称。  
  
    > [!NOTE]  
    > 在 Windows PowerShell 3.0 和更高版本的 Windows PowerShell 中，没有无需服务器管理器 cmdlet 模块导入到 Windows PowerShell 会话，然后再运行模块的一部分的 cmdlet。 在你首次运行 cmdlet（模块的一部分）时，模块被自动导入。 此外，Windows PowerShell cmdlet 或 cmdlet 中使用的功能名称都不是区分大小写。  
  
3.  类型**get-help Install-windowsfeature**，然后按**Enter**若要查看的语法和接受的参数`Install-WindowsFeature`cmdlet。  
  
4.  键入以下内容，，然后按**Enter**，其中*feature_name*表示某个角色或功能，你想要安装 （在步骤 2 中获得），该功能的命令名称和*computer_name*表示你想要安装角色和功能的远程计算机。 使用逗号分隔多个 *feature_name* 值。 如果角色或功能安装需要，则 `Restart` 参数会自动重新启动目标服务器。  
  
    ```  
    Install-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
    ```  
  
    若要在脱机 VHD 上安装角色和功能，请同时添加 `computerName` 参数和 `VHD` 参数。 如果未添加 `computerName` 参数，cmdlet 假定装载了本地计算机来访问 VHD。 `computerName` 参数含有安装 VHD 的服务器名称，`VHD` 参数含有 VHD 在指定服务器上的路径。  
  
    > [!NOTE]  
    > 必须添加`computerName`参数运行 cmdlet 的计算机上，如果正在运行 Windows 客户端操作系统。  
    >   
    > 若要在脱机 VHD 上安装角色和功能，目标 VHD 必须符合以下要求。  
    >   
    > -   Vhd 必须运行与服务器管理器的正在运行的版本匹配的 Windows Server 的版本。 请参阅注释的开头[通过使用添加角色和功能向导安装角色、 角色服务和功能](#BKMK_installarfw)。  
    > -   VHD 不能具备多个系统卷或分区。  
    > -   存储 VHD 文件的网络共享文件夹必须向已选择安装 VHD 的服务器的计算机（或本地系统）帐户授予以下访问权限。 仅用户帐户访问权限是不够的。 该共享可向 **“所有人”** 组授予 **“读取”** 和 **“写入”** 权限，以允许访问 VHD，但出于安全原因，不建议这样做。  
    >   
    >     -   “文件共享”对话框上的“读/写”权限。  
    >     -   **完全控制**上访问**安全**选项卡、 文件或文件夹**属性**对话框。  
  
    ```  
    Install-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
    ```  
  
    **示例：** 以下 cmdlet 在远程服务器 ContosoDC1 上安装 active directory 域服务角色和组策略管理功能。 已使用 `IncludeManagementTools` 参数添加管理工具和管理单元，并且目标服务器将自动重新启动（如果安装需要重新启动服务器）。  
  
    ```  
    Install-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
    ```  
  
5.  完成安装后，验证安装，方法是打开**的所有服务器**页在服务器管理器中，选择在其上安装了角色和功能，并查看**角色和功能**所选服务器的页面上磁贴。 您还可以运行`Get-WindowsFeature`将所选服务器作为目标的 cmdlet (Get-windowsfeature-computerName <*computer_name*>) 若要查看的服务器上安装角色和功能的列表。  
  
## <a name="BKMK_removerrfw"></a>删除角色、 角色服务和功能使用删除角色和功能向导  
您必须将登录到服务器管理员在卸载角色、 角色服务和功能。 如果你登录到你在卸载目标服务器上没有管理员权限的帐户在本地计算机，右键单击中的目标服务器**服务器**磁贴，然后依次**管理方式**提供具有管理员权限的帐户。 要装载脱机 VHD 的服务器必须添加到服务器管理器，且你必须具有对该服务器的管理员权限。  
  
#### <a name="to-remove-roles-and-features-by-using-the-remove-roles-and-features-wizard"></a>若要删除角色和功能使用删除角色和功能向导  
  
1.  如果服务管理器已经打开，则继续执行下一步。 如果服务器管理器尚未打开，请执行以下任一操作打开它。  
  
    -   在 Windows 桌面上，启动服务器管理器，方法是单击 Windows 任务栏中的“服务器管理器”。  
  
    -   在 Windows 的 **“开始”** 屏幕上，单击 **“服务器管理器”** 磁贴。  
  
2.  在“管理”菜单上，单击“删除角色和功能”。  
  
3.  在“开始之前”页上，验证是否准备好从服务器中删除角色和功能。 单击“下一步” 。  
  
4.  上**选择目标服务器**页上，从服务器池中选择服务器或选择脱机 VHD。 若要选择离线的 VHD，请选择安装 VHD 的服务器，然后选择 VHD 文件。  
  
    > [!NOTE]  
    > 存储 VHD 文件的网络共享文件夹必须向已选择安装 VHD 的服务器的计算机（或本地系统）帐户授予以下访问权限。 仅用户帐户访问权限是不够的。 该共享可向 **“所有人”** 组授予 **“读取”** 和 **“写入”** 权限，以允许访问 VHD，但出于安全原因，不建议这样做。  
    >   
    > -   “文件共享”对话框上的“读/写”权限。  
    > -   文件或文件夹“属性”对话框中“安全”选项卡上的“完全控制”访问权限。  
  
    有关如何将服务器添加到服务器池的信息，请参阅[将服务器添加到服务器管理器](add-servers-to-server-manager.md)。 选择目标服务器后，单击“下一步” 。  
  
    > [!NOTE]  
    > 可以使用删除角色和功能向导删除角色和功能从正在运行相同版本的 Windows Server 的服务器支持版本的服务器管理器正在使用。 您无法从运行 Windows Server 2016 的服务器删除角色、 角色服务或功能如果您在 Windows Server 2012 R2、 Windows Server 2012 或 Windows 8 上运行服务器管理器。 不能使用删除角色和功能向导，以从运行 Windows Server 2008 的服务器或 Windows Server 2008 R2 中删除角色和功能。  
  
5.  选择角色、选择该角色的角色服务（如果适用），然后单击“下一步”  以选择功能。  
  
    在继续操作，删除角色和功能向导会自动提示您删除任何角色，将删除角色服务或功能或功能，您就不能运行的角色。  
  
    此外，您可以选择要删除管理工具和目标服务器上的角色的管理单元。 默认情况下，在删除角色和功能向导中，通过删除选定的管理工具。 如果你计划使用选中的服务器管理其他远程服务器上的角色，则可以保留管理工具和管理单元。  
  
6.  上**确认删除选择**页上，查看你的角色、 功能和服务器选择。 如果你已准备好删除角色或功能，请单击**删除**。  
  
7.  单击后**删除**，则**删除进度**页会显示删除进度、 结果和消息，如警告、 失败或所必需的如删除后配置步骤的重新启动目标服务器。 在 Windows Server 2012 和更高版本的 Windows Server 中，你可以关闭的删除角色和功能向导仍在删除时进度，并查看删除结果或其他消息**通知**的顶部区域服务器管理器控制台。 单击**通知**标志可查看有关删除或执行服务器管理器中其他任务的更多详细信息。  
  
## <a name="BKMK_removewps"></a>使用 Windows PowerShell cmdlet 删除角色、 角色服务和功能  
Windows PowerShell 函数类似于基于 GUI 的服务器管理器部署 cmdlet 删除角色和功能向导中，有一个重要区别。 在 Windows PowerShell 中，不同于在删除角色和功能向导、 管理工具和管理单元的角色不会删除默认情况下。 要在角色删除中包括管理工具，可向 cmdlet 中添加 `IncludeManagementTools` 参数。 如果要从服务器卸载角色和功能运行服务器核心安装选项的 Windows Server 2012 或更高版本的 Windows Server、 此参数会删除命令行和指定的 Windows PowerShell 管理工具角色和功能。  
  
#### <a name="to-remove-roles-and-features-by-using-the-uninstall-windowsfeature-cmdlet"></a>若要使用 Uninstall-WindowsFeature cmdlet 删除角色和功能  
  
1.  使用提升的用户权限执行以下操作之一以打开 Windows PowerShell 会话。  
  
    > [!NOTE]  
    > 如果要从远程服务器中卸载角色和功能，您不需要使用提升的用户权限运行 Windows PowerShell。  
  
    -   在 Windows 桌面上，右键单击任务栏上的 Windows PowerShell，然后单击“以管理员身份运行”。  
  
    -   在 Windows 上**启动**屏幕上，右键单击 Windows PowerShell 磁贴，并在应用栏上，单击**以管理员身份运行**。  
  
2.  键入 **Get-WindowsFeature** ，再按 **Enter** ，以查看本地服务器上可用和安装的角色和功能的列表。 如果本地计算机不是服务器，或者你想远程服务器的信息，运行**Get-windowsfeature-computerName <***computer_name***>**，在其中*computer_name*表示正在运行 Windows Server 2016 的远程计算机的名称。 Cmdlet 的结果包含角色和功能添加到你在步骤 4 中的 cmdlet 的命令的名称。  
  
    > [!NOTE]  
    > 在 Windows PowerShell 3.0 和更高版本的 Windows PowerShell 中，没有无需服务器管理器 cmdlet 模块导入到 Windows PowerShell 会话，然后再运行模块的一部分的 cmdlet。 在你首次运行 cmdlet（模块的一部分）时，模块被自动导入。 此外，Windows PowerShell cmdlet 或 cmdlet 中使用的功能名称都不是区分大小写。  
  
3.  类型**get-help Uninstall-windowsfeature**，然后按**Enter**若要查看的语法和接受的参数`Uninstall-WindowsFeature`cmdlet。  
  
4.  键入以下项，再按 **Enter**，其中 *feature_name* 表示要删除的角色或功能（在步骤 2 中获取）的命令名称，而 *computer_name* 表示要从中删除角色和功能的远程计算机。 使用逗号分隔多个 *feature_name* 值。 `Restart` 参数会根据角色或功能删除的要求自动重新启动目标服务器。  
  
    ```  
    Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
    ```  
  
    若要从脱机 VHD 中删除角色和功能，请同时添加 `computerName` 参数和 `VHD` 参数。 如果未添加 `computerName` 参数，cmdlet 假定装载了本地计算机来访问 VHD。 `computerName` 参数含有安装 VHD 的服务器名称，`VHD` 参数含有 VHD 在指定服务器上的路径。  
  
    > [!NOTE]  
    > 必须添加`computerName`参数运行 cmdlet 的计算机上，如果正在运行 Windows 客户端操作系统。  
    >   
    > 存储 VHD 文件的网络共享文件夹必须向已选择安装 VHD 的服务器的计算机（或本地系统）帐户授予以下访问权限。 仅用户帐户访问权限是不够的。 该共享可向 **“所有人”** 组授予 **“读取”** 和 **“写入”** 权限，以允许访问 VHD，但出于安全原因，不建议这样做。  
    >   
    > -   “文件共享”对话框上的“读/写”权限。  
    > -   **完全控制**上访问**安全**选项卡、 文件或文件夹**属性**对话框。  
  
    ```  
    Uninstall-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
    ```  
  
    **示例：** 以下 cmdlet 从远程服务器 ContosoDC1 中删除 active directory 域服务角色和组策略管理功能。 还已删除管理工具和管理单元，并且目标服务器将被自动重新启动（如果删除需要重新启动服务器）。  
  
    ```  
    Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
    ```  
  
5.  完成删除后，验证，通过打开删除角色和功能**的所有服务器**页在服务器管理器中，选择从中删除角色和功能、 服务器和查看**角色和功能**磁贴上所选服务器的页面。 您还可以运行`Get-WindowsFeature`将所选服务器作为目标的 cmdlet (Get-windowsfeature-computerName <*computer_name*>) 若要查看的服务器上安装角色和功能的列表。  
  
## <a name="BKMK_batch"></a>多个服务器上安装角色和功能，通过运行 Windows PowerShell 脚本  
尽管不能使用添加角色和功能向导在单个向导会话中多个目标服务器上安装角色、 角色服务和功能，可以使用 Windows PowerShell 脚本在多个目标上安装角色、 角色服务和功能正在使用服务器管理器管理的服务器。 用于执行批量部署，因为此过程称为，脚本指向 XML 配置文件，可以轻松地创建使用添加角色和功能向导，并单击**导出配置设置**后通过向导为提前**确认安装选择**添加角色和功能向导的页。  
  
> [!IMPORTANT]  
> 在脚本中指定的所有目标服务器必须都运行与服务器管理器的本地计算机都运行的版本匹配的 Windows Server 的版本。 例如，如果 Windows 10 上运行服务器管理器，您可以安装角色、 角色服务和功能在运行 Windows Server 2016 的服务器上。 如果将基于 GUI 的管理工具添加到安装，安装过程自动将目标服务器正在运行的 Windows Server 的服务器核心安装选项的完整安装选项 （带有完全 GUI，也称为服务器为正在运行服务器图形 Shell)。  
>   
> 在本部分中提供的脚本是如何通过使用执行批量部署的一个示例`Install-WindowsFeature`cmdlet，使用 Windows PowerShell 脚本。 有其他可能的脚本和方法可执行对多个服务器的批量部署。 若要搜索或提供用于部署角色和功能的其他脚本，请搜索 [脚本中心存储库](https://gallery.technet.microsoft.com/ScriptCenter)。  
  
#### <a name="to-install-roles-and-features-on-multiple-servers"></a>在多个服务器上安装角色和功能  
  
1.  如果尚未这样做，则创建 XML 配置文件，其中包含角色、 角色服务和你想在多台服务器上安装的功能。 可以通过运行添加角色和功能向导，选择角色、 角色服务和功能所需，然后单击创建此配置文件**导出配置设置**后到向导的改进**确认安装选择**页。 将配置文件保存到某个方便的位置。 不需要单击**安装**或完成该向导，如果运行的它仅用于创建配置文件。  
  
2.  使用提升的用户权限执行以下操作之一以打开 Windows PowerShell 会话。  
  
    -   在 Windows 桌面上，右键单击任务栏上的 Windows PowerShell，然后单击“以管理员身份运行”。  
  
    -   在 Windows 上**启动**屏幕上，右键单击 Windows PowerShell 磁贴，并在应用栏上，单击**以管理员身份运行**。  
  
3.  复制并将以下脚本粘贴到你的 Windows PowerShell 会话。  
  
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
  
    2.  若要运行此函数，请键入以下内容，然后按 Enter ，其中 `$ServerNames` 为在上述步骤中所创建变量的示例， *C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml* 为在步骤 1 中所创建配置文件路径的示例。  
  
        **Invoke-WindowsFeatureBatchDeployment -computerNames $ServerNames -ConfigurationFilepath C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml**  
  
5.  完成安装后，验证安装，方法是打开**的所有服务器**页在服务器管理器中，选择在其上安装了角色和功能，并查看**角色和功能**所选服务器的页面上磁贴。 您还可以运行`Get-WindowsFeature`针对特定服务器的 cmdlet (`Get-WindowsFeature -computerName` <*computer_name*>) 若要查看的服务器上安装角色和功能的列表。  
  
## <a name="BKMK_FoD"></a>安装.NET Framework 3.5 和其他按需功能  
从 Windows Server 2012 和 Windows 8 开始，.NET Framework 3.5 （其中包括.NET Framework 2.0 和.NET Framework 3.0） 的功能文件不可用默认情况下在本地计算机上。 文件已删除。 可通过 Windows 更新的文件已在按需功能配置，以及.NET Framework 3.5 功能文件中删除的功能。 默认情况下，如果功能文件不可用于运行 Windows Server 2012 或更高版本中，在目标服务器上安装过程中搜索缺少的文件由连接到 Windows 更新。 可以配置组策略设置或安装过程中，指定一个备用源路径替代默认行为，不管通过使用添加角色和功能向导 GUI 或命令行安装。  
  
通过执行以下一项操作，可以安装 .NET Framework 3.5。  
  
-   使用 [通过运行 Install-WindowsFeature cmdlet 来安装 .NET Framework 3.5](#BKMK_dotnetcmdlet) 来添加 `Source` 参数，并指定从中获取 .NET Framework 3.5 功能文件的源。 如果未添加 `Source` 参数，安装进程先确定组策略设置是否指定了功能文件路径，并在找到此类路径后，使用 Windows 更新搜索缺少的功能文件。  
  
-   使用[若要通过使用添加角色和功能向导安装.NET Framework 3.5](#BKMK_arfw)上指定备用源文件位置**确认安装选项**添加角色和功能向导的页。  
  
-   使用 [使用 DISM 安装 .NET Framework 3.5](#BKMK_dism) 来从 Windows 更新中默认获取文件，或通过指定到安装媒体的源路径来实现获取。  
  
如果在本地计算机上找不到功能文件，则使用[在组策略中为功能文件配置备用来源](#BKMK_configgp) 来获取 .NET Framework 3.5 或其他功能。  
  
> [!IMPORTANT]  
> 从远程来源安装功能文件时，源路径或文件共享必须将“读取”  权限授予“任何人”  组（出于安全原因，不建议这样做），或授予目标服务器的计算机（本地系统）帐户；授予用户帐户访问权限并不足够。  
>   
> 即使工作组服务器的计算机帐户具有外部共享“读取”  权限，位于工作组中的服务器也无法访问外部文件共享。 为工作组服务器服务的备用源位置包括安装媒体、Windows 更新和存储在本地工作组服务器上的 VHD 或 WIM 文件。  
>   
> 为正在运行的物理服务器上安装角色、 角色服务和功能时，可以指定 WIM 文件作为替换功能文件源。 WIM 文件的源路径应采用以下格式，与**WIM**作为前缀，功能文件所在作为后缀索引：**WIM:e:\sources\install.wim:4**。 但是，您不能使用 WIM 文件作为源直接安装到脱机 VHD; 的角色、 角色服务和功能的必须安装脱机 VHD 并指向其装入源文件路径，或必须指向包含 WIM 文件的内容的副本的文件夹。  
  
### <a name="BKMK_dotnetcmdlet"></a>若要通过运行 Install-windowsfeature cmdlet 安装.NET Framework 3.5  
  
1.  使用提升的用户权限执行以下操作之一以打开 Windows PowerShell 会话。  
  
    > [!NOTE]  
    > 如果从远程服务器安装角色和功能，您不需要使用提升的用户权限运行 Windows PowerShell。  
  
    -   在 Windows 桌面上，右键单击任务栏上的 Windows PowerShell，然后单击“以管理员身份运行”。  
  
    -   在 Windows 上**启动**屏幕上，右键单击 Windows PowerShell 磁贴，并在应用栏上，单击**以管理员身份运行**。  
  
    -   在运行 Windows Server 2012 R2 的服务器核心安装选项的服务器或 Windows Server 2012 上，键入**PowerShell**在命令提示符下，然后按**Enter**。  
  
2.  键入以下命令，然后按**Enter**。 在以下示例中，源文件位于驱动器 D 上的安装介质中的并排存储区（简称为 **SxS**）中。  
  
    ```  
    Install-WindowsFeature NET-Framework-Core -Source D:\Sources\SxS  
    ```  
  
    如果想要命令使用 Windows 更新作为缺少的功能文件的源，或已使用组策略配置默认源，则无需添加 `Source` 参数，除非要指定不同源。  
  
### <a name="BKMK_arfw"></a>若要通过使用添加角色和功能向导安装.NET Framework 3.5  
  
1.  上**管理**菜单中服务器管理器中，单击**添加角色和功能**。  
  
2.  选择正在运行 Windows Server 2016 的目标服务器。  
  
3.  上**选择的功能**的添加角色和功能向导中，选择页 **.NET Framework 3.5**。  
  
4.  如果组策略设置允许本地计算机这样做，安装进程将尝试使用 Windows 更新获取缺少的功能文件。 单击“安装” ；你无需继续执行下一步。  
  
    如果组策略设置不允许这样做，或者想要使用的.NET Framework 3.5 功能文件上的其他来源**确认安装选择**页的向导中，单击**指定备用源路径**.  
  
5.  提供并排存储区（称为 **SxS**）在安装介质的路径或 WIM 文件的路径。 在以下示例中，安装介质位于驱动器 D。  
  
    **D:\Sources\SxS\\**  
  
    若要指定 WIM 文件，请添加 **WIM:** 前缀，并添加要在 WIM 文件中用作后缀的映像索引，如以下示例所示。  
  
    **WIM:\\\\***server_name***\share\install.wim:3**  
  
6.  单击“确定” ，再单击“安装” 。  
  
### <a name="BKMK_dism"></a>若要使用 DISM 安装.NET Framework 3.5  
  
1.  使用提升的用户权限执行以下操作之一以打开 Windows PowerShell 会话。  
  
    > [!NOTE]  
    > 如果从远程服务器安装角色和功能，您不需要使用提升的用户权限运行 Windows PowerShell。  
  
    -   在 Windows 桌面上，右键单击任务栏上的 Windows PowerShell，然后单击“以管理员身份运行”。  
  
    -   在 Windows 上**启动**屏幕上，右键单击 Windows PowerShell 磁贴，并在应用栏上，单击**以管理员身份运行**。  
  
    -   在服务器上运行服务器核心安装选项，键入**PowerShell**在命令提示符下，然后按**Enter**。  
  
2.  运行以下 DISM 命令之一。  
  
    -   如果计算机有权访问 Windows 更新或已在组策略中，运行以下命令配置的默认源文件位置。  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All  
        ```  
  
    -   如果计算机有权访问安装媒体上，运行类似以下的命令。 在以下示例中，操作系统安装介质位于驱动器 d。`LimitAccess`参数可防止命令尝试联系 Windows 更新或运行 WSUS 的服务器。  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All /LimitAccess /Source:d:\sources\sxs  
        ```  
  
    > [!NOTE]  
    > DISM 命令区分大小写。  
  
### <a name="BKMK_configgp"></a>在组策略中配置为功能文件的备用来源  
本节中介绍的组策略设置指定 .NET Framework 3.5 文件及已在按需功能配置中删除的其他功能文件的授权源位置。 策略设置**指定可选组件安装和组件修复的设置**位于**计算机配置 \ 管理模板 \ 系统**组策略中的文件夹管理控制台或本地组策略编辑器。  
  
> [!NOTE]  
> 你必须是管理员组成员，才能在本地计算机上更改组策略设置。 如果你想要管理的计算机的组策略设置在域级别控制，则你必须是域管理员组成员，才能更改组策略设置。  
  
##### <a name="to-configure-a-default-alternate-source-path-in-group-policy"></a>在组策略中配置默认备用源路径  
  
1.  在本地组策略编辑器或组策略管理控制台中，打开以下策略设置。  
  
    **计算机 \ 系统 \ 指定可选组件安装和组件修复的设置**  
  
2. Sselect**已启用**以启用策略设置中，如果未启用。  
  
3.  在“选项”区域的“备用源路径”文本框中，指定共享文件夹或 WIM 文件的完全限定路径。 若要将 WIM 文件指定为备用源文件位置，请将前缀 **WIM:** 添加到路径，并添加要在 WIM 文件中用作后缀的映像索引。 以下是你可以指定的值示例：  
  
    -   共享文件夹路径: **\\\\***server_name***\share\\* * * 文件夹名*  
  
    -   WIM 文件，在其中的路径**3**表示在其中找到功能文件的图像的索引：**WIM:\\\\***server_name***\share\install.wim:3**  
  
4.  如果不希望由要搜索缺少的功能文件在 Windows 更新中，选择此策略设置控制的计算机**永远不会尝试从 Windows 更新下载负载**。  
  
5.  如果此策略设置控制的计算机通常通过 WSUS 接收更新，但你首选通过 Windows 更新而非 WSUS 查找缺少的功能文件，请选择“联系 Windows 更新直接下载修复内容，而非 Windows Server 更新服务 (WSUS)”。  
  
6.  更改此策略设置后，单击“确定”，再关闭组策略编辑器。  
  
## <a name="see-also"></a>请参阅  
[Windows Server 安装选项](https://go.microsoft.com/fwlink/p/?LinkId=241573)  
[Microsoft.NET Framework 3.5 部署注意事项](https://go.microsoft.com/fwlink/p/?LinkId=248869)  
[如何启用或禁用 Windows 功能](https://go.microsoft.com/fwlink/p/?LinkId=246552)  
  



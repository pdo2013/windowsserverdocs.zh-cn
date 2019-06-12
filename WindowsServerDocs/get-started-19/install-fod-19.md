---
title: 服务器核心应用兼容性按需功能 (FOD)
description: 如何按需安装 Windows Server 功能
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 747258601aa05885d209aacde6947eb7b05e8121
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810801"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>服务器核心应用兼容性按需功能 (FOD)

> 适用于：Windows Server 2019，Windows Server 半年频道

**Server Core 应用程序兼容性功能按需**是一个可选功能包，可以添加到 Windows Server 2019 服务器核心安装或 Windows Server 半年频道，在任何时间。

在功能上需 (FOD) 的详细信息，请参阅[按需功能](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)。

## <a name="why-install-the-app-compatibility-fod"></a>为什么安装应用程序兼容性 FOD 吗？

应用程序兼容性，按需为 Server Core 功能通过显著提高了应用程序兼容性的 Windows 服务器核心安装选项包括二进制文件和带有桌面体验的 Windows Server 中的包的子集，而无需添加Windows Server 桌面体验的图形环境。 此可选包也不能在单独的 ISO，或从 Windows 更新，但只能添加到 Windows Server 核心安装和映像。

应用程序兼容性 FOD 提供的两个主值包括：

- 增加服务器应用程序已在市场中或已由组织开发和部署服务器核心的兼容的性。
- 提供 OS 的组件和应用程序兼容性很严重的故障排除和调试方案中使用的软件工具的帮助。

操作系统组件，都可用，因为的服务器核心应用程序兼容性 FOD 部分：

-   Microsoft 管理控制台 (mmc.exe)

-   事件查看器 (Eventvwr.msc)

-   性能监视器 (PerfMon.exe)

-   资源监视器 (Resmon.exe)

-   设备管理器 (Devmgmt.msc)

-   File Explorer (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   磁盘管理 (Diskmgmt.msc)

-   故障转移群集管理器 (CluAdmin.msc)

    -   首先需要添加故障转移群集 Windows Server 功能。

        -   从提升的 PowerShell 会话： 

            ```PowerShell
            Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools
            ```

        -   若要运行故障转移群集管理器中，输入**cluadmin**在命令提示符处。

运行 Windows Server 的服务器，1903年和更高版本还支持以下组件 （当使用相同版本的应用程序兼容性 FOD）：

- HYPER-V 管理器 (virtmgmt.msc)
- 任务计划程序 (taskschd.msc)

## <a name="installing-the-app-compatibility-fod"></a>安装应用程序兼容性 FOD

只能在 Server Core 上安装应用程序兼容性 FOD。 请勿尝试将服务器核心应用程序兼容性 FOD 添加到带桌面体验的 Windows Server 的 Windows Server 安装。 同一 FOD 可选包 ISO 可用于 Windows Server 2019 服务器核心安装或 Windows Server 半年频道的安装。

1. 如果服务器可以连接到 Windows 更新，只需是从提升的 PowerShell 会话运行以下命令，然后在命令完成运行后，重新启动 Windows Server:

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0
    ```

2. 如果服务器无法连接到 Windows 更新，而是下载服务器 FOD 可选包 ISO，并将 ISO 复制到本地网络上的共享文件夹：

   - 如果您具有可以在其中获取 OS ISO 映像文件的同一门户中下载 Server FOD ISO 映像文件的卷许可证：[批量许可服务中心](https://www.microsoft.com/Licensing/servicecenter/default.aspx)。
   - 将服务器 FOD ISO 映像文件，还可以在[Microsoft 评估中心](https://www.microsoft.com/evalcenter/evaluate-windows-server)或在[Visual Studio 门户](https://visualstudio.microsoft.com)订阅者。

3. 使用服务器核心计算机连接到你的本地网络和你想要添加到 FOD 上的管理员帐户登录。

4. 使用**net 使用**，或通过某些其他方法，以连接到 FOD ISO 的位置。

5. 将 FOD ISO 复制到您选择的本地文件夹。

6. 在提升的 PowerShell 会话中使用以下命令装载 FOD ISO:

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

7. 运行下面的命令：

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0 -Source <Mounted_Server_FOD_Drive> -LimitAccess
     ```

8. 进度栏完成后，重新启动操作系统。

   有关 DISM 命令的详细信息，请参阅[在 Windows PowerShell 中使用 DISM](https://docs.microsoft.com/windows-hardware/manufacture/desktop/use-dism-in-windows-powershell-s14)

## <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>若要根据需要向 Internet Explorer 11 服务器核心 （在添加服务器核心应用程序兼容性 FOD）

 >[!NOTE]  
   > 服务器核心应用程序兼容性 FOD 用于 Internet Explorer 11 中，添加需要但不需要添加服务器核心应用程序兼容性 FOD Internet Explorer 11。

1. 以管理员身份登录对应用程序兼容性 FOD 已添加的服务器核心计算机和服务器 FOD 可选包 ISO 以本地方式复制。

2. 启动 PowerShell，通过输入**powershell.exe**在命令提示符。

3. 使用以下命令装载 FOD ISO:

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

4. 运行以下命令，使用`$package_path`变量以输入到 Internet Explorer cab 文件的路径：

    ```PowerShell
    $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

    Add-WindowsPackage -Online -PackagePath $package_path
    ```

5. 进度栏完成后，重新启动操作系统。

## <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>发行说明和建议的服务器核心应用程序兼容性 FOD 和 Internet Explorer 11 的可选包

> [!IMPORTANT]
> Windows Server 上安装的 FODs，版本 1809年不会保持不变的就地升级到 Windows Server 版本 1903 之后, 因此必须在升级后重新安装它们。 或者，您可以将 FODs 添加到新的 Windows Server 安装源，在升级之前。 这可确保任何 FODs 的新版本在升级完成后存在。 有关详细信息，请参阅[添加到脱机 WIM Server Core 映像的功能和可选包](install-fod-19.md#add-capabilities)。

- **重要说明：** 读取任何问题、 注意事项或指导来继续进行安装和使用的服务器核心应用程序兼容性 FOD 和 Internet Explorer 11 可选包的 Windows Server 2019 发行说明。

- 很可能会遇到闪烁的 Server Core 的控制台体验问题时使用的 Windows Update 安装累积更新后添加应用程序兼容性 FOD。  更新使用 2018 年 12 月，解决此问题。  有关详细信息和解决方法步骤，请参阅[知识库文章 4481610:屏幕闪烁之后在 Windows Server 2019 Server Core 安装服务器核心应用程序兼容性 FOD](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core)。

- 应用程序兼容性 FOD 和重新启动服务器的安装后，命令控制台窗口框架颜色将更改为不同为蓝色阴影。

- 如果您选择安装 Internet Explorer 11 可选包，请注意，不支持双击以打开本地保存的.htm 文件。 但是，你可以**右键单击**，然后选择**打开与 IE**，或者可以直接从 Internet 资源管理器中打开**文件** -> **打开**.

- 若要进一步增强应用程序兼容性的服务器核心使用应用程序兼容性 FOD，IIS 管理控制台具有已添加到服务器核心作为可选组件。  但是，它是绝对必要，若要首先添加应用程序兼容性 FOD 使用 IIS 管理控制台。 IIS 管理控制台依赖于 Microsoft 管理控制台 (mmc.exe)，此程序只能用在 Server Core 上通过应用程序兼容性 FOD 添加。  使用 Powershell [ **Install-windowsfeature** ](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps)添加 IIS 管理控制台。

- 按常规的点的指导，在服务器上的安装应用核心 （有或没有这些可选包） 它时，有时需要使用无提示安装选项和说明。 
    
  - 例如，SQL Server 2016 和 SQL Server 2017 的 SQL Server Management Studio 可以在 Server Core 上安装且完全正常运行存在应用程序兼容性 FOD 时。  查看，请[从命令提示符安装 SQL Server](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017)。
  - 如果不需要 SQL Server Management Studio，则无需安装服务器核心应用程序兼容性 FOD。  查看，请[Server Core 上安装 SQL Server](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017)。

## <a name="a-idadd-capabilities-adding-capabilities-and-optional-packages-to-an-offline-wim-server-core-image"></a><a id="add-capabilities"> 将功能和可选包添加到脱机 WIM Server Core 映像

1. 将 Windows Server 和 Server FOD ISO 图像文件下载到 Windows 计算机上的本地文件夹。

   - 如果你拥有的批量许可证可以下载 Windows Server 和 Server FOD ISO 图像文件从[批量许可服务中心](https://www.microsoft.com/Licensing/servicecenter/default.aspx)。
   - Server FOD ISO 图像文件也是可用的版本上的长期服务频道[Microsoft 评估中心](https://www.microsoft.com/evalcenter/evaluate-windows-server)或在[Visual Studio 门户](https://visualstudio.microsoft.com)订阅者。

2. 打开 PowerShell 会话以管理员身份，然后使用以下命令来装载驱动器作为图像文件：

   ```PowerShell
   Mount-DiskImage -ImagePath Path_To_ServerFOD_ISO
   Mount-DiskImage -ImagePath Path_To_Windows_Server_ISO
   ```

3. 复制到本地文件夹的 Windows Server ISO 文件的内容 (例如， *C:\SetupFiles\WindowsServer*)。

4. 获取你想要通过使用以下命令修改 Install.wim 文件中的映像名称。<br>
使用`$install_wim_path`变量以输入位于 \Sources 文件夹中的 ISO 文件的 Install.wim 文件的路径。

   ```PowerShell
   $install_wim_path = "C:\SetupFiles\WindowsServer\sources\install.wim"

   Get-WindowsImage -ImagePath $install_wim_path
   ```

5. 装载 Install.wim 文件的新文件夹中，通过使用以下命令将示例变量值替换你自己，并重复使用`$install_wim_path`变量前一命令。<br>

   - `$image_name` -输入你想要装载的映像的名称。
   - `$mount_folder variable` -指定要访问 Install.wim 文件的内容时使用的文件夹。

   ```PowerShell
   $image_name = "Windows Server Datacenter"
   $mount_folder = "c:\test\offline"

   Mount-WindowsImage -ImagePath $install_wim_path -Name $image_name -path $mount_folder
   ```

6. 通过使用以下命令，用您自己将示例变量值到装载 Install.wim 映像添加功能和所需的包。<br>

   - `$capability_name` -指定安装 （在此情况下，AppCompatibility 功能） 的功能的名称。
   - `$package_path` -指定要安装 （在此情况下，Internet Explorer） 的包的路径。
   - `$fod_drive` -指定已装载的 Server FOD 映像的驱动器号。

   ```PowerShell
   $capability_name = "ServerCore.AppCompatibility~~~~0.0.1.0"
   $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"
   $fod_drive = "d:\"

   Add-WindowsCapability -Path $mount_folder -Name $capability_name -Source $fod_drive -LimitAccess
   Add-WindowsPackage -Path $mount_folder -PackagePath $package_path
   ```

7. 卸除并将更改提交到 Install.wim 文件中，通过使用以下命令，使用`$mount_folder`变量从之前的命令：

   ```PowerShell
   Dismount-WindowsImage -Path $mount_folder -Save
   ```

现在可以将服务器升级为 Windows Server 安装文件创建的文件夹从运行 setup.exe (在此示例中：*C:\SetupFiles\WindowsServer*)。 现在，此文件夹包含其他功能和可选包包含 Windows Server 安装文件。
---
title: Server Core 应用兼容性按需功能 (FOD)
description: 如何安装 Windows Server 按需功能
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 441cd9593371cb7e5018a61c3d7a6af991ce8fed
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280169"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>Server Core 应用兼容性按需功能 (FOD)

> 适用于：Windows Server 2019、Windows Server 半年频道

**Server Core 应用兼容性按需功能**是一个可选的功能包，随时可将其添加到 Windows Server 2019 Server Core 安装或 Windows Server 半年频道。

有关按需功能 (FOD) 的详细信息，请参阅[按需功能](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)。

## <a name="why-install-the-app-compatibility-fod"></a>为何要安装应用兼容性 FOD？

应用兼容性是适用于 Server Core 的一项按需功能，包含带桌面体验的 Windows Server 的一部分二进制文件和包，无需添加 Windows Server 桌面体验图形环境，因此显著提高了 Windows Server Core 安装选项的应用兼容性。 此可选包在单独的 ISO 中提供，或者通过 Windows 更新提供，但只能添加到 Windows Server Core 安装和映像。

应用兼容性 FOD 提供的两项主要价值为：

- 提高已面市的或者已由组织开发并部署的服务器应用程序的 Server Core 兼容性。
- 帮助提供 OS 组件，并提高重要故障排除和调试方案中使用的软件工具的应用兼容性。

作为 Server Core 应用兼容性 FOD 一部分提供的操作系统组件包括：

-   Microsoft 管理控制台 (mmc.exe)

-   事件查看器 (Eventvwr.msc)

-   性能监视器 (PerfMon.exe)

-   资源监视器 (Resmon.exe)

-   设备管理器 (Devmgmt.msc)

-   文件资源管理器 (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   磁盘管理 (Diskmgmt.msc)

-   故障转移群集管理器 (CluAdmin.msc)

    -   首先需要添加故障转移群集 Windows Server 功能。

        -   在权限提升的 PowerShell 会话中： 

            ```PowerShell
            Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools
            ```

        -   若要运行故障转移群集管理器，请在命令提示符下输入 **cluadmin**。

运行 Windows Server 版本 1903 和更高版本的服务器还支持以下组件（使用相同版本的应用兼容性 FOD 时）：

- Hyper-V 管理器 (virtmgmt.msc)
- 任务计划程序 (taskschd.msc)

## <a name="installing-the-app-compatibility-fod"></a>安装应用兼容性 FOD

只能在 Server Core 上安装应用兼容性 FOD。 请勿尝试将 Server Core 应用兼容性 FOD 添加到带桌面体验的 Windows Server 安装。 同一 FOD 可选包 ISO 可用于 Windows Server 2019 Server Core 安装或 Windows Server 半年频道安装。

1. 如果服务器可以连接到 Windows 更新，则只需从权限提升的 PowerShell 会话运行以下命令，然后在该命令完成运行后重启 Windows Server：

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0
    ```

2. 如果服务器无法连接到 Windows 更新，请下载服务器 FOD 可选包 ISO，然后将该 ISO 复制到本地网络上的某个共享文件夹：

   - 如果你有批量许可证，可以从获取 OS ISO 映像文件的同一个门户下载 Server FOD ISO 映像文件：[批量许可服务中心](https://www.microsoft.com/Licensing/servicecenter/default.aspx)。
   - Server FOD ISO 映像文件还会在 [Microsoft 评估中心](https://www.microsoft.com/evalcenter/evaluate-windows-server)或 [Visual Studio 门户](https://visualstudio.microsoft.com)中为订阅者提供。

3. 在已连接到本地网络的、要将 FOD 添加到的 Server Core 计算机上使用管理员帐户登录。

4. 使用 **net use** 或其他某种方法连接到 FOD ISO 的位置。

5. 将 FOD ISO 复制到所选的本地文件夹。

6. 在权限提升的 PowerShell 会话中，使用以下命令装载 FOD ISO：

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

7. 运行以下命令：

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0 -Source <Mounted_Server_FOD_Drive> -LimitAccess
     ```

8. 进度条显示任务完成后，重启操作系统。

   有关 DISM 命令的详细信息，请参阅[在 Windows PowerShell 中使用 DISM](https://docs.microsoft.com/windows-hardware/manufacture/desktop/use-dism-in-windows-powershell-s14)

## <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>根据需要将 Internet Explorer 11 添加到 Server Core（在添加 Server Core 应用兼容性 FOD 之后）

 >[!NOTE]  
   > 需要使用 Server Core 应用兼容性 FOD 来添加 Internet Explorer 11，但不需要使用 Internet Explorer 11 来添加 Server Core 应用兼容性 FOD。

1. 在已添加应用兼容性 FOD 并已在本地复制 Server FOD 可选包 ISO 的 Server Core 计算机上以管理员身份登录。

2. 在命令提示符下输入 **powershell.exe** 启动 PowerShell。

3. 使用以下命令装载 FOD ISO：

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

4. 运行以下命令，并在其中使用 `$package_path` 变量输入 Internet Explorer cab 文件的路径：

    ```PowerShell
    $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

    Add-WindowsPackage -Online -PackagePath $package_path
    ```

5. 进度条显示任务完成后，重启操作系统。

## <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>Server Core 应用兼容性 FOD 和 Internet Explorer 11 可选包的发行说明与建议

> [!IMPORTANT]
> 就地升级到 Windows Server 版本 1903 之后，安装在 Windows Server 版本 1809 上的 FOD 不会保留在原地，因此升级后必须重新安装它们。 或者，在升级之前可将 FOD 添加到新的 Windows Server 安装源。 这可以确保在升级完成之后，提供所有 FOD 的新版本。 有关详细信息，请参阅[将功能和可选包添加到脱机 WIM Server Core 映像](install-fod-19.md#add-capabilities)。

- **重要说明：** 在继续安装和使用 Server Core 应用兼容性 FOD 与 Internet Explorer 11 可选包之前，请阅读 Windows Server 2019 发行说明了解任何问题、注意事项或指导。

- 使用 Windows 更新安装累积更新后添加应用兼容性 FOD 时，有可能会遇到 Server Core 控制台体验闪烁的问题。  2018 年 12 月更新已解决此问题。  有关详细信息和解决方法步骤，请参阅[知识库文章 4481610：在 Windows Server 2019 Server Core 中安装 Server Core 应用兼容性 FOD 后屏幕闪烁](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core)。

- 安装应用兼容性 FOD 并重新启动服务器后，命令控制台窗口框架颜色将更改为不同的蓝色调。

- 如果同时选择安装 Internet Explorer 11 可选包，请注意，不支持双击打开本地保存的 .htm 文件。 但是，可以**右键单击**并选择“使用 Internet Explorer 打开”，或者可以直接在 Internet Explorer 中选择“文件” -> “打开”来打开此类文件。   

- 为了进一步使用应用兼容性 FOD 增强 Server Core 的应用兼容性，IIS 管理控制台现已作为一个可选组件添加到 Server Core。  但是，若要使用 IIS 管理控制台，必须先添加应用兼容性 FOD。 IIS 管理控制台依赖于 Microsoft 管理控制台 (mmc.exe)，而后者只能在添加了应用兼容性 FOD 的 Server Core 中使用。  使用 Powershell [**Install-WindowsFeature**](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) 添加 IIS 管理控制台。

- 作为一项常规指导，在 Server Core（包含或不包含这些可选包）上安装应用时，有时需要使用无提示安装选项和指令。 
    
  - 例如，适用于 SQL Server 2016 和 SQL Server 2017 的 SQL Server Management Studio 可以在 Server Core 上安装，如果存在应用兼容性 FOD，则它可以完全正常运行。  请参阅[从命令提示符安装 SQL Server](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017)。
  - 如果不需要 SQL Server Management Studio，则无需安装 Server Core 应用兼容性 FOD。  请参阅[在 Server Core 上安装 SQL Server](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017)。

## <a name="a-idadd-capabilities-adding-capabilities-and-optional-packages-to-an-offline-wim-server-core-image"></a><a id="add-capabilities"> 将功能和可选包添加到脱机 WIM Server Core 映像

1. 将 Windows Server 和 Server FOD ISO 映像文件下载到 Windows 计算机上的本地文件夹。

   - 如果你有批量许可证，可以从[批量许可服务中心](https://www.microsoft.com/Licensing/servicecenter/default.aspx)下载 Windows Server 和 Server FOD ISO 映像文件。
   - Server FOD ISO 映像文件还会在 [Microsoft 评估中心](https://www.microsoft.com/evalcenter/evaluate-windows-server)或 [Visual Studio 门户](https://visualstudio.microsoft.com)中，作为长期服务频道发行版为订阅者提供。

2. 以管理员身份打开 PowerShell 会话，然后使用以下命令将映像文件装载为驱动器：

   ```PowerShell
   Mount-DiskImage -ImagePath Path_To_ServerFOD_ISO
   Mount-DiskImage -ImagePath Path_To_Windows_Server_ISO
   ```

3. 将 Windows Server ISO 文件的内容复制到本地文件夹（例如 *C:\SetupFiles\WindowsServer*）。

4. 使用以下命令获取 Install.wim 文件中要修改的映像名称。<br>
使用 `$install_wim_path` 变量输入位于 ISO 文件的 \Sources 文件夹中 Install.wim 文件的路径。

   ```PowerShell
   $install_wim_path = "C:\SetupFiles\WindowsServer\sources\install.wim"

   Get-WindowsImage -ImagePath $install_wim_path
   ```

5. 使用以下命令（请将示例变量值替换为自己的值，并重复使用前一命令中的 `$install_wim_path` 变量），在新文件夹中装载 Install.wim 文件。<br>

   - `$image_name` - 输入要装载的映像名称。
   - `$mount_folder variable` - 指定访问 Install.wim 文件的内容时要使用的文件夹。

   ```PowerShell
   $image_name = "Windows Server Datacenter"
   $mount_folder = "c:\test\offline"

   Mount-WindowsImage -ImagePath $install_wim_path -Name $image_name -path $mount_folder
   ```

6. 使用以下命令（请将示例变量值替换为自己的值）将所需的功能和包添加到装载的 Install.wim 映像。<br>

   - `$capability_name` - 指定要安装的功能（在本例中为 AppCompatibility 功能）的名称。
   - `$package_path` - 指定要安装的包（在本例中为 Internet Explorer）的路径。
   - `$fod_drive` - 指定已装载的 Server FOD 映像的驱动器号。

   ```PowerShell
   $capability_name = "ServerCore.AppCompatibility~~~~0.0.1.0"
   $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"
   $fod_drive = "d:\"

   Add-WindowsCapability -Path $mount_folder -Name $capability_name -Source $fod_drive -LimitAccess
   Add-WindowsPackage -Path $mount_folder -PackagePath $package_path
   ```

7. 使用以下命令（其中使用了前面命令中的 `$mount_folder` 变量）卸除映像并将更改提交到 Install.wim 文件：

   ```PowerShell
   Dismount-WindowsImage -Path $mount_folder -Save
   ```

现在，可以在为 Windows Server 安装文件创建的文件夹中运行 setup.exe 来升级服务器（在本示例中，该文件夹为：*C:\SetupFiles\WindowsServer*）。 现在，此文件夹包含 Windows Server 安装文件及其他功能与可选包。
---
title: 服务器核心应用兼容性按需功能 (FOD)
description: 如何按需安装 Windows Server 功能
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: b8211ace56aa6565295a15adce26a8dfbc98e1e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866108"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>服务器核心应用兼容性按需功能 (FOD)

> 适用于 Windows Server 2019 和 Windows Server 版本 1809

**Server Core 应用程序兼容性功能按需**是一个可选功能包，可以添加到 Windows Server 2019 服务器核心安装或 Windows Server 版本 1809，在任何时间。

在功能上需 (FOD) 的详细信息，请参阅[按需功能](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)。


## <a name="why-install-the-app-compatibility-fod"></a>为什么安装应用程序兼容性 FOD 吗？ 

应用程序兼容性，按需为 Server Core 功能通过显著提高了应用程序兼容性的 Windows 服务器核心安装选项包括二进制文件和带有桌面体验的 Windows Server 中的包的子集，而无需添加Windows Server 桌面体验的图形环境。 此可选包也不能在单独的 ISO，或从 Windows 更新，但只能添加到 Windows Server 核心安装和映像。

应用程序兼容性 FOD 提供的两个主值包括：

1.  增加服务器应用程序已在市场中或已由组织开发和部署服务器核心的兼容的性。

2.  提供 OS 的组件和应用程序兼容性很严重的故障排除和调试方案中使用的软件工具的帮助。

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

        -   使用 Powershell Cmdlet 将添加， `Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools`

        -   若要运行故障转移群集管理器中，输入**cluadmin**在命令提示符处。

## <a name="installing-the-app-compatibility-fod"></a>安装应用程序兼容性 FOD

 >[!IMPORTANT] 
   >只能在 Server Core 上安装应用程序兼容性 FOD。 不要尝试将服务器核心应用程序兼容性 FOD 添加到带桌面体验的 Windows Server 的 Windows Server 安装。

### <a name="to-add-the-server-core-app-compatibility-feature-on-demand-fod-to-a-running-instance-of-server-core"></a>若要添加到正在运行的实例的服务器核心的按需 (FOD) 的服务器核心应用程序兼容性功能

 >[!NOTE] 
   > 此过程使用部署映像服务和管理 (DISM.exe) 命令行工具。 有关 DISM 命令的详细信息，请参阅[DISM 功能包服务命令行选项](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-capabilities-package-servicing-command-line-options)。

>[!NOTE] 
   > 可以为 Windows Server 2019 服务器核心安装或 Windows Server 版本 1809年、 安装使用相同 FOD 可选包 ISO。

>[!NOTE] 
   > 如果您的计算机或虚拟机，并在运行 Server Core 能够连接到 Windows 更新，则可以跳过下面的步骤 1-7。 但请务必留 /Source 和 /LimitAccess 从步骤 8 中的 DISM 命令。

1. 下载服务器 FOD 可选包 ISO，并将 ISO 复制到本地网络上的共享文件夹：

 - 如果您具有可以在其中获取 OS ISO 映像文件的同一门户中下载 Server FOD ISO 映像文件的卷许可证：[批量许可服务中心](https://www.microsoft.com/Licensing/servicecenter/default.aspx)。
 - 将服务器 FOD ISO 映像文件，还可以在[Microsoft 评估中心](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)或在[Visual Studio 门户](https://visualstudio.microsoft.com)订阅者。


2. 以管理员身份登录，连接到本地网络和你想要添加到 FOD 在服务器核心计算机上。

3. 使用**net 使用**，或通过某些其他方法，以连接到 FOD ISO 的位置。

4. 将 FOD ISO 复制到您选择的本地文件夹。

5. 启动 PowerShell，通过输入**powershell.exe**在命令提示符。

6. 使用以下命令装载 FoD ISO:

        Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

7. 类型**退出**退出 PowerShell。

8.  运行下面的命令：

        DISM /Online /Add-Capability /CapabilityName:"ServerCore.AppCompatibility~~~~0.0.1.0" /Source:drive_letter_of_mounted_ISO: /LimitAccess

9.  进度栏完成后，重新启动操作系统。

### <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>若要根据需要向 Internet Explorer 11 服务器核心 （在添加服务器核心应用程序兼容性 FOD）

 >[!NOTE]  
   > 服务器核心应用程序兼容性 FOD 用于 Internet Explorer 11 中，添加需要但不需要添加服务器核心应用程序兼容性 FOD Internet Explorer 11。

1.  以管理员身份登录对应用程序兼容性 FOD 已添加的服务器核心计算机和服务器 FOD 可选包 ISO 以本地方式复制。

2.  启动 PowerShell，通过输入**powershell.exe**在命令提示符。

3.  使用以下命令装载 FoD ISO:

         Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

4.  类型**退出**退出 PowerShell。


5.  运行下面的命令：

        Dism /online /add-package:drive_letter_of_mounted_iso:"Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

6.  进度栏完成后，重新启动操作系统。

 
#### <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>发行说明和建议的服务器核心应用程序兼容性 FOD 和 Internet Explorer 11 的可选包

- **重要说明：** 任何问题、 注意事项或指导来继续进行安装和使用的服务器核心应用程序兼容性 FOD 和 Internet Explorer 11 可选包的 Windows Server 2019 发行说明，请阅读。
 
 >[!NOTE] 
   > 很可能会遇到闪烁的 Server Core 的控制台体验问题时使用的 Windows Update 安装累积更新后添加应用程序兼容性 FOD。  更新使用 2018 年 12 月，解决此问题。  有关详细信息和解决方法步骤，请参阅[知识库文章 4481610:屏幕闪烁之后在 Windows Server 2019 Server Core 安装服务器核心应用程序兼容性 FOD](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core)。

- 应用程序兼容性 FOD 和重新启动服务器的安装后，命令控制台窗口框架颜色将更改为不同为蓝色阴影。

- 如果您选择安装 Internet Explorer 11 可选包，请注意，不支持双击以打开本地保存的.htm 文件。 但是，你可以**右键单击**，然后选择**打开与 IE**，或者可以直接从 Internet 资源管理器中打开**文件** -> **打开**. 

- 若要进一步增强应用程序兼容性的服务器核心使用应用程序兼容性 FOD，IIS 管理控制台具有已添加到服务器核心作为可选组件。  但是，它是绝对必要，若要首先添加应用程序兼容性 FOD 使用 IIS 管理控制台。 IIS 管理控制台依赖于 Microsoft 管理控制台 (mmc.exe)，此程序只能用在 Server Core 上通过应用程序兼容性 FOD 添加。  使用 Powershell [ **Install-windowsfeature** ](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps)添加 IIS 管理控制台。

- 按常规的点的指导，在服务器上的安装应用核心 （有或没有这些可选包） 它时，有时需要使用无提示安装选项和说明。 
    
 - 例如，SQL Server 2016 和 SQL Server 2017 的 SQL Server Management Studio 可以在 Server Core 上安装且完全正常运行存在应用程序兼容性 FOD 时。  查看，请[从命令提示符安装 SQL Server](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017)。
 - 如果不需要 SQL Server Management Studio，则无需安装服务器核心应用程序兼容性 FOD。  查看，请[Server Core 上安装 SQL Server](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017)。


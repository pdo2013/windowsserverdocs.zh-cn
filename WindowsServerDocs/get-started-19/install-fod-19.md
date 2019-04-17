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
ms.sourcegitcommit: dbb4738fdac3b7911952ff11f1eaed9649d6567a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/07/2019
ms.locfileid: "9149897"
---
# 服务器核心应用兼容性按需功能 (FOD)

> 适用于 Windows Server 2019 和 Windows Server 版本 1809

**服务器核心应用兼容性按需功能**是可以添加到 Windows Server 2019 服务器核心安装或 Windows Server 版本 1809，在任何时间的可选功能包。

在功能上按需 (FOD) 的详细信息，请参阅[按需功能](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)。


## 为什么要安装的应用兼容性 FOD？ 

应用兼容性，服务器核心的按需功能通过而无需添加包括二进制文件和从 Windows Server 桌面体验的包的子集，显著提高 Windows 服务器核心安装选项的应用兼容性Windows Server 桌面体验图形环境。 此可选包可在单独的 ISO，或从 Windows 更新，但只能添加到 Windows Server Core 安装和映像。

应用兼容性 FOD 提供的两个主要值包括：

1.  增加了服务器应用程序已在市场中或已由组织开发和部署的服务器核心的兼容性。

2.  使用提供的操作系统组件和提升的应用兼容性的锐音符疑难解答和调试方案中使用的软件工具的帮助。

在服务器核心应用兼容性 FOD 的一部分包含的操作系统组件：

-   Microsoft 管理控制台 (mmc.exe)

-   事件查看器 (Eventvwr.msc)

-   性能监视器 (PerfMon.exe)

-   资源监视器 (Resmon.exe)

-   设备管理器 (Devmgmt.msc)

-   文件资源管理器 (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   磁盘管理 (Diskmgmt.msc)

-   故障转移群集管理器 (CluAdmin.msc)

    -   首先需要添加故障转移群集的 Windows Server 功能。

        -   使用 Powershell Cmdlet 添加， `Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools`

        -   若要运行故障转移群集管理器，输入**cluadmin**在命令提示符。

## 安装的应用兼容性 FOD

 >[!IMPORTANT] 
   >仅可以在服务器核心上安装应用程序兼容性 FOD。 请不要尝试添加到 Windows Server 安装具有桌面体验的 Windows Server 的服务器核心应用兼容性 FOD。

### 若要将服务器核心应用兼容性按需功能 (FOD) 添加到正在运行的服务器核心实例

 >[!NOTE] 
   > 此过程使用部署映像服务和管理 (DISM.exe) 命令行工具。 有关 DISM 命令的详细信息，请参阅[DISM 功能包服务命令行选项](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-capabilities-package-servicing-command-line-options)。

>[!NOTE] 
   > Windows Server 2019 服务器核心安装，或 Windows Server 版本 1809年，安装可用的 FOD 可选包 ISO。

>[!NOTE] 
   > 如果你的计算机或正在运行服务器核心的虚拟机能够连接到 Windows 更新，则可以跳过下面的步骤 1-7。 但是，请务必去掉 /Source 和 /LimitAccess 从第 8 步中的 DISM 命令。

1. 下载服务器 FOD 可选包 ISO，并将 ISO 复制到本地网络上的共享文件夹：

 - 如果你具有批量许可你可以从何处获取操作系统 ISO 图像文件的相同门户下载服务器 FOD ISO 图像文件：[批量许可服务中心](https://www.microsoft.com/Licensing/servicecenter/default.aspx)。
 - 服务器 FOD ISO 图像文件也是[Microsoft 评估中心](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)或订阅服务器在[Visual Studio 门户](https://visualstudio.microsoft.com)上可用。


2. 以管理员身份登录到本地网络连接，你想要添加到 FOD 在服务器核心计算机上。

3. 使用**网络使用**或其他方法，以连接到 FOD ISO 的位置。

4. 将 FOD ISO 复制到你选择的本地文件夹。

5. 通过输入**powershell.exe**在命令提示符下启动 PowerShell。

6. 通过使用以下命令装载 FoD ISO:

        Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

7. 键入**exit**以退出 PowerShell。

8.  运行以下命令：

        DISM /Online /Add-Capability /CapabilityName:"ServerCore.AppCompatibility~~~~0.0.1.0" /Source:drive_letter_of_mounted_ISO: /LimitAccess

9.  进度栏完成后，重新启动操作系统。

### （可选） 将 Internet Explorer 11 添加到服务器核心，（之后添加服务器核心应用兼容性 FOD）

 >[!NOTE]  
   > 服务器核心应用兼容性 FOD 需要添加的 Internet Explorer 11，但不需要添加服务器核心应用兼容性 FOD 的 Internet Explorer 11。

1.  以管理员身份登录具有已添加的应用兼容性 FOD 在服务器核心计算机和服务器 FOD 可选包 ISO 本地复制上。

2.  通过输入**powershell.exe**在命令提示符下启动 PowerShell。

3.  通过使用以下命令装载 FoD ISO:

         Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

4.  键入**exit**以退出 PowerShell。


5.  运行以下命令：

        Dism /online /add-package:drive_letter_of_mounted_iso:"Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

6.  进度栏完成后，重新启动操作系统。

 
#### 发行说明和建议的服务器核心应用兼容性 FOD 和 Internet Explorer 11 的可选包

- **重要：** 任何问题，注意事项或指南，然后再继续安装和使用的服务器核心应用兼容性 FOD 和 Internet Explorer 11 的可选包的 Windows Server 2019 发行说明，请阅读。
 
 >[!NOTE] 
   > 很可能会遇到使用 Windows 更新安装的累积更新后添加的应用兼容性 FOD 时使用的服务器核心控制台体验闪烁。  将更新与 2018 年 12 月，可以解决此问题。  有关详细信息和解决步骤，请参阅[知识库文章 4481610： 屏幕闪烁后在 Windows Server 2019 服务器核心安装服务器核心应用兼容性 FOD](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core)。

- 安装后的应用兼容性 FOD 和重新启动服务器，命令控制台窗口框架颜色将更改为不同的蓝色底纹。

- 如果你选择还安装 Internet Explorer 11 可选包，请注意，不支持双击打开本地保存的.htm 文件。 但是，你可以**右键单击**并选择**使用 IE 打开**，或直接从 Internet Explorer**文件**可以打开 -> **打开**。 

- 若要进一步增强应用与兼容性的服务器核心应用兼容性 FOD，IIS 管理控制台已添加到服务器核心作为可选组件。  但是，它是绝对有必要，首先添加应用程序兼容性 FOD，若要使用 IIS 管理控制台。 IIS 管理控制台依赖于 Microsoft 管理控制台 (mmc.exe)，该服务仅适用于服务器核心应用兼容性 FOD 添加了。  使用 Powershell[**安装 WindowsFeature**](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps)添加 IIS 管理控制台。

- 常规的指南，当在服务器上的安装应用核心 （有或没有这些可选包） 它点情况，有时需要使用无提示安装选项和说明。 
    
 - 例如，SQL Server 2016 和 SQL Server 2017 SQL Server Management Studio 可以安装在服务器核心上且完全正常运行时为应用兼容性 FOD。  请参阅，[安装 SQL Server 从命令提示符](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017)。
 - 如果不需要 SQL Server Management Studio，则它不需要安装服务器核心应用兼容性 FOD。  请参阅，[安装 SQL Server Core 上的服务器](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017)。


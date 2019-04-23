---
title: 更新 Nano Server
description: " "
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 7f74b35e93d4ddbe39b955daf7f78c4ef693aa9a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835178"
---
# <a name="updating-nano-server"></a>更新 Nano Server

> [!IMPORTANT]
> 自 Windows Server 版本 1709 开始，Nano Server 将仅用作[容器基本操作系统映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)。 查看[对 Nano Server 进行的更改](nano-in-semi-annual-channel.md)以了解这意味着什么。 

Nano Server 提供各种保持最新的方法。 与其他的 Windows Server 安装选项相比，Nano Server 遵循与 Windows 10 维护模型类似的更积极的维护模型。 这些定期版本称为 **Current Branch for Business (CBB)** 版本。 此方法支持想要更快地创新并以快速开发生命周期的云节奏前进的客户。 有关 CBB 的详细信息可在 [Windows Server 博客](https://blogs.technet.microsoft.com/windowsserver/2016/07/12/windows-server-2016-new-current-branch-for-business-servicing-option/)上找到。

**在这些 CBB 版本之间**，Nano Server 通过一系列*累积更新*保持最新版本。 例如，2016 年 9 月 26，与发布适用于 Nano Server 的第一个累积更新[KB4093120](https://support.microsoft.com/help/4093120/windows-10-update-kb4093120)。 由于发布此更新和后续累积更新的原因，我们提供在 Nano Server 上安装这些更新的各种选项。 在本文中，我们使用 KB3192366 更新作为示例，介绍如何获取累积更新并将它们应用到 Nano Server。 有关累积更新模型的详细信息，请参阅 [Microsoft 更新博客](https://blogs.technet.microsoft.com/mu/2016/10/25/patching-with-windows-server-2016/)。

> [!NOTE]
> 如果从媒体或联机存储库安装可选 Nano Server 程序包，这将不包括最近的安全修补程序。 为避免可选程序包和基本操作系统之间的版本不匹配，应在安装任意可选程序包后立即安装最新的累计更新，**然后**再重启服务器。

对于累积更新 Windows Server 2016 的：2016 年 9 月 26 日 ([KB3192366](https://support.microsoft.com/en-us/kb/3192366))，则应首先安装最新服务堆栈更新为 Windows 10 版本 1607年:2016 年 8 月 23 日的必备组件 ([KB3176936](https://support.microsoft.com/en-us/kb/3176936))。 对于以下大多数选项，需要包含 .cab 更新程序包的 .msu 文件。 访问 Microsoft 更新目录，下载所有这些更新程序包：
- [https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3192366](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3192366)
- [https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3176936](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3176936)

从 Microsoft 更新目录下载 .msu 文件后，将它们保存到网络共享或本地目录，例如 C:\ServicingPackages。 可根据知识库编号重命名 .msu 文件，就像我们在下面所做的那样，使它们更易于标识。 然后使用 EXPAND 实用工具将 .cab 文件从 .msu 文件提取到单独的目录中，并将 .cabs 复制到单个文件夹。

```code
    mkdir C:\ServicingPackages_expanded
    mkdir C:\ServicingPackages_expanded\KB3176936
    mkdir C:\ServicingPackages_expanded\KB3192366
    Expand C:\ServicingPackages\KB3176936.msu -F:* C:\ServicingPackages_expanded\KB3176936
    Expand C:\ServicingPackages\KB3192366.msu -F:* C:\ServicingPackages_expanded\KB3192366
    mkdir C:\ServicingPackages_cabs
    copy C:\ServicingPackages_expanded\KB3176936\Windows10.0-KB3176936-x64.cab C:\ServicingPackages_cabs
    copy C:\ServicingPackages_expanded\KB3192366\Windows10.0-KB3192366-x64.cab C:\ServicingPackages_cabs
```

此时可通过几种不同的方法使用提取的 .cab 文件将这些更新应用到 Nano Server 映像，具体取决于需求。 以下选项没有按照特定的优先顺序显示，请使用最适用于环境的选项。

> [!NOTE]
> 使用 DISM 工具维护 Nano Server 时，必须使用与所维护的 Nano Server 版本相同或更高的 DISM 版本。 在匹配的 Windows 版本中运行 DISM、安装 [Windows 评估和部署工具包 (ADK)](https://developer.microsoft.com/en-us/windows/hardware/windows-assessment-deployment-kit) 或在 Nano Server 上运行 DISM，均可达到此目的。

## <a name="option-1-integrate-a-cumulative-update-into-a-new-image"></a>选项 1：将累积更新集成到新的映像
如果在生成新 Nano Server 映像，可将最新累积更新直接集成到映像，以便在第一次启动时即已完全修补。

```powershell
New-NanoServerImage -ServicingPackagePath 'C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab', 'C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab' -<other parameters>
```

## <a name="option-2-integrate-a-cumulative-update-into-an-existing-image"></a>选项 2：将累积更新集成到现有的映像
如果现在就拥有 Nano Server 映像，并用作创建 Nano Server 特定实例的基线，则可将最新累积更新直接集成到现有基线映像，以便使用该映像创建的计算机在第一次启动时即已完全修补。

```powershell
Edit-NanoServerImage -ServicingPackagePath 'C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab', 'C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab' -TargetPath .\NanoServer.wim
```

## <a name="option-3-apply-the-cumulative-update-to-an-existing-offline-vhd-or-vhdx"></a>选项 3：将累积更新应用到现有脱机 VHD 或 VHDX
如果现在就拥有虚拟硬盘（VHD 或 VHDX），可使用 DISM 工具将该更新应用到虚拟硬盘。 需要确保磁盘没有在使用中，方法是使用磁盘关闭任何 VM 或卸载虚拟硬盘文件。

- 使用 PowerShell

   ```powershell
   Mount-WindowsImage -ImagePath .\NanoServer.vhdx -Path .\MountDir -Index 1
   Add-WindowsPackage -Path .\MountDir -PackagePath  C:\ServicingPackages_cabs
   Dismount-WindowsImage -Path .\MountDir -Save
   ```

- 使用 dism.exe

   ```code
   dism.exe /Mount-Image /ImageFile:C:\NanoServer.vhdx /Index:1 /MountDir:C:\MountDir
   dism.exe /Image:C:\MountDir /Add-Package /PackagePath:C:\ServicingPackages_cabs
   dism.exe /Unmount-Image /MountDir:C:\MountDir /Commit
   ```

## <a name="option-4-apply-the-cumulative-update-to-a-running-nano-server"></a>选项 4：将累积更新应用到正在运行的 Nano Server
如果具有正在运行的 Nano Server VM 或物理主机，并已下载用于更新的 .cab 文件，可在操作系统联机的同时使用 DISM 工具应用更新。 需要在 Nano Server 上本地复制 .cab 文件，或将该文件复制到可访问的网络位置。 如果在应用服务堆栈更新，请确保在应用服务堆栈更新前重启服务器，然后应用其他更新。

> [!NOTE]
> 如果已使用 New-NanoServerImage cmdlet 创建了 Nano Server VHD 或 VHDX 映像，但没有指定用于虚拟硬盘文件的 MaxSize，则 4GB 的默认大小太小，不能应用累积更新。 在安装更新前，请使用 Hyper-V 管理器、磁盘管理、PowerShell 或其他工具将虚拟硬盘和系统卷的大小扩展到至少 10GB，或者在 DISM 工具上使用 ScratchDir 参数设置可用空间至少为 10GB 的卷的暂存目录。

```powershell
$s = New-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
Copy-Item -ToSession $s -Path C:\ServicingPackages_cabs -Destination C:\ServicingPackages_cabs -Recurse
Enter-PSSession $s
```

- 使用 PowerShell

   ```powershell
   # Apply the servicing stack update first and then restart
   Add-WindowsPackage -Online -PackagePath C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab
   Restart-Computer; exit

   # After restarting, apply the cumulative update and then restart
   Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
   Add-WindowsPackage -Online -PackagePath C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab
   Restart-Computer; exit
   ```

- 使用 dism.exe
   ```powershell
   # Apply the servicing stack update first and then restart
   dism.exe /Online /Add-Package /PackagePath:C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab
   
   # After the operation completes successfully and you are prompted to restart, it's safe to
   # press Ctrl+C to cancel the pipeline and return to the prompt
   Restart-Computer; exit

   # After restarting, apply the cumulative update and then restart
   Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
   dism.exe /Online /Add-Package /PackagePath:C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab
   Restart-Computer; exit
   ```

## <a name="option-5-download-and-install-the-cumulative-update-to-a-running-nano-server"></a>选项 5:下载并安装到正在运行的 Nano Server 累积更新

如果具有正在运行的 Nano Server VM 或物理主机，可使用 Windows Update WMI 提供程序在操作系统联机时下载并安装更新。 使用此方法无需从 Microsoft 更新目录中单独下载 .msu 文件。 WMI 提供程序将同时检测、下载和安装所有可用更新。

```powershell
Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
```

- 扫描可用更新
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  
   $result = $ci | Invoke-CimMethod -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=0";OnlineScan=$true}
   $result.Updates
   ```

- 安装所有可用更新
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession
   Invoke-CimMethod -InputObject $ci -MethodName ApplyApplicableUpdates
   Restart-Computer; exit
   ```

- 获取安装的更新列表
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession
   $result = $ci | Invoke-CimMethod -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=1";OnlineScan=$true}
   $result.Updates
   ```
   
## <a name="additional-options"></a>其他选项
其他更新 Nano Server 的方法可能与上述选项重合或对其加以补充。 此类选项包括使用 Windows Server Update Services (WSUS)、System Center Virtual Machine Manager (VMM)、任务计划程序或非 Microsoft 解决方案。
- 通过设置以下注册表项[配置 WSUS 的 Windows 更新](https://msdn.microsoft.com/en-us/library/dd939844(v=ws.10).aspx)：
  - WUServer
  - WUStatusServer（通常使用与 WUServer 相同的值）
  - UseWUServer
  - AUOptions
- [在 VMM 中管理构造更新](https://technet.microsoft.com/library/gg675084(v=sc.12).aspx)
- [注册计划的任务](https://technet.microsoft.com/library/jj649811.aspx)
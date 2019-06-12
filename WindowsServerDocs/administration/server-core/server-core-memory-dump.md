---
title: 配置服务器核心安装的内存转储文件
description: 了解如何配置 Windows Server 的服务器核心安装的内存转储文件
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 235df6f681de51a12f82b9fad019dd2db45fd486
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435542"
---
# <a name="configure-memory-dump-files-for-server-core-installation"></a>配置服务器核心安装的内存转储文件

>适用于：Windows Server （半年频道） 和 Windows Server 2016

使用以下步骤配置你的服务器核心安装的内存转储。 

## <a name="step-1-disable-the-automatic-system-page-file-management"></a>第 1 步：禁用自动系统页面文件管理

第一步是手动配置系统的故障与恢复选项。 这是所必需完成的剩余步骤。

运行下面的命令： 

```
wmic computersystem set AutomaticManagedPagefile=False
```
 
## <a name="step-2-configure-the-destination-path-for-a-memory-dump"></a>步骤 2：配置内存转储的目标路径

您无需具有页面文件的分区上安装操作系统。 若要将另一个分区上的页面文件，必须创建一个名为的新注册表项**DedicatedDumpFile**。 您可以通过使用定义分页文件的大小**DumpFileSize**注册表项。 若要创建 DedicatedDumpFile 和 DumpFileSize 注册表项，请执行以下步骤： 

1. 在命令提示符处，运行**regedit**命令，以打开注册表编辑器。
2. 找到并单击以下注册表子项：HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl
3. 单击**编辑 > 新建 > 字符串值**。
4. 新值命名**DedicatedDumpFile**，然后按 ENTER。
5. 右键单击**DedicatedDumpFile**，然后单击**修改**。
6. 在中**数值数据**类型 **\<驱动器\>:\\\<Dedicateddumpfile.sys\>** ，然后单击**确定**.

   >[!NOTE] 
   > 替换\<驱动器\>与驱动器具有足够磁盘空间的页面文件，并替换\<Dedicateddumpfile.dmp\>使用专用的文件的完整路径。
 
7. 单击**编辑 > 新建 > DWORD 值**。
8. 类型**DumpFileSize**，然后按 ENTER。
9. 右键单击**DumpFileSize**，然后单击**修改**。
10. 在中**编辑 DWORD 值**下**基**，单击**十进制**。
11. 在中**数值数据**，键入适当的值，然后单击**确定**。
    >[!NOTE]
    > 转储文件的大小是以兆字节 (MB)。
12. 退出注册表编辑器。

确定分区的位置的内存转储后，配置页面文件的目标路径。 若要查看的页面文件的当前目标路径，请运行以下命令：

```
wmic RECOVEROS get DebugFilePath
```

默认目标**DebugFilePath**是 %systemroot%\memory.dmp。 若要更改当前的目标路径，请运行以下命令：

```
wmic RECOVEROS set DebugFilePath = <FilePath>
```

设置\<FilePath\>到目标路径。 例如，以下命令设置 C:\WINDOWS\MEMORY 内存转储目标路径。DMP: 

```
wmic RECOVEROS set DebugFilePath = C:\WINDOWS\MEMORY.DMP
```
 
## <a name="step-3-set-the-type-of-memory-dump"></a>步骤 3:设置类型的内存转储

确定内存转储，若要为服务器配置的类型。 若要查看当前的内存转储类型，请运行以下命令：

```
wmic RECOVEROS get DebugInfoType
```

若要更改当前的内存转储类型，请运行以下命令： 

```
wmic RECOVEROS set DebugInfoType = <Value>
```

\<值\>可以是 0、 1、 2 或 3，定义如下。

- 0:禁用删除的内存转储。
- 1：完全内存转储。 记录您的计算机意外停止时的情况下，所有的系统内存内容。 完全内存转储可能包含从收集内存转储时正在运行的进程的数据。
- 2：内核内存转储 （默认值）。 仅记录内核内存。 这可以加快您的计算机意外停止时的日志文件中记录信息的过程。
- 3：小内存转储。 记录可能有助于确定您的计算机意外终止的原因的有用信息的最小集。

## <a name="step-4-configure-the-server-to-restart-automatically-after-generating-a-memory-dump"></a>步骤 4：将服务器配置为生成内存转储后自动重新启动

默认情况下，它会生成内存转储后自动重新启动服务器。 若要查看当前配置，请运行以下命令：

```
wmic RECOVEROS get AutoReboot
```

如果为值**AutoReboot**为 TRUE 时，服务器将重新启动后自动生成内存转储。 无需任何配置，你可以转到下一步。

如果为值**AutoReboot**为 FALSE 时，服务器将不自动重新启动。 运行以下命令以更改值：

```
wmic RECOVEROS set AutoReboot = true
```
 
## <a name="step-5-configure-the-server-to-overwrite-the-existing-memory-dump-file"></a>步骤 5：将服务器配置为覆盖现有的内存转储文件

默认情况下，服务器将覆盖现有的内存转储文件时新类型的值，则会创建一个。 若要确定是否已配置现有的内存转储文件被覆盖，请运行以下命令：

```
wmic RECOVEROS get OverwriteExistingDebugFile
```

如果值为 1，服务器将覆盖现有的内存转储文件。 不需要任何配置，并可以转到下一步。

如果值为 0，则服务器不会覆盖现有的内存转储文件。 运行以下命令以更改值： 

```
wmic RECOVEROS set OverwriteExistingDebugFile = 1
```
 
## <a name="step-6-set-an-administrative-alert"></a>步骤 6：设置管理警报

确定管理警报是否合适，并且已设置**SendAdminAlert**相应地。 若要查看 SendAdminAlert 的当前值，请运行以下命令：

```
wmic RECOVEROS get SendAdminAlert
```

SendAdminAlert 的可能值为 TRUE 或 FALSE。 若要修改现有 SendAdminAlert 值设为 true，请运行以下命令： 

```
wmic RECOVEROS set SendAdminAlert = true
```
 
## <a name="step-7-set-the-memory-dumps-page-file-size"></a>步骤 7：设置内存转储的页面文件大小

若要检查当前页面文件设置，请运行以下命令之一：

   ```
   wmic.exe pagefile
   ```

   或

   ```
   wmic.exe pagefile list /format:list
   ```

例如，运行以下命令以配置页面文件的初始和最大大小：

```
wmic pagefileset where name="c:\\pagefile.sys" set InitialSize=1000,MaximumSize=5000
```

## <a name="step-8-configure-the-server-to-generate-a-manual-memory-dump"></a>步骤 8：将服务器配置为生成手动内存转储

通过使用 ps/2 键盘，可以手动生成内存转储。 默认情况下禁用此功能并不适用于通用串行总线 (USB) 键盘。

若要启用手动内存转储使用 ps/2 键盘，运行以下命令：

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\i8042prt\Parameters /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
```

若要确定是否已正确启用该功能，请运行以下命令：

```
Reg query HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ i8042prt \ Parameters / v CrashOnCtrlScroll
```

必须重新启动服务器上的更改才会生效。 可以通过运行以下命令重新启动服务器：

```
Shutdown / r / t 0
```

你可以生成使用 ps/2 键盘通过按住右 CTRL 键同时按 SCROLL LOCK 键两次连接到你的服务器的手动内存转储。 这将使计算机错误，错误代码为 0xE2 检查。 重新启动服务器后，新的转储文件将出现在步骤 2 中建立的目标路径。

## <a name="step-9-verify-that-memory-dump-files-are-being-created-correctly"></a>步骤 9：验证正确创建的文件的内存转储

Dumpchk.exe utlity 可用于验证正确创建内存转储文件。 Dumpchk.exe 实用程序未安装与服务器核心安装选项，因此您必须从具有桌面体验的服务器或 Windows 10 中运行它。 此外，必须安装 Windows 产品的调试工具。  

Dumpchk.exe 实用工具，可以将内存转储文件从 Windows Server 2008 的服务器核心安装转移到另一台计算机，通过使用你选择的介质。

> [!WARNING]
> 页面文件可以非常大，因此要认真考虑传输方法和方法需要的资源。
 

其他参考

有关使用内存转储文件的常规信息，请参阅[概述内存转储文件的选项中为 Windows](https://support.microsoft.com/help/254649/overview-of-memory-dump-file-options-for-windows)。

有关专用的转储文件的详细信息，请参阅[如何使用 DedicatedDeumpFile 注册表值来捕获系统内存转储时克服在系统驱动器上的空间限制](https://blogs.msdn.microsoft.com/ntdebugging/2010/04/02/how-to-use-the-dedicateddumpfile-registry-value-to-overcome-space-limitations-on-the-system-drive-when-capturing-a-system-memory-dump/)。




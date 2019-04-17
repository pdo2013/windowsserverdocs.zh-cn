---
title: 配置服务器核心安装的内存转储的文件
description: 了解如何配置 Windows Server 的服务器核心安装的内存转储文件
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: bd22378ec7ce5a1ff4e39546246e6e85ca859c45
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2018
ms.locfileid: "1447956"
---
# <a name="configure-memory-dump-files-for-server-core-installation"></a>配置服务器核心安装的内存转储的文件

>适用于：Windows Server（半年频道）和 Windows Server 2016

使用以下步骤配置的服务器核心安装内存转储。 

## <a name="step-1-disable-the-automatic-system-page-file-management"></a>步骤 1： 禁用自动系统页面文件管理

第一步是手动配置您的系统故障和恢复选项。 这需要完成的其余步骤。

运行以下命令： 

```
wmic computersystem set AutomaticManagedPagefile=False
```
 
## <a name="step-2-configure-the-destination-path-for-a-memory-dump"></a>步骤 2： 配置内存转储的目标路径

您不必具有页面文件的分区上安装操作系统。 将放在另一个分区上的页面文件，您必须创建一个名为**DedicatedDumpFile**的新注册表项。 您可以使用**DumpFileSize**注册表项中定义分页文件的大小。 若要创建 DedicatedDumpFile 和 DumpFileSize 注册表项，请按照下列步骤： 

1. 在命令提示符处运行**regedit**命令以打开注册表编辑器。
2. 找到并单击以下注册表子项： HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl
3. 单击**编辑 > 新建 > 字符串值**。
4. 命名新值**DedicatedDumpFile**，，然后按 ENTER。
5. 右键单击**DedicatedDumpFile**，，，然后单击**修改**。
6. 在**数值数据**类型**\ < Drive\ >: \\\ < Dedicateddumpfile.sys\ >**，然后单击**确定**。

   >[!NOTE] 
   > 替换 \ < Drive\ > 与具有足够的磁盘空间不足的分页文件，并替换驱动器 \ < Dedicateddumpfile.dmp\ > 与专用的文件的完整路径。
 
7. 单击**编辑 > 新建 > DWORD 值**。
8. 键入**DumpFileSize**，，然后按 ENTER。
9. 右键单击**DumpFileSize**，，，然后单击**修改**。
10. 在**编辑 DWORD 值**下**基本**，单击**小数**。
11. 在**值数据**键入适当的值，，然后单击**确定**。
   >[!NOTE]
   > 转储文件的大小为以兆字节 (MB)。
12. 退出注册表编辑器。

确定的内存转储分区位置后，配置页面文件的目标路径。 若要查看当前页面文件的目标路径，请运行以下命令：

```
wmic RECOVEROS get DebugFilePath
```

**DebugFilePath**默认目标是 %systemroot%\memory.dmp。 若要更改当前目标路径，请运行以下命令：

```
wmic RECOVEROS set DebugFilePath = <FilePath>
```

设置 \ < FilePath\ > 到目标路径。 例如，以下命令将内存转储目标路径设置为 C:\WINDOWS\MEMORY。DMP: 

```
wmic RECOVEROS set DebugFilePath = C:\WINDOWS\MEMORY.DMP
```
 
## <a name="step-3-set-the-type-of-memory-dump"></a>步骤 3： 设置的内存转储类型

确定内存转储为您的服务器配置的类型。 若要查看当前的内存转储类型，请运行以下命令：

```
wmic RECOVEROS get DebugInfoType
```

若要更改当前内存转储类型，请运行以下命令： 

```
wmic RECOVEROS set DebugInfoType = <Value>
```

\ < Value\ > 可以是 0、 1、 2 或 3，如下面定义。

- 0： 禁用内存转储的删除操作。
- 1： 完全内存转储。 记录您的计算机意外停止时的情况下，所有系统内存的内容。 完全内存转储可能包含已收集的内存转储时正在运行的进程中的数据。
- 2： 核心内存转储 （默认值）。 仅记录内核内存。 这加快时计算机意外停止日志文件中记录的信息的过程。
- 3： 小内存转储。 将记录的最小的有用信息可以帮助确定您的计算机意外终止的原因。

## <a name="step-4-configure-the-server-to-restart-automatically-after-generating-a-memory-dump"></a>步骤 4： 配置服务器以自动生成的内存转储后重新启动

默认情况下，它将生成的内存转储后自动重新启动服务器。 若要查看当前配置，请运行以下命令：

```
wmic RECOVEROS get AutoReboot
```

如果**AutoReboot**的值为 TRUE，将生成的内存转储后自动重新启动服务器。 不需要配置，您可以继续执行下一步。

如果**AutoReboot**的值为 FALSE，服务器将不自动重新启动。 运行以下命令以更改的值：

```
wmic RECOVEROS set AutoReboot = true
```
 
## <a name="step-5-configure-the-server-to-overwrite-the-existing-memory-dump-file"></a>步骤 5： 配置服务器以覆盖现有的内存转储文件

默认情况下，服务器将覆盖现有的内存转储文件时创建一个新。 若要确定是否已配置现有的内存转储文件将覆盖，请运行以下命令：

```
wmic RECOVEROS get OverwriteExistingDebugFile
```

如果值为 1，则服务器将覆盖现有的内存转储文件。 不需要配置，并可以继续执行下一步。

如果值为 0，服务器将不会覆盖现有的内存转储文件。 运行以下命令以更改的值： 

```
wmic RECOVEROS set OverwriteExistingDebugFile = 1
```
 
## <a name="step-6-set-an-administrative-alert"></a>第 6 步： 设置管理通知

确定是否适合管理警报并相应地设置**SendAdminAlert** 。 若要查看当前值的 SendAdminAlert，运行以下命令：

```
wmic RECOVEROS get SendAdminAlert
```

SendAdminAlert 的可能值为 TRUE 或 FALSE。 若要修改现有的 SendAdminAlert 值为 true，请运行以下命令： 

```
wmic RECOVEROS set SendAdminAlert = true
```
 
## <a name="step-7-set-the-memory-dumps-page-file-size"></a>步骤 7： 设置内存转储页面文件大小

若要检查当前页面文件设置，请运行以下命令之一：

   ```
   wmic.exe pagefile
   ```

   或

   ```
   wmic.exe pagefile list /format:list
   ```

例如，运行以下命令以配置初始和最大页面文件大小：

```
wmic pagefileset where name="c:\\pagefile.sys" set InitialSize=1000,MaximumSize=5000
```

## <a name="step-8-configure-the-server-to-generate-a-manual-memory-dump"></a>步骤 8： 配置服务器以生成手动内存转储

您可以通过使用 PS/2 键盘手动生成内存转储。 默认情况下禁用此功能和不可用的通用串行总线 (USB) 键盘。

若要手动内存转储使用 PS/2 键盘，运行以下命令：

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\i8042prt\Parameters /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
```

若要确定是否已正确启用该功能，请运行以下命令：

```
Reg query HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ i8042prt \ Parameters / v CrashOnCtrlScroll
```

您必须重新启动服务器上的更改才会生效。 您可以通过运行以下命令重新启动服务器：

```
Shutdown / r / t 0
```

您可以生成与连接到您的服务器在两次按 SCROLL LOCK 键的同时按住右 CTRL 键的 PS/2 键盘手动内存转储。 这将使计算机 bug 检查错误代码 0xE2。 重新启动服务器后，新的转储文件显示在您在步骤 2 中建立的目标路径中。

## <a name="step-9-verify-that-memory-dump-files-are-being-created-correctly"></a>步骤 9： 验证正确创建文件的内存转储

您可以使用 dumpchk.exe utlity 验证正确创建内存转储文件。 Dumpchk.exe 实用程序未安装的服务器核心安装选项，因此您必须运行它从与桌面体验服务器或 Windows 10。 此外，必须安装 Windows 产品的调试工具。  

Dumpchk.exe 实用程序允许您将内存转储文件从 Windows Server 2008 的服务器核心安装传输到另一台计算机，使用您选择的介质。

> [!WARNING]
> 仔细考虑转接方法和方法所要求的资源可能会非常大，页面文件。
 

其他参考

有关使用内存转储文件的常规信息，请参阅[Overview of windows 内存转储文件选项](https://support.microsoft.com/help/254649/overview-of-memory-dump-file-options-for-windows)。

有关专用的转储文件的详细信息，请参阅[如何使用 DedicatedDeumpFile 注册表值克服时捕获系统内存转储系统驱动器上的空间限制](https://blogs.msdn.microsoft.com/ntdebugging/2010/04/02/how-to-use-the-dedicateddumpfile-registry-value-to-overcome-space-limitations-on-the-system-drive-when-capturing-a-system-memory-dump/)。




---
title: 修补的服务器核心
description: 了解如何更新 Windows Server 的服务器核心安装
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: b19512a6f34e13469433aba6051f1232824beb0e
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034155"
---
# <a name="patch-a-server-core-installation"></a>服务器核心安装的修补程序

> 适用于：Windows Server （半年频道） 和 Windows Server 2016

您可以修补通过以下方式在运行服务器核心安装的服务器：

- **使用 Windows Update 自动或使用 Windows Server Update Services (WSUS)** 。 通过使用 Windows 更新，自动或通过命令行工具或 Windows Server Update Services (WSUS)，可以服务运行服务器核心安装的服务器。

- **手动**。 即使在不使用 Windows update 或 WSUS 的组织，你可以手动应用更新。

## <a name="view-the-updates-installed-on-your-server-core-server"></a>查看在服务器核心服务器上安装的更新
将新的更新添加到服务器核心之前，它是查看哪些更新已安装的一个好办法。

若要使用 Windows PowerShell 中查看更新，请运行**Get-hotfix**。

若要通过运行命令来查看更新，请运行**systeminfo.exe**。 虽然该工具会检查系统可能有一个短暂的延迟。

您还可以运行**wmic qfe 列表**从命令行。 

## <a name="patch-server-core-automatically-with-windows-update"></a>使用 Windows 更新自动修补程序服务器核心

使用以下步骤来修补服务器使用 Windows 更新自动：

1. 验证当前的 Windows 更新设置：
   ```
   %systemroot%\system32\Cscript scregedit.wsf /AU /v 
   ```

2. 若要启用自动更新：

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 4 
   Net start wuauserv
   ```  

3. 若要禁用自动更新，请运行：

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 1 
   Net start wuauserv 
   ```

如果服务器是域成员，则你还可以使用组策略配置 Windows 更新。 有关详细信息，请参阅 https://go.microsoft.com/fwlink/?LinkId=192470 。 但是，当使用此方法，只有选项 4 （"自动下载并计划安装"） 是与服务器核心安装由于缺少图形界面。 为了更好地控制何时安装哪个更新，你可以使用提供大多数 Windows 更新图形界面的命令行等效项的脚本。 有关脚本的信息，请参阅 https://go.microsoft.com/fwlink/?LinkId=192471。

若要强制 Windows 更新立即检测和安装可用更新，请运行以下命令：

```
Wuauclt /detectnow 
```

取决于安装的更新，你可能需要重启计算机，尽管系统不会通知你这些。 若要确定是否已完成安装过程，使用任务管理器来验证**Wuauclt**或**可信的安装程序**进程是否未活跃运行。 此外可以使用中的方法[查看在服务器核心服务器上安装的更新](#view-the-updates-installed-on-your-server-core-server)检查已安装的更新列表。

## <a name="patch-the-server-with-wsus"></a>使用 WSUS 服务器的修补程序 

如果服务器核心服务器是域成员，则你可以将该服务器配置为使用带组策略的 WSUS 服务器。 有关详细信息，下载[组策略的参考信息](https://www.microsoft.com/download/details.aspx?id=25250)。 此外可以查看[的自动更新配置组策略设置](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>手动修补服务器

下载更新并使其可供服务器核心安装。
在命令提示符处，运行以下命令：

```
Wusa <update>.msu /quiet 
```

取决于安装的更新，你可能需要重启计算机，尽管系统不会通知你这些。

若要手动卸载更新时，运行以下命令：

```
Wusa /uninstall <update>.msu /quiet 
```


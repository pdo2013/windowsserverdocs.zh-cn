---
title: 修补服务器核心
description: 了解如何更新的 Windows Server 的服务器核心安装
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: f51ffae5ed8f91cca386eb209e7a1d8cc664ceeb
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2018
ms.locfileid: "1447961"
---
# <a name="patch-a-server-core-installation"></a>修补程序的服务器核心安装

> 适用于： Windows Server （半年通道） 和 Windows Server 2016

您可以修补按以下方式运行服务器核心安装的服务器：

- **使用 Windows Update 自动或使用 Windows Server Update Services (WSUS)**。 通过使用 Windows Update，自动或使用命令行工具或 Windows Server Update Services (WSUS)，可以服务运行的服务器核心安装服务器。

- **手动**。 即使在组织中的 Windows update 或 WSUS 不使用，也可以手动应用更新。

## <a name="view-the-updates-installed-on-your-server-core-server"></a>查看您的服务器核心服务器上安装更新
向服务器核心添加新的更新之前，最好以查看已安装了哪些更新。

若要使用 Windows PowerShell 查看更新，请运行**获取修补程序**。

若要通过运行命令查看更新，请运行**systeminfo.exe**。 该工具将检查您的系统时，可能会出现延迟一小段时间。

您还可以从命令行运行**wmic qfe 列表**。 

## <a name="patch-server-core-automatically-with-windows-update"></a>修补程序的服务器核心自动使用 Windows Update

使用以下步骤修补程序的服务器自动使用 Windows Update:

1. 验证当前的 Windows Update 设置：
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

如果服务器是域成员，您还可以配置使用组策略的 Windows Update。 有关详细信息，请参阅 https://go.microsoft.com/fwlink/?LinkId=192470。 但是，当您使用此方法，唯一选项 4 （"自动下载和安装计划"） 是与服务器核心安装相关由于缺乏图形界面。 对于更好地控制其将安装更新并时，您可以使用它提供的 Windows Update 图形用户界面的大部分命令行等效的脚本。 有关脚本的信息，请参阅https://go.microsoft.com/fwlink/?LinkId=192471。

若要强制 Windows Update 以立即检测并安装所有可用的更新，请运行以下命令：

```
Wuauclt /detectnow 
```

根据安装的更新，您可能需要重新启动计算机，虽然系统不会通知您此。 若要确定是否已完成安装过程，使用任务管理器验证**Wuauclt**或**受信任的安装程序**进程不当前正在运行。 您可以在[服务器核心服务器上安装更新的视图](#view-the-updates-installed-on-your-Server-Core-server)中使用方法检查安装更新的列表。

## <a name="patch-the-server-with-wsus"></a>修补程序与 WSUS 服务器 

如果服务器核心服务器是域的成员，可以对其使用组策略的 WSUS 服务器进行配置。 有关详细信息，下载[组策略的参考信息](https://www.microsoft.com/download/details.aspx?id=25250)。 您还可以查看[的自动更新配置组策略设置](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>手动修补程序服务器

下载更新并使其可供服务器核心安装。
在命令提示符处运行以下命令：

```
Wusa <update>.msu /quiet 
```

根据安装的更新，您可能需要重新启动计算机，虽然系统不会通知您此。

若要手动卸载更新，请运行以下命令：

```
Wusa /uninstall <update>.msu /quiet 
```


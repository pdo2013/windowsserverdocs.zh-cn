---
title: 修补服务器核心
description: 了解如何更新 Windows Server 的服务器核心安装
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: eacb80d89e7bcc95d6b5c12269d7587dc7d6870c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383320"
---
# <a name="patch-a-server-core-installation"></a>修补服务器核心安装

> 适用于：Windows Server 2019、Windows Server 2016 和 Windows Server （半年频道）

可以通过以下方式修补运行服务器核心安装的服务器：

- **自动使用 Windows 更新或使用 Windows Server Update Services （WSUS）** 。 通过使用 Windows 更新（自动或使用命令行工具或 Windows Server Update Services （WSUS）），可以为运行服务器核心安装的服务器提供服务。

- **手动**。 即使在不使用 Windows update 或 WSUS 的组织中，你也可以手动应用更新。

## <a name="view-the-updates-installed-on-your-server-core-server"></a>查看服务器核心服务器上安装的更新
在将新的更新添加到服务器内核之前，最好查看已安装的更新。

若要使用 Windows PowerShell 查看更新，请运行**Get-热修补程序**。

若要通过运行命令来查看更新，请运行**systeminfo.exe**。 该工具检查系统时可能会有短暂的延迟。

还可以从命令行运行**wmic qfe 列表**。 

## <a name="patch-server-core-automatically-with-windows-update"></a>自动修补服务器核心与 Windows 更新

使用以下步骤，通过 Windows 更新自动修补服务器：

1. 验证当前 Windows 更新设置：
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

如果服务器是域成员，则你还可以使用组策略配置 Windows 更新。 有关详细信息，请参阅 https://go.microsoft.com/fwlink/?LinkId=192470 。 但是，使用此方法时，仅选项4（"自动下载并计划安装"）与服务器核心安装相关，因为缺少图形界面。 为了更好地控制何时安装哪个更新，你可以使用提供大多数 Windows 更新图形界面的命令行等效项的脚本。 有关该脚本的信息，请参阅 https://go.microsoft.com/fwlink/?LinkId=192471 。

若要强制 Windows 更新立即检测和安装可用更新，请运行以下命令：

```
Wuauclt /detectnow 
```

取决于安装的更新，你可能需要重启计算机，尽管系统不会通知你这些。 若要确定安装过程是否已完成，请使用任务管理器来验证**wuauclt.exe**或**受信任的安装程序**进程是否未处于活动状态。 你还可以使用中的方法来[查看安装在服务器核心服务器上的更新](#view-the-updates-installed-on-your-server-core-server)，以检查已安装的更新列表。

## <a name="patch-the-server-with-wsus"></a>利用 WSUS 修补服务器 

如果服务器核心服务器是域成员，则你可以将该服务器配置为使用带组策略的 WSUS 服务器。 有关详细信息，请下载[组策略参考信息](https://www.microsoft.com/download/details.aspx?id=25250)。 你还可以查看[配置组策略设置自动更新](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>手动修补服务器

下载更新并使其可用于服务器核心安装。
在命令提示符下, 运行以下命令:

```
Wusa <update>.msu /quiet 
```

取决于安装的更新，你可能需要重启计算机，尽管系统不会通知你这些。

若要手动卸载更新，请运行以下命令：

```
Wusa /uninstall <update>.msu /quiet 
```


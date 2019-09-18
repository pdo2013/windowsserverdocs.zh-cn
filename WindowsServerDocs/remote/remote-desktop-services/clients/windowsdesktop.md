---
title: Windows 桌面客户端入门
description: 有关 Windows 桌面客户端的基本信息。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 09/13/2019
ms.localizationpriority: medium
ms.openlocfilehash: c864ba0e51054a553bfd53f845bd4d1c9ff3c8ba
ms.sourcegitcommit: 61767c405da44507bd3433967543644e760b20aa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/14/2019
ms.locfileid: "70988235"
---
# <a name="get-started-with-the-windows-desktop-client"></a>Windows 桌面客户端入门

>适用于：Windows 10 和 Windows 7

可以使用 Windows 桌面的远程桌面客户端以远程方式从其他 Windows 设备访问 Windows 应用和桌面。

> [!NOTE]
> - 本文档不适用于 Windows 附带的远程桌面连接 (MSTSC) 客户端。 它适用于新的远程桌面 (MSRDC) 客户端。
> - 此客户端当前仅支持从 [Windows 虚拟桌面](https://aka.ms/wvd)访问远程应用和桌面。
> - 想了解 Windows 桌面客户端的新版本吗？ 请查看 [Windows 桌面客户端中的新功能](windowsdesktop-whatsnew.md)

## <a name="install-the-client"></a>安装客户端

当前可以下载适用于 Windows 64 位版的客户端。 当客户端可用于 Windows 的更多版本时，我们将更新此列表。

- [Windows 64 位版客户端](https://go.microsoft.com/fwlink/?linkid=2068602)

你可以为当前用户安装客户端（这不需要管理员权限），或者你的管理员可以安装和配置客户端，以便设备上的所有用户都可以访问该客户端。

安装后，可以通过搜索“远程桌面”  ，从“开始”菜单启动客户端。

## <a name="feeds"></a>源

通过订阅管理员提供的源，获取可访问的托管资源（如应用和桌面）的列表。 当你订阅时，你的本地电脑上的资源将变为可用。 Windows 桌面客户端当前支持从 Windows 虚拟桌面发布的资源。

### <a name="subscribe-to-a-feed"></a>订阅源

1. 在客户端的主页（也称为“连接中心”）中，点击“订阅”  。
2. 出现提示时，请使用用户帐户登录。
3. 资源将显示在“连接中心”中（按工作区分组）。

可以通过以下方法之一启动资源：

- 访问“连接中心”，然后双击资源以启动它。
- 还可以访问“开始”菜单并查找带有“工作区”名称的文件夹，或在搜索栏中输入资源名称。

### <a name="workspace-details"></a>工作区详细信息

订阅后，可以在“详细信息”面板中查看有关工作区的其他信息：

- 工作区的名称
- 用于订阅的 URL 和用户名
- 应用和桌面的数量
- 上次更新的日期/时间
- 上次更新的状态

访问“详细信息”面板：

1. 在“连接中心”中，点击“工作区”旁边的溢出菜单 ( **...** )。
2. 从下拉菜单中选择“详细信息”  。
3. “详细信息”面板将显示在客户端的右侧。

订阅后，“工作区”会定期自动更新。 资源可能会基于管理员所做的更改而添加、更改或删除。

你还可以在需要时通过选择“详细信息”面板中的“立即更新”来手动查找资源的更新  。

### <a name="unsubscribe-from-a-feed"></a>取消订阅源

本部分将介绍如何取消订阅源。 你可以取消订阅以使用其他帐户重新订阅或从系统中删除资源。

1. 在“连接中心”中，点击“工作区”旁边的溢出菜单 ( **...** )。
2. 从下拉菜单中选择“取消订阅”  。
3. 查看该对话框并选择  “继续”。

## <a name="update-the-client"></a>更新客户端

除非已被管理员禁用，否则在有新版本的客户端可用时，系统会通知你。 此通知可能会直接显示在“连接中心”中，或者显示在“Windows 操作中心”中。 选择通知以启动更新过程。

还可以手动搜索客户端的新更新：

1. 在“连接中心”中，点击客户端顶部的命令栏上的溢出菜单 ( **...** )。
2. 从下拉菜单中选择“关于”  。
3. 点击“检查更新”  。
4. 如果有可用的更新，请点击  “安装更新”以更新客户端。

## <a name="providing-feedback"></a>提供反馈

想要提出功能建议，还是想要报告问题？ 请使用也可以从客户端访问的[反馈中心](feedback-hub://?tabid=2&contextid=883)告诉我们：

1. 在“连接中心”中，点击客户端顶部的命令栏上的溢出菜单 ( **...** )。
2. 从下拉菜单中选择“反馈”以打开“反馈中心”  。

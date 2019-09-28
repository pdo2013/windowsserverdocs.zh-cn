---
title: 适用于管理员的 Windows 桌面客户端
description: 有关 Windows 桌面客户端的信息，主要对管理员有用。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 09/16/2019
ms.localizationpriority: medium
ms.openlocfilehash: b3fc8be8ad53213c41c6683d3cfc9233198d50e3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404111"
---
# <a name="windows-desktop-client-for-admins"></a>适用于管理员的 Windows 桌面客户端

>适用于：Windows 10 和 Windows 7

本主题提供有关 Windows 桌面客户端的其他信息，这些信息对管理员有用。 如需基本使用信息，请参阅 [Windows 桌面客户端入门](windowsdesktop.md)。

## <a name="installation-options"></a>安装选项

虽然你的用户可以在下载客户端后直接进行安装，但如果你要将客户端部署到多个设备，则可能还需要通过其他方式进行部署。 使用组策略或 System Center Configuration Manager 进行部署时，可以通过一个命令行以无提示方式运行安装程序。 运行以下命令即可按设备或按用户部署客户端。

### <a name="per-device-installation"></a>按设备安装

```
msiexec.exe /I <path to the MSI> /qn ALLUSERS=1
```

### <a name="per-user-installation"></a>按用户安装

```
msiexec.exe /i `<path to the MSI>` /qn ALLUSERS=2 MSIINSTALLPERUSER=1
```

## <a name="configuration-options"></a>配置选项

此部分介绍适用于此客户端的新配置选项。

### <a name="configure-update-notifications"></a>配置更新通知

默认情况下，客户端会在有更新的情况下通知你。 若要关闭通知，请设置以下注册表信息：

- **键:** HKLM\Software\Microsoft\MSRDC\Policies
- **类型:** REG_DWORD
- **名称：** AutomaticUpdates
- **数据:** 0 = 禁用通知。 1 = 显示通知。

### <a name="configure-user-groups"></a>配置用户组

可以针对以下用户组类型之一来配置客户端，这决定了客户端何时收到更新。

#### <a name="insider-group"></a>预览体验组

预览体验组用于早期验证，由管理员及其选择的用户组成。 预览体验组充当测试组，目的是检测更新中可能影响性能的任何问题。在确定没有问题后才会将更新发布到公共组。

> [!NOTE]
> 建议每个组织都安排一些用户到预览体验组中，尽早测试更新并发现问题。

为了及早进行验证，新版客户端会在每个月的第二个星期二发布给预览体验组中的用户。 如果更新没有问题，则会在两周后将其发布到公共组。 只要更新准备就绪，预览体验组中的用户就会自动收到更新通知。 有关客户端更改的更多详细信息，可参阅 [Windows 桌面客户端新增功能](windowsdesktop-whatsnew.md)。

若要针对预览体验组来配置客户端，请设置以下注册表信息：

- **键:** HKLM\Software\Microsoft\MSRDC\Policies
- **类型:** REG_SZ
- **名称：** ReleaseRing
- **数据:** insider

#### <a name="public-group"></a>公共组

此组适用于所有用户，是最稳定的版本。 不需执行任何操作来配置此组。

公共组会在每个月的第四个星期二收到经预览体验组测试过的客户端版本。 公共组中的所有用户会收到更新通知，但前提是已启用该设置。

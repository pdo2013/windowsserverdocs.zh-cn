---
title: Windows 桌面客户端中的新功能
description: 了解 Windows 桌面的远程桌面客户端的最新更改
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
ms.date: 09/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 4325bd7b33c16d972cac980e17c10bacbfeffd8c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387586"
---
# <a name="whats-new-in-the-windows-desktop-client"></a>Windows 桌面客户端中的新功能

有关 Windows 桌面客户端的更多详细信息，可参阅 [Windows 桌面客户端入门](windowsdesktop.md)。 可在下面找到客户端的最新更新。

## <a name="latest-client-versions"></a>最新客户端版本

可以针对不同的[用户组](windowsdesktop-admin.md#configure-user-groups)来配置客户端。 下表列出了适用于每个用户组的当前版本：

|用户组 |版本  |
|-----------|---------|
|Public     |1.2.247  |
|预览体验成员    |1.2.247  |

## <a name="updates-for-version-12247"></a>针对版本 1.2.247 的更新

*发布日期：2019/09/17*

- 修复了在连接过程中进行身份验证时发生的崩溃问题。
- 修复了在关闭客户端时发生的崩溃问题。

## <a name="updates-for-version-12246"></a>针对版本 1.2.246 的更新

*发布日期：2019 年 8 月 28 日*

- 改进了本地化版本的回退语言。 （例如，FR-CA 将以法语正确显示，而不是以英语显示。）
- 删除订阅时，客户端现在会从凭据管理器中正确删除保存的凭据。
- 现在，客户端更新过程在启动后无人参与的情况下完成，并且将在完成后重新启动客户端。
- 现在可以在 Windows 10 上以 S 模式使用客户端。
- 修复了一个问题，对于用户名中有一个空格的用户，该问题会导致更新过程失败。

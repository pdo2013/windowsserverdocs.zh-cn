---
title: 设置
description: 了解 Windows 管理中心（Project Honolulu）中的设置。 用户设置允许用户更改其语言/区域和其他首选项。 网关设置允许管理员配置网关。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: e0fd6618f275058d4e22fe9abb9e484d4752ac9a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407053"
---
# <a name="windows-admin-center-settings"></a>Windows 管理中心设置

> 适用于：Windows Admin Center

Windows 管理中心设置由用户级和网关级设置组成。 对用户级设置的更改仅影响当前用户的配置文件，而对网关级别设置的更改会影响该 Windows 管理中心网关上的所有用户。

## <a name="user-settings"></a>用户设置

用户级设置由以下部分组成：

- 帐户
- 个性化
- 语言/区域
- 建议
- 高级

在 "**帐户**" 选项卡中，用户可以查看他们用于向 Windows 管理中心进行身份验证的凭据。 如果 Azure AD 配置为标识提供者，则用户可以从此选项卡中注销其 Azure AD 帐户。

在 "**个性化**" 选项卡中，用户可以切换到深色 UI 主题。

在 "**语言/区域**" 选项卡中，用户可以更改 Windows 管理中心显示的语言和区域格式。

在 "**建议**" 选项卡中，用户可以切换有关 Azure 服务和新功能的建议。

"**高级**" 选项卡为 Windows 管理中心扩展开发人员提供附加功能。

## <a name="gateway-settings"></a>网关设置

网关级设置由以下部分组成：

- Extensions
- 访问
- Azure
- 共享连接

只有网关管理员才能查看和更改这些设置。 更改这些设置将更改网关的配置，并影响 Windows 管理中心网关的所有用户。

在 "**扩展**" 选项卡中，管理员可以安装、卸载或更新网关扩展。 [了解有关扩展的详细信息。](using-extensions.md)

"**访问**" 选项卡允许管理员配置可以访问 Windows 管理中心网关的人员，以及用于对用户进行身份验证的标识提供者。 [了解有关控制网关访问权限的详细信息。](user-access-control.md)

在**azure**选项卡中，管理员可以向 azure 注册网关，以在 Windows 管理中心中启用[azure 集成功能](azure-integration.md)。

使用 "**共享连接**" 选项卡，管理员可以配置要在 Windows 管理中心网关的所有用户之间共享的单个连接列表。 [详细了解如何为网关的所有用户配置一次连接。](shared-connections.md)
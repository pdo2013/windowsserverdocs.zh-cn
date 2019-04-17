---
title: 设置
description: 了解 Windows Admin Center (Project Honolulu) 中的设置。 用户设置允许用户更改其语言/地区和其他首选项。 网关设置让管理员配置网关。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d67a2c743900792353141186112cd09dbf780309
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296619"
---
# Windows Admin Center 设置

> 适用于： Windows Admin Center

Windows Admin Center 设置包含用户级别和网关级别设置。 对用户级别设置的更改仅影响当前用户的配置文件，而对网关级别设置的更改将影响上的 Windows Admin Center 网关的所有用户。

## 用户设置

用户级别设置由以下部分组成：

- 帐户
- 个性化
- 语言/区域
- 建议
- 高级

在**帐户**选项卡，用户可以查看他们已用于进行身份验证到 Windows Admin Center 的凭据。 如果 Azure AD 配置为可标识提供程序，用户可以注销此选项卡上从其 Azure AD 帐户。

在**个性化**选项卡上，用户可以切换到深色 UI 主题。

在**语言/地区**选项卡，用户可以更改由 Windows Admin Center 显示的语言和区域格式。

在**建议**选项卡，用户可以切换有关 Azure 服务和新功能的建议。

**高级**选项卡为 Windows Admin Center 扩展开发人员提供了其他功能。

## 网关设置

网关级别的设置由以下部分组成：

- 扩展
- Access
- Azure
- 共享的连接

只有网关管理员将能够查看和更改这些设置。 这些设置的更改更改的网关的配置，并且会影响 Windows Admin Center 网关的所有用户。

在**扩展**选项卡中，管理员可以安装、 卸载或更新网关扩展。 [了解有关扩展的详细信息。](using-extensions.md)

**的访问权限**选项卡使管理员可以配置谁可以访问 Windows Admin Center 网关，以及用于对用户进行身份验证的标识提供程序。 [了解有关控制访问网关。](user-access-control.md)

从**Azure**选项卡，管理员可以注册与 Azure 启用[Azure 集成功能](azure-integration.md)在 Windows Admin Center 网关。

使用**共享连接**选项卡，管理员可以配置连接的 Windows Admin Center 网关的所有用户之间共享单个列表。 [了解有关配置网关连接一次为所有用户。](shared-connections.md)
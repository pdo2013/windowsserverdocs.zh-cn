---
title: 设置
description: 了解 Windows Admin Center (项目 Honolulu) 中的设置。 用户设置允许用户更改其语言/区域和其他首选项。 网关设置可让管理员配置网关。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 1e1231500733f70ddfcbd4f8a847047b73f24a00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882378"
---
# <a name="settings"></a>设置

> 适用于：Windows Admin Center

Windows Admin Center 设置包含用户级别和网关级别设置。 对用户级别设置的更改仅影响当前用户的配置文件，而对网关级别设置的更改会影响网关的 Windows Admin Center 上的所有用户。

## <a name="user-settings"></a>用户设置

用户级设置包括以下各节：

- 帐户
- 语言/区域
- 建议

在中**帐户**选项卡上，用户可以查看他们有用于进行身份验证到 Windows Admin Center 的凭据。 如果 Azure AD 配置为标识提供程序，用户可以注销 Azure AD 帐户在此选项卡。

在中**语言/区域**选项卡上，用户可以更改 Windows Admin Center 所显示的语言和区域格式。

在中**建议**选项卡上，用户可以切换有关 Azure 服务和新功能的建议。

### <a name="dark-theme"></a>深色主题

> 适用于：Windows Admin Center 预览版

在 Windows Admin Center 预览版中，可以解锁额外**个性化设置**部分中，其中包含以切换到深色 UI 主题的选项。 若要启用**个性化**部分中，输入```msft.sme.shell.personalization```作为试验的密钥。

>[!IMPORTANT]
> 深色主题是正在进行的工作，这一次，请执行不报告错误。

## <a name="gateway-settings"></a>网关设置

网关级别的设置包括以下各节：

- Extensions
- 访问
- Azure

仅网关管理员将能够查看和更改这些设置。 更改这些设置更改网关的配置，并且会影响 Windows Admin Center 网关的所有用户。

在中**扩展**选项卡上，管理员可以安装、 卸载或更新网关扩展。 [了解有关扩展的详细信息。](using-extensions.md)

**访问**选项卡允许管理员配置谁可以访问 Windows Admin Center 网关，以及用于对用户进行身份验证的标识提供程序。 [详细了解如何控制对网关的访问。](user-access-control.md)

从**Azure**选项卡上，管理员可以向注册网关使 Azure [Azure 集成功能](azure-integration.md)Windows Admin Center 中。
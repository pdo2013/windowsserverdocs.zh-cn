---
title: 配置 Azure 集成
description: 配置 Azure 集成 Windows 管理中心（Project Honolulu）。 将 Windows 管理中心网关连接到 Azure。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 448a8fb3e4340752b673b06f86d5d49211b6b147
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357359"
---
# <a name="configuring-azure-integration"></a>配置 Azure 集成

>适用于：Windows Admin Center、Windows Admin Center 预览版

Windows 管理中心支持与 Azure 服务集成的多项可选功能。 [了解 Windows 管理中心提供的 Azure 集成选项。](../plan/azure-integration-options.md)

允许 Windows 管理中心网关与 Azure 进行通信以利用 Azure Active Directory 的身份验证进行网关访问，或代表你创建 Azure 资源（例如，使用 Azure Site 保护 Windows 管理中心中管理的 Vm恢复），需要首先向 Azure 注册 Windows 管理中心网关。 你只需为 Windows 管理中心网关执行此操作一次，将网关更新到较新版本时，会保留此设置。

## <a name="register-your-gateway-with-azure"></a>向 Azure 注册你的网关

首次尝试使用 Windows 管理中心中的 Azure 集成功能时，系统将提示你将网关注册到 Azure。 你还可以通过转到 Windows 管理中心 "设置" 中的 " **Azure** " 选项卡来注册网关。

指导式产品内步骤会在你的目录中创建一个 Azure AD 应用，以便 Windows 管理中心能够与 Azure 通信。 若要查看自动创建的 Azure AD 应用，请参阅 Windows 管理中心设置的**Azure**选项卡。 " **Azure** " 超链接中的视图可让你查看 Azure 门户中的 Azure AD 应用。 

创建的 Azure AD 应用用于 Windows 管理中心中的所有 Azure 集成点，包括[网关 Azure AD 身份验证](../configure/user-access-control.md#azure-active-directory)。
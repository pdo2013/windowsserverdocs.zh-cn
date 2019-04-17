---
title: 配置 Azure 集成
description: 配置 Azure 集成 Windows Admin Center (Project Honolulu)。 连接到 Azure 在 Windows Admin Center 网关。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 38b973680463cdebc1b3168e447abfcf1caba6b5
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296878"
---
# 配置 Azure 集成

>适用于：Windows Admin Center、Windows Admin Center 预览版

Windows Admin Center 支持与 Azure 服务集成的多个可选功能。 [了解如何使用 Windows Admin Center 可用的 Azure 集成选项。](../plan/azure-integration-options.md)

若要允许与 Azure 利用 Azure Active Directory 身份验证的网关的访问权限，或创建 Azure 资源 （例如，若要保护虚拟机在 Windows Admin Center 中管理使用 Azure 站点你的名义进行通信的 Windows Admin Center 网关恢复），你将需要先通过 Azure 注册 Windows Admin Center 网关。 只需执行此操作后 Windows Admin Center 网关-当你更新到较新版本网关时保留设置。

## 通过 Azure 注册你的网关

第一次尝试在 Windows Admin Center 中使用 Azure 集成功能将提示你注册到 Azure 的网关。 此外可以通过转到在 Windows Admin Center 设置中的**Azure**选项卡中注册该网关。

指导的产品内步骤将在你允许 Windows Admin Center 进行通信使用 Azure 的目录中创建 Azure AD 应用。 若要查看自动创建 Azure AD 应用程序，请转到 Windows Admin Center 设置的**Azure**选项卡。 **在 Azure 中的视图**超链接，可以在 Azure 门户中查看 Azure AD 应用。 

Azure 集成 Windows Admin Center，包括[到网关的 Azure AD 身份验证](../configure/user-access-control.md#azure-active-directory)中的所有点可都用于创建的 Azure AD 应用。
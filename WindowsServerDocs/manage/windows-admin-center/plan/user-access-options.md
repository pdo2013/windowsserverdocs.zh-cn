---
title: 使用 Windows Admin Center 的用户访问选项
description: 用户访问选项和使用 Windows Admin Center (Project Honolulu) 的标识提供者
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9adea736d6e7ae181bdfe50289564083146f5b30
ms.sourcegitcommit: 4961576f2891600ef9a760ca7df650d14332e057
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2019
ms.locfileid: "9151992"
---
# 使用 Windows Admin Center 的用户访问选项

>适用于：Windows Admin Center、Windows Admin Center 预览版

当 Windows Server 上部署，Windows Admin Center 提供集中于服务器环境的管理点。 通过控制访问 Windows Admin Center，你可以改进管理环境的安全。

## 网关访问角色

Windows Admin Center 网关服务定义两个角色的访问权限： 网关用户和网关管理员。

> [!NOTE]
> 访问网关并不意味着由网关到目标服务器中可见的访问权限。 若要管理目标服务器，用户必须连接的目标服务器具有管理权限的凭据。

**网关用户**可以连接到 Windows Admin Center 网关服务，以管理服务器通过该网关，但不是能更改的访问权限，也不用于到网关身份验证的身份验证机制。

**网关管理员**可以配置谁可以访问权限以及如何为用户将进行身份验证到网关。

>[!NOTE]
> 如果没有在 Windows Admin Center 中定义的访问权限组，角色将反映网关服务器 Windows 帐户访问权限。 

[在 Windows Admin Center 中配置网关用户和管理员访问权限。](../configure/user-access-control.md)

## 标识提供程序选项

网关管理员可以选择以下任一：

 - [Active Directory/本地计算机组](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory 作为标识提供程序为 Windows Admin Center](../configure/user-access-control.md#azure-active-directory)


### 智能卡身份验证

当使用 Active Directory 或本地计算机组作为标识提供程序，你可以通过要求访问 Windows Admin Center 是其他基于智能卡的安全组的成员的用户在强制执行智能卡身份验证。 [在 Windows Admin Center 中配置智能卡身份验证。](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### 条件访问和多重身份验证

通过要求网关的 Azure AD 身份验证，你可以利用条件访问和 Azure ad 提供的多重身份验证等其他安全功能。 [了解有关配置与 Azure Active Directory 条件访问。](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## 基于角色的访问控制

默认情况下，用户需要在他们想要使用 Windows Admin Center 管理的计算机上的完整本地管理员权限。
这使它们可以远程连接到计算机，并确保它们有足够的权限，以查看和修改系统设置。
但是，某些用户可能不需要对计算机执行其作业无限制的访问。
你可以使用 Windows Admin Center 中**基于角色的访问控件**提供此类用户具有有限访问权限而不是使其完整本地管理员的计算机。

Windows Admin Center 中的基于角色的访问控制的工作原理是使用 PowerShell [Just Enough Administration](https://aka.ms/jeadocs)终结点配置每个托管的服务器。
此终结点定义角色，包括每个角色可以管理系统的哪些方面以及哪些用户分配角色。
当用户连接到受限制的终结点时，会创建临时的本地管理员帐户来管理代表他们系统。
这将确保，甚至不具有其自己的委派模型工具仍可以管理使用 Windows Admin Center。
当用户停止管理通过 Windows Admin Center 的计算机时，会自动删除临时帐户。

当用户连接到使用基于角色的访问控制配置的计算机时，Windows Admin Center 将首先检查它们是否本地管理员。
如果是这样，他们将收到了不受限制的完整 Windows Admin Center 体验。
否则，Windows Admin Center 将检查用户是否属于任何预定义的角色。
用户称为具有*有限的访问权限*，如果它们属于 Windows Admin Center 角色，但不是完整的管理员。
最后，如果用户是既不管理员，也不是角色的成员，它们将被拒绝访问管理计算机。

基于角色的访问控制是适用于服务器管理器和故障转移群集解决方案。

### 可用的角色

Windows Admin Center 支持以下最终用户角色：

角色名称 | 预期用途
----------|-------------
管理员 | 允许用户在 Windows Admin Center 中使用的大部分功能，而无需向其授予访问远程桌面或 PowerShell。 此角色非常适合想要限制在计算机上的管理入口点"跳转服务器"方案。
阅读器 | 允许用户在服务器上，查看信息和设置但不是进行更改。
HYPER-V 管理员 | 允许用户更改 HYPER-V 虚拟机和交换机，但限制为只读访问其他功能。

以下内置扩展降低了功能，当用户连接具有有限访问权限：

- 文件 （没有文件上载或下载）
- PowerShell （不可用）
- 远程桌面 （不可用）
- 存储副本 （不可用）

此时，你不能为你的组织创建自定义角色，但你可以选择哪些用户会被授予访问每个角色。

### 准备基于角色的访问控制

若要利用的临时的本地帐户，每台目标计算机需要配置以在 Windows Admin Center 支持基于角色的访问控制。
配置过程涉及到在使用所需状态配置的计算机上安装的 PowerShell 脚本和 Just Enough Administration 终结点。

如果只有几台计算机，你可以轻松地对应用配置单独使用的 Windows Admin Center 基于角色的访问控制页面每台计算机。
当你将在个别计算机上的基于角色的访问控制设置时，在创建本地安全组来控制对每个角色的访问。
通过将其添加为角色安全组的成员，可以向用户或其他安全组授予访问权限。

企业级部署多台计算机上，可以从该网关下载配置脚本并将其分配给你使用所需状态配置拉取服务器、 Azure 自动化或首选的管理工具的计算机。

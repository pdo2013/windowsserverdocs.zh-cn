---
title: 使用 Windows Admin Center 的用户访问选项
description: 用户访问选项和标识提供程序与 Windows Admin Center (项目 Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9adea736d6e7ae181bdfe50289564083146f5b30
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825888"
---
# <a name="user-access-options-with-windows-admin-center"></a>使用 Windows Admin Center 的用户访问选项

>适用于：Windows Admin Center，Windows Admin Center 预览版

部署 Windows Server 上，Windows Admin Center 提供了一个集中式的服务器环境的管理点。 通过控制对 Windows Admin Center 的访问，可以提高您管理环境的安全性。

## <a name="gateway-access-roles"></a>网关访问角色

Windows Admin Center 到网关服务定义两个角色的访问： 用户网关和网关管理员。

> [!NOTE]
> 到网关的访问权限并不表示网关访问可见的目标服务器。 若要管理的目标服务器，用户必须使用目标服务器具有管理权限的凭据连接。

**网关用户**可以连接到 Windows Admin Center 网关服务来管理服务器通过该网关，但它们不能更改访问权限，也不用于对网关进行身份验证的身份验证机制。

**网关管理员**可以配置谁获取访问权限以及如何向网关的用户将进行身份验证。

>[!NOTE]
> 如果没有 Windows Admin Center 中定义的访问组，这些角色将反映对网关服务器的 Windows 帐户访问权限。 

[在 Windows Admin Center 中配置网关用户和管理员访问权限。](../configure/user-access-control.md)

## <a name="identity-provider-options"></a>标识提供程序选项

网关管理员可以选择以下之一：

 - [Active Directory / 本地计算机组](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory 作为 Windows Admin Center 的标识提供程序](../configure/user-access-control.md#azure-active-directory)


### <a name="smartcard-authentication"></a>智能卡身份验证

在为标识提供者使用 Active Directory 或本地计算机组，可以通过要求用户访问 Windows Admin Center 是其他基于智能卡的安全组的成员的用户实施智能卡身份验证。 [在 Windows Admin Center 中配置智能卡身份验证。](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### <a name="conditional-access-and-multi-factor-authentication"></a>条件性访问和多重身份验证

通过网关要求 Azure AD 身份验证，您可以利用其他安全功能，如条件性访问和 Azure AD 提供的多重身份验证。 [了解有关使用 Azure Active Directory 中配置条件性访问的详细信息。](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="role-based-access-control"></a>基于角色的访问控制

默认情况下，用户需要他们想要使用 Windows Admin Center 进行管理的计算机上的完整的本地管理员权限。
这使它们能够远程连接到计算机，并确保它们具有足够的权限查看和修改系统设置。
但是，某些用户可能不需要对计算机执行其作业无限制的访问。
可以使用**基于角色的访问控制**Windows Admin Center 这类用户提供受限访问权限而不是使其完整的本地管理员在计算机中。

Windows Admin Center 中的基于角色的访问控制的工作原理是使用 PowerShell 配置每个托管的服务器[Just Enough Administration](https://aka.ms/jeadocs)终结点。
此终结点定义角色，包括每个角色可以管理系统的哪些方面以及哪些用户分配给角色。
当用户连接到受限制的终结点时，创建一个临时的本地管理员帐户来管理代表系统。
这可确保，即使工具不具有其自己的委派模型仍可管理与 Windows Admin Center。
当用户停止管理通过 Windows Admin Center 的计算机时，会自动删除临时帐户。

当用户连接到配置有基于角色的访问控制的计算机时，Windows Admin Center 将首先检查它们是否是本地管理员。
如果是，他们将收到不带任何限制的完整 Windows Admin Center 体验。
否则，Windows Admin Center 将检查用户是否属于任何预定义角色。
用户被称为能够*有限的访问权限*如果它们属于 Windows Admin Center 角色，但不是完整的管理员。
最后，如果用户是管理员既不是角色的成员，它们将被拒绝访问，可以管理计算机。

基于角色的访问控制是适用于服务器管理器和故障转移群集解决方案。

### <a name="available-roles"></a>可用角色

Windows Admin Center 支持以下最终用户角色：

角色名称 | 预期用途
----------|-------------
Administrators | 允许用户在 Windows Admin Center 中使用的大多数功能，而无需授予其访问远程桌面或 PowerShell。 此角色是适用于想要限制在计算机上的管理入口点的"跳转服务器"方案。
Readers | 允许用户在服务器上，查看信息和设置，但不是能进行更改。
Hyper-V 管理员 | 允许用户对 HYPER-V 虚拟机和交换机，进行更改，但限制为只读访问权限的其他功能。

当用户使用有限的访问权限，功能有所削减以下内置扩展：

- 文件 （无文件上传或下载）
- PowerShell （不可用）
- 远程桌面 （不可用）
- 存储副本 （不可用）

在此期间，不能为您的组织创建自定义角色，但您可以选择哪些用户有权访问每个角色。

### <a name="preparing-for-role-based-access-control"></a>准备用于基于角色的访问控制

若要利用临时的本地帐户，每个目标计算机必须配置为在 Windows Admin Center 中支持基于角色的访问控制。
配置过程涉及使用 Desired State Configuration 在计算机上安装 PowerShell 脚本和 Just Enough Administration 终结点。

如果仅有的几台计算机，你可以轻松地配置单独应用到每台计算机在 Windows Admin Center 中使用基于角色的访问控制页。
如果设置了基于角色的访问控制的单台计算机上时，创建本地安全组来控制对每个角色的访问。
可以将其作为角色安全组的成员添加到用户或其他安全组授予访问权限。

对于多台计算机上的企业级部署，可以从网关下载配置脚本，并将其分发给你的计算机使用 Desired State Configuration 请求服务器、 Azure 自动化中或你首选的管理工具。

---
title: 使用 Windows 管理中心的用户访问选项
description: 使用 Windows 管理中心的用户访问选项和标识提供程序（Project Honolulu）
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 084cdae0bf8ca0eb3aff1f4679d30978b860efef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356915"
---
# <a name="user-access-options-with-windows-admin-center"></a>使用 Windows 管理中心的用户访问选项

>适用于：Windows Admin Center、Windows Admin Center 预览版

在 Windows Server 上部署时，Windows 管理中心为您的服务器环境提供了集中管理点。 通过控制对 Windows 管理中心的访问，你可以提高管理环境的安全性。

## <a name="gateway-access-roles"></a>网关访问角色

Windows 管理中心定义了两个用于访问网关服务的角色：网关用户和网关管理员。

> [!NOTE]
> 访问网关并不意味着可以访问网关可见的目标服务器。 若要管理目标服务器，用户必须与在目标服务器上具有管理权限的凭据连接。

**网关用户**可以连接到 Windows 管理中心的网关服务，以便通过该网关管理服务器，但不能更改访问权限，也不能更改用于向网关进行身份验证的身份验证机制。

**网关管理员**可以配置谁可以访问以及用户将如何向网关进行身份验证。

>[!NOTE]
> 如果在 Windows 管理中心没有定义访问组，则这些角色将反映 Windows 帐户对网关服务器的访问权限。 

[在 Windows 管理中心中配置网关用户和管理员访问权限。](../configure/user-access-control.md)

## <a name="identity-provider-options"></a>标识提供程序选项

网关管理员可以选择以下任一项：

 - [Active Directory/本地计算机组](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory 为 Windows 管理中心的标识提供者](../configure/user-access-control.md#azure-active-directory)


### <a name="smartcard-authentication"></a>智能卡身份验证

使用 Active Directory 或本地计算机组作为标识提供者时，可以通过要求访问 Windows 管理中心的用户成为其他基于智能卡的安全组的成员，来强制执行智能卡身份验证。 [在 Windows 管理中心中配置智能卡身份验证。](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### <a name="conditional-access-and-multi-factor-authentication"></a>条件性访问和多重身份验证

通过要求网关 Azure AD 身份验证，你可以利用 Azure AD 提供的其他安全功能，例如条件访问和多重身份验证。 [详细了解如何使用 Azure Active Directory 配置条件访问。](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="role-based-access-control"></a>基于角色的访问控制

默认情况下，用户需要使用 Windows 管理中心对他们要管理的计算机拥有完全的本地管理员权限。
这使它们可以远程连接到计算机，并确保它们具有足够的权限来查看和修改系统设置。
但是，某些用户可能无需对计算机进行不受限制的访问权限即可执行其作业。
您可以在 Windows 管理中心中使用**基于角色的访问控制**，为此类用户提供对计算机的有限访问权限，而不是使其完全成为本地管理员。

Windows 管理中心中基于角色的访问控制的工作原理是使用 PowerShell 的[足够管理](https://aka.ms/jeadocs)终结点配置每个托管服务器。
此终结点定义角色，其中包括允许每个角色管理的系统的哪些方面，以及分配给该角色的用户。
当用户连接到受限制的终结点时，将创建一个临时的本地管理员帐户以代表他们管理系统。
这可确保即使是没有自己的委派模型的工具仍可以使用 Windows 管理中心进行管理。
当用户通过 Windows 管理中心停止管理计算机时，会自动删除临时帐户。

当用户连接到配置了基于角色的访问控制的计算机时，Windows 管理中心将首先检查它们是否是本地管理员。
如果他们是，他们将收到 Windows 管理中心中心的完整体验，但没有任何限制。
否则，Windows 管理中心将检查用户是否属于任何预定义的角色。
如果用户属于 Windows 管理中心角色但不是完全管理员，则认为该用户具有*受限的访问权限*。
最后，如果用户既不是管理员也不是角色的成员，则将拒绝他们对计算机的管理访问权限。

基于角色的访问控制可用于服务器管理器和故障转移群集解决方案。

### <a name="available-roles"></a>可用角色

Windows 管理中心支持以下最终用户角色：

角色名称 | 预期用途
----------|-------------
Administrators | 允许用户使用 Windows 管理中心中的大部分功能，而无需向其授予对远程桌面或 PowerShell 的访问权限。 此角色适用于希望限制计算机上的管理入口点的 "跳转服务器" 方案。
Readers | 允许用户查看服务器上的信息和设置，但不允许进行更改。
Hyper-V 管理员 | 允许用户更改 Hyper-v 虚拟机和交换机，但将其他功能限制为只读访问。

当用户使用受限访问权限进行连接时，以下内置扩展功能降低了功能：

- 文件（无文件上传或下载）
- PowerShell （不可用）
- 远程桌面（不可用）
- 存储副本（不可用）

此时，你无法为你的组织创建自定义角色，但你可以选择授予哪些用户访问每个角色的权限。

### <a name="preparing-for-role-based-access-control"></a>准备基于角色的访问控制

若要利用临时本地帐户，需要将每个目标计算机配置为支持 Windows 管理中心中基于角色的访问控制。
此配置过程涉及到在使用 Desired State Configuration 的计算机上安装 PowerShell 脚本和一个刚好足够的管理终结点。

如果只有几台计算机，则可以使用 Windows 管理中心中的基于角色的访问控制页，轻松地将配置单独应用到每台计算机。
在单个计算机上设置基于角色的访问控制时，会创建本地安全组来控制对每个角色的访问。
可以通过将用户或其他安全组添加为角色安全组的成员来授予对它们的访问权限。

对于在多台计算机上进行企业范围的部署，可以从网关上下载配置脚本，并使用所需状态配置请求服务器、Azure 自动化或你喜欢的管理工具将其分发到计算机。

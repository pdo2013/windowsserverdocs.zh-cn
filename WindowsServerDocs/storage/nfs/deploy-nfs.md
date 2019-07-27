---
title: 部署网络文件系统
description: 描述如何部署网络文件系统。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: cc02f0a82b4143b80fc1107a63d234b117502d2d
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544651"
---
# <a name="deploy-network-file-system"></a>部署网络文件系统

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

网络文件系统 (NFS) 提供了一个文件共享解决方案, 可让你使用 NFS 协议在运行 Windows Server 和 UNIX 操作系统的计算机之间传输文件。 本主题介绍部署 NFS 应遵循的步骤。

## <a name="whats-new-in-network-file-system"></a>网络文件系统的新增功能

Windows Server 2012 中 NFS 的是更改:

- **对 NFS 版本4.1 的支持**。 此协议版本包括以下增强功能。
  - 导航防火墙更容易, 从而提高了可访问性。
  - 支持 RPCSEC\_GSS 协议, 提供更强的安全性, 并允许客户端和服务器协商安全性。
  - 支持 UNIX 和 Windows 文件语义。
  - 利用群集文件服务器部署。
  - 支持 WAN 友好复合过程。

- **适用于 Windows PowerShell 的 NFS 模块**。 利用内置 NFS cmdlet, 可以更轻松地自动执行各种操作。 Cmdlet 名称与其他 Windows PowerShell cmdlet 一致 (使用如 "Get" 和 "Set" 这样的动词), 使得熟悉 Windows PowerShell 的用户可以更轻松地了解如何使用新的 cmdlet。
- **NFS 管理改进**。 新的集中式基于 UI 的管理控制台除了管理群集文件服务器外, 还简化了 SMB 和 NFS 共享、配额、文件屏蔽和分类的配置和管理。
- **标识映射改进**。 新的 UI 支持和基于任务的 Windows PowerShell cmdlet, 用于配置标识映射, 使管理员能够快速配置标识映射源, 并为用户创建单独的映射标识。 改进使管理员能够轻松地为 NFS 和 SMB 的多协议访问设置共享。
- **群集资源模型重组**。 此改进为 Windows NFS 和 SMB 协议服务器的群集资源模型提供了一致性, 并简化了管理。 对于具有多个共享的 NFS 服务器, 减少了需要对包含大量 NFS 共享的卷进行故障转移的资源网络和 WMI 调用次数。
- **与恢复密钥管理器集成**。 恢复密钥管理器是一个组件, 用于跟踪文件服务器和文件系统的状态, 并使 Windows SMB 和 NFS 协议服务器能够故障转移, 而不会中断在文件服务器上存储其数据的客户端或服务器应用程序。 此改进是运行 Windows Server 2012 的文件服务器的连续可用性功能的关键组件。

## <a name="scenarios-for-using-network-file-system"></a>使用网络文件系统的方案

NFS 支持基于 Windows 和基于 UNIX 的操作系统的混合环境。 以下部署方案举例说明了如何使用 NFS 部署持续可用的 Windows Server 2012 文件服务器。

### <a name="provision-file-shares-in-heterogeneous-environments"></a>在异类环境中预配文件共享

此方案适用于具有异类环境 (包括 Windows 和其他操作系统, 如基于 UNIX 或基于 Linux 的客户端计算机) 的组织。 在这种情况下, 可以通过 SMB 和 NFS 协议提供对同一文件共享的多协议访问。 通常, 在这种情况下, 当你部署 Windows 文件服务器时, 你希望在基于 Windows 和 UNIX 的计算机上的用户之间实现协作。 当配置文件共享时, 它会与 SMB 和 NFS 协议共享, Windows 用户通过 SMB 协议访问其文件, 并且基于 UNIX 的计算机上的用户通常通过 NFS 协议访问其文件。

对于这种情况, 必须具有有效的标识映射源配置。 Windows Server 2012 支持以下标识映射存储:

- 映射文件
- Active Directory 域服务 (AD DS)
- 符合 RFC 2307 的 LDAP 存储, 如 Active Directory 轻型目录服务 (AD LDS)
- 用户名映射 (UNM) 服务器

### <a name="provision-file-shares-in-unix-based-environments"></a>在基于 UNIX 的环境中预配文件共享

在此方案中, Windows 文件服务器部署在基于 UNIX 的主要环境中, 以便为基于 UNIX 的客户端计算机提供对 NFS 文件共享的访问权限。 最初为 Windows Server 2008 R2 中的 NFS 共享实现了未映射的 UNIX 用户访问 (UUUA) 选项, 以便可以使用 Windows 服务器来存储 NFS 数据, 而无需创建 UNIX 到 Windows 的帐户映射。 UUUA 允许管理员快速设置和部署 NFS, 无需配置帐户映射。 为 NFS 启用时, UUUA 会创建自定义安全标识符 (Sid) 来表示未映射的用户。 映射的用户帐户使用标准 Windows 安全标识符 (Sid), 未映射的用户使用自定义 NFS Sid。

## <a name="system-requirements"></a>系统要求

NFS 服务器可以安装在任何版本的 Windows Server 2012 上。 如果 nfs 服务器和客户端实现符合以下协议规范之一, 你可以将 NFS 与运行 NFS 服务器或 NFS 客户端的基于 UNIX 的计算机配合使用:

1. NFS 版本4.1 协议规范 (如 RFC [5661](https://tools.ietf.org/html/rfc5661)中所定义)
2. NFS 版本3协议规范 (如 RFC [1813](https://tools.ietf.org/html/rfc1813)中所定义)
3. NFS 版本2协议规范 (如 RFC [1094](https://tools.ietf.org/html/rfc1094)中所定义)

## <a name="deploy-nfs-infrastructure"></a>部署 NFS 基础结构

你需要部署以下计算机并将其连接到局域网 (LAN):

- 一台或多台运行 Windows Server 2012 的计算机, 你将在该计算机上安装两个主要的 NFS 服务组件:NFS 服务器和 NFS 客户端。 可以在同一台计算机或不同的计算机上安装这些组件。
- 运行 NFS 服务器和 NFS 客户端软件的一个或多个基于 UNIX 的计算机。 运行 NFS 服务器的基于 UNIX 的计算机承载了一个 NFS 文件共享或导出, 该计算机由运行 Windows Server 2012 的计算机作为使用客户端 NFS 的客户端进行访问。 你可以根据需要在相同的基于 UNIX 的计算机或基于 UNIX 的不同计算机上安装 NFS 服务器和客户端软件。
- 在 Windows Server 2008 R2 功能级别运行的域控制器。 域控制器为 Windows 环境提供用户身份验证信息和映射。
- 如果未部署域控制器, 则可以使用网络信息服务 (NIS) 服务器为 UNIX 环境提供用户身份验证信息。 或者, 如果你愿意, 可以使用存储在运行用户名映射服务的计算机上的密码和组文件。

### <a name="install-network-file-system-on-the-server-with-server-manager"></a>通过服务器管理器在服务器上安装网络文件系统

1. 从“添加角色和功能向导”中，在“服务器角色”下，选择“文件和存储服务”  （如果尚未安装）。
2. 在 "**文件和 ISCSI 服务**" 下, 选择 "**文件服务器**" 和 " **NFS 服务器**"。 选择 "**添加功能**", 包括选定的 NFS 功能。
3. 选择 "**安装**" 以在服务器上安装 NFS 组件。

### <a name="install-network-file-system-on-the-server-with-windows-powershell"></a>通过 Windows PowerShell 在服务器上安装网络文件系统

1. 启动 Windows PowerShell。 在任务栏上，右键单击 PowerShell 图标，然后选择“以管理员身份运行” 。
2. 运行以下 Windows PowerShell 命令：

```PowerShell
Import-Module ServerManager
Add-WindowsFeature FS-NFS-Service
Import-Module NFS
```

## <a name="configure-nfs-authentication"></a>配置 NFS 身份验证

使用 NFS 版本4.1 和 NFS 版本3.0 协议时, 可以使用以下身份验证和安全选项。

- RPCSEC\_GSS
  - **Krb5**。 在授予对文件共享的访问权限之前, 使用 Kerberos 版本5协议对用户进行身份验证。
  - **Krb5i**。 使用 Kerberos 版本5协议通过完整性检查 (校验和) 进行身份验证, 这将验证数据是否未更改。
  - **Krb5p**使用 Kerberos 版本5协议, 该协议通过加密来对 NFS 流量进行身份验证。
- AUTH\_SYS

你还可以选择不使用服务器授权 (AUTH\_SYS), 这将为你提供启用未映射的用户访问的选项。 使用未映射的用户访问权限时, 你可以指定, 以允许 UID/GID 取消映射的用户访问 (这是默认设置), 也可以允许匿名访问。

有关配置 NFS 身份验证的说明, 请参见下一节。

## <a name="create-an-nfs-file-share"></a>创建 NFS 文件共享

可以使用服务器管理器或 Windows PowerShell NFS cmdlet 创建 NFS 文件共享。

### <a name="create-an-nfs-file-share-with-server-manager"></a>使用服务器管理器创建 NFS 文件共享

1. 以本地管理员用户组成员之一的身份登录服务器。
2. 服务器管理器会自动打开。 如果它未自动启动, 请选择 "**开始**", 键入 " **servermanager**", 然后选择 "**服务器管理器**"。
3. 在左侧, 选择 "**文件和存储服务**", 然后选择 "**共享**"。
4. 选择**要创建文件共享, 请启动新建共享向导**。
5. 在 "**选择配置文件**" 页上, 选择 " **NFS 共享–快速**或**nfs 共享-高级**", 然后选择 "**下一步**"。
6. 在 "**共享位置**" 页上, 选择服务器和卷, 然后选择 "**下一步**"。
7. 在 "**共享名**" 页上, 指定新共享的名称, 然后选择 "**下一步**"。
8. 在 "**身份验证**" 页上, 指定要用于此共享的身份验证方法。
9. 在 "**共享权限**" 页面上, 选择 "**添加**", 然后指定要向其授予共享权限的主机、客户端组或网络组。
10. 在 "**权限**" 中, 配置用户要拥有的访问控制的类型, 然后选择 **"确定"** 。
11. 在 "**确认**" 页上, 查看配置, 然后选择 "**创建**" 以创建 NFS 文件共享。

### <a name="windows-powershell-equivalent-commands"></a>Windows PowerShell 等效命令

下面的 Windows PowerShell cmdlet 还可以创建 NFS 文件共享 (其中`nfs1` , 是共享的名称, `C:\\shares\\nfsfolder`是文件路径):

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### <a name="known-issue"></a>已知问题
NFS 版本4.1 允许使用非法字符创建或复制文件名。 如果尝试用 vi 编辑器打开文件, 则它会显示为已损坏。 不能从 vi、rename、move 或 change 权限保存该文件。 避免使用非法字符。

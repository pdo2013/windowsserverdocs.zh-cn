---
title: 部署网络文件系统
description: 介绍如何部署网络文件系统。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2f3671283720d515cd3e3e609d98e02343c15892
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860768"
---
# <a name="deploy-network-file-system"></a>部署网络文件系统

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

网络文件系统 (NFS) 提供了一种文件共享解决方案，允许您运行 Windows Server 和使用 NFS 协议的 UNIX 操作系统的计算机之间传输文件。 本主题介绍部署 NFS 应遵循的步骤。

## <a name="whats-new-in-network-file-system"></a>什么是网络文件系统中的新增功能

下面是 Windows Server 2012 中的 nfs 更改的内容：

- **支持 nfs 版本 4.1**。 此协议版本包括以下增强功能。
  - 导航防火墙会更方便，提高可访问性。
  - 支持 RPCSEC\_GSS 协议，提供更强大的安全性，并允许客户端和服务器协商安全。
  - 支持 UNIX 和 Windows 文件语义。
  - 充分利用群集的文件服务器部署。
  - 支持 WAN 友好复合过程。

- **Windows PowerShell 的 NFS 模块**。 内置的 NFS cmdlet 的可用性使其更轻松地自动执行各种操作。 Cmdlet 名称是一致的与其他 Windows PowerShell cmdlet （使用动词，如"Get"和"Set"），使其更容易为用户熟悉 Windows PowerShell，若要了解如何使用新 cmdlet。
- **NFS 管理改进**。 新的集中式基于 UI 的管理控制台可简化配置和管理 SMB 和 NFS 共享，配额、 文件屏蔽和分类，除了管理群集的文件服务器。
- **标识映射改进**。 新的 UI 支持和基于任务的 Windows PowerShell cmdlet，用于配置标识映射，这样管理员就能快速配置标识映射源，并创建单个映射的用户身份。 改进使管理员方便地对 NFS 和 SMB 设置对多协议访问的共享。
- **群集资源模型重组**。 这一改进引入了 Windows NFS 群集资源模型和 SMB 协议服务器之间的一致性和简化了管理。 对于拥有多个共享的 NFS 服务器，资源网络所需的 WMI 调用数故障转移一个卷，其中包含大量的 NFS 共享减少。
- **与恢复密钥管理器集成**。 恢复密钥管理器是一个组件，用于跟踪文件服务器和文件系统状态并使 Windows SMB 和 NFS 协议服务器进行故障转移而不会中断客户端或将其数据存储在文件服务器的服务器应用程序。 此项改进是运行 Windows Server 2012 的文件服务器的连续可用性功能的关键组件。

## <a name="scenarios-for-using-network-file-system"></a>使用网络文件系统的方案

NFS 支持基于 Windows 的和基于 UNIX 的操作系统的混合的环境。 在以下部署方案是如何部署使用 NFS 的连续可用的 Windows Server 2012 文件服务器的示例。

### <a name="provision-file-shares-in-heterogeneous-environments"></a>在异类环境中预配文件共享

此方案适用于 Windows 和其他操作系统，例如 UNIX 或基于 Linux 的客户端组成的异类环境的组织的计算机。 使用此方案中，可以通过 SMB 和 NFS 协议来提供多协议访问相同的文件共享。 通常情况下，在部署此方案中的 Windows 文件服务器时，你想要简化 Windows 上的用户和基于 UNIX 的计算机之间的协作。 配置文件共享后，使用 SMB 和 NFS 协议时，与 Windows 用户访问其文件通过 SMB 协议共享和基于 UNIX 的计算机上的用户通常通过 NFS 协议访问他们的文件。

对于此方案，必须具有有效的标识映射源配置。 Windows Server 2012 支持以下标识映射存储：

- 映射文件
- Active Directory 域服务 (AD DS)
- RFC 2307 符合 LDAP 存储如 Active Directory 轻型目录服务 (AD LDS)
- 用户名称映射 (UNM) 服务器

### <a name="provision-file-shares-in-unix-based-environments"></a>在基于 UNIX 的环境中预配文件共享

在此方案中，Windows 文件服务器部署在主要基于 UNIX 的环境中为基于 UNIX 的客户端计算机提供对 NFS 文件共享的访问。 未映射的 UNIX 用户访问 (UUUA) 选项是最初为实现的 Windows Server 2008 R2 中的 NFS 共享，以便服务器可以使用 NFS 数据存储而无需创建 UNIX 到 Windows 的 Windows 帐户的映射。 UUUA 允许管理员快速预配和部署而无需配置帐户映射的 NFS。 启用 nfs，UUUA 创建自定义安全标识符 (Sid) 来表示未映射的用户。 映射的用户帐户使用标准 Windows 安全标识符 (Sid) 和未映射的用户使用自定义 NFS Sid。

## <a name="system-requirements"></a>系统要求

可以在任何版本的 Windows Server 2012 上安装 nfs 服务器。 您可以使用 NFS 与基于 UNIX 的计算机正在运行的 NFS 服务器或 NFS 客户端如果这些 NFS 服务器和客户端实现符合以下协议规范之一：

1. NFS 版本 4.1 协议规范 (RFC 中定义[5661](https://tools.ietf.org/html/rfc5661))
2. NFS 版本 3 协议规范 (RFC 中定义[1813年](https://tools.ietf.org/html/rfc1813))
3. NFS 版本 2 协议规范 (RFC 中定义[1094年](https://tools.ietf.org/html/rfc1094))

## <a name="deploy-nfs-infrastructure"></a>部署 NFS 基础结构

需要部署以下计算机并将它们连接局域网 (LAN) 上：

- 运行 Windows Server 2012 将其安装 NFS 组件的两个主要服务的一个或多个计算机：NFS 服务器和 nfs 客户端。 您可以在同一计算机上或不同计算机上安装这些组件。
- 一个或多个基于 UNIX 的计算机运行 NFS 服务器和 NFS 客户端软件。 正在运行的 NFS 服务器的基于 UNIX 的计算机托管 NFS 文件共享或导出功能，可通过使用 NFS 客户端的客户端运行 Windows Server 2012 的计算机。 您可以在同一个基于 UNIX 的计算机或不同的基于 UNIX 的计算机，根据需要上安装 NFS 服务器和客户端软件。
- 在 Windows Server 2008 R2 功能级别运行的域控制器。 域控制器提供用户身份验证信息和 Windows 环境的映射。
- 未部署域控制器时，可以使用网络信息服务 (NIS) 服务器提供 UNIX 环境的用户身份验证信息。 或者，如果您愿意，可以使用存储在运行用户名映射服务的计算机的密码和组文件。

### <a name="install-network-file-system-on-the-server-with-server-manager"></a>使用服务器管理器在服务器上安装网络文件系统

1. 从“添加角色和功能向导”中，在“服务器角色”下，选择“文件和存储服务”  （如果尚未安装）。
2. 下**文件和 iSCSI 服务**，选择**文件服务器**并**nfs 服务器**。 选择**添加功能**以包含所选的 NFS 功能。
3. 选择**安装**若要在服务器上安装 NFS 组件。

### <a name="install-network-file-system-on-the-server-with-windows-powershell"></a>使用 Windows PowerShell 在服务器上安装网络文件系统

1. 启动 Windows PowerShell。 在任务栏上，右键单击 PowerShell 图标，然后选择“以管理员身份运行” 。
2. 运行以下 Windows PowerShell 命令：

```PowerShell
Import-Module ServerManager
Add-WindowsFeature FS-NFS-Service
Import-Module NFS
```

## <a name="configure-nfs-authentication"></a>配置 NFS 身份验证

使用 NFS 版本 4.1 和 NFS 版本 3.0 协议时，您将使用以下身份验证和安全选项。

- RPCSEC\_GSS
  - **Krb5**。 使用 Kerberos 版本 5 协议来授予访问权限的文件共享之前对用户进行身份验证。
  - **Krb5i**。 使用 Kerberos 版本 5 协议进行完整性检查 （校验和），身份验证用于验证数据未更改。
  - **Krb5p**使用 Kerberos 版本 5 协议，对 NFS 通信进行有关隐私加密进行身份验证。
- 身份验证\_SYS

您还可以选择不应使用服务器授权 (身份验证\_SYS)，后者可提供用于启用未映射的用户访问的选项。 使用未映射的用户访问时，你可以指定允许未映射的用户访问通过 UID / GID，这是默认设置，或允许匿名访问。

以下部分中讨论配置上的 NFS 身份验证的说明。

## <a name="create-an-nfs-file-share"></a>创建 NFS 文件共享

可以创建 NFS 文件共享使用服务器管理器或 Windows PowerShell NFS cmdlet。

### <a name="create-an-nfs-file-share-with-server-manager"></a>使用服务器管理器创建 NFS 文件共享

1. 以本地管理员用户组成员之一的身份登录服务器。
2. 服务器管理器会自动打开。 如果自动启动，则选择**启动**，类型**servermanager.exe**，然后选择**服务器管理器**。
3. 在左侧，选择**文件和存储服务**，然后选择**共享**。
4. 选择**若要创建文件共享，启动新建共享向导**。
5. 上**选择配置文件**页上，选择**NFS 共享-快速**或**NFS 共享 – 高级**，然后选择**下一步**。
6. 上**共享位置**页上，选择服务器和卷，然后选择**下一步**。
7. 上**共享名**页上，指定的新共享的名称并选择**下一步**。
8. 上**身份验证**页上，指定想要用于此共享的身份验证方法。
9. 上**共享权限**页上，选择**添加**，然后指定主机、 客户端组或网络组你想要授予对共享的权限。
10. 在中**权限**，配置所需的用户，并选择访问控制类型**确定**。
11. 上**确认**页上，查看您的配置，然后选择**创建**创建 NFS 文件共享。

### <a name="windows-powershell-equivalent-commands"></a>Windows PowerShell 等效命令

以下 Windows PowerShell cmdlet 还可以创建 NFS 文件共享 (其中`nfs1`是共享的名称和`C:\\shares\\nfsfolder`是文件路径):

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### <a name="known-issue"></a>已知问题
NFS 版本 4.1 允许文件名为创建或复制使用非法字符。 如果尝试使用 vi 编辑器打开文件，它将显示为已损坏。 不能将文件保存从 vi、 重命名、 将其移动或更改权限。 避免使用 illigal 字符。

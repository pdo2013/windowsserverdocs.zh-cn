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
ms.sourcegitcommit: fcc26ec5a2cc73b59c5752377b39c070d288655e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/19/2018
ms.locfileid: "8976683"
---
# 部署网络文件系统

>适用于： Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

网络文件系统 (NFS) 提供了一种文件共享解决方案，可运行 Windows Server 和使用 NFS 协议 UNIX 操作系统的计算机之间传输文件。 本主题介绍了部署 NFS 应遵循的步骤。

## 什么是网络文件系统中的新增功能

下面是 Windows Server 2012 中的 nfs 更改的内容：

- **Nfs 版本 4.1 的支持**。 此协议版本包括以下增强功能。
  - 导航防火墙就很容易，提高辅助功能。
  - 支持 RPCSEC\_GSS 协议，提供更强大的安全性，并允许客户端和服务器协商安全。
  - 支持 UNIX 和 Windows 文件语义。
  - 充分利用群集的文件服务器部署。
  - 支持 WAN 友好复合过程。

- **NFS 适用于 Windows PowerShell 的模块**。 内置 NFS cmdlet 的可用性容易地自动执行各种操作。 这些 cmdlet 名称不与其他 Windows PowerShell cmdlet （使用动词命令，如"获取"和"设置"），使其更轻松的用户熟悉 Windows PowerShell，若要了解如何使用新的 cmdlet 一致。
- **NFS 管理改进**。 新的基于 UI 进行集中式的管理控制台简化的 SMB 和 NFS 共享、 配额、 文件屏蔽和分类，除了管理群集的文件服务器配置和管理。
- **标识映射改进**。 新的 UI 支持和配置标识映射，使管理员能够快速配置标识映射源，然后创建的用户的各个映射的标识基于任务的 Windows PowerShell cmdlet。 改进轻松管理员能够通过 NFS 和 SMB 多协议访问的共享设置。
- **群集资源模型重构**。 这一改进带来了 Windows NFS 群集资源模型和 SMB 协议服务器之间的一致性，并简化管理。 对于具有多个共享的 NFS 服务器，资源网络的所需的 WMI 调用次数故障转移包含减少了大量 NFS 共享的卷。
- **恢复密钥管理器集成**。 恢复密钥管理器是一个组件，跟踪文件服务器和文件系统状态并允许 Windows SMB 和 NFS 协议服务器而不影响客户端或服务器应用程序数据存储在文件服务器故障转移。 这一改进是运行 Windows Server 2012 的文件服务器的连续可用性功能的关键组件。

## 使用网络文件系统的方案

NFS 支持基于 Windows 和基于 UNIX 操作系统在混合的环境。 以下部署方案是如何部署使用 NFS 持续可用的 Windows Server 2012 文件服务器的示例。

### 异类环境中的预配文件共享

此方案中适用于 Windows 和其他操作系统，如 UNIX 或基于 Linux 的客户端组成的异类环境使用的组织的计算机。 使用此方案中，你可以通过 SMB 和 NFS 协议提供多协议访问相同的文件共享。 通常情况下，当你部署 Windows 文件服务器在此方案中，你想要促进 Windows 上的用户和基于 UNIX 计算机之间的协作。 配置文件共享时，与 SMB 和 NFS 协议，与 Windows 用户通过 SMB 协议，访问其文件共享和基于 UNIX 的计算机上的用户通常通过 NFS 协议访问的文件。

对于此方案中，你必须具有有效的标识映射源配置。 Windows Server 2012 支持以下身份映射存储：

- 映射文件
- Active Directory 域服务 (AD DS)
- RFC 2307 符合 LDAP 存储如 Active Directory 轻型目录服务 (AD LDS)
- 用户名称映射 (UNM) 服务器

### 基于 UNIX 环境中的预配文件共享

在此方案中，Windows 文件服务器部署在主要基于 UNIX 环境，基于 UNIX 的客户端计算机提供 NFS 文件共享的访问权限。 以便 Windows 服务器可以用于存储 NFS 数据，而无需创建 UNIX 到 Windows 帐户映射，最初是为 Windows Server 2008 R2 中的 NFS 共享实现一个未映射 UNIX 用户访问权限 (UUUA) 选项。 UUUA 使管理员能够快速预配和部署 NFS 而无需配置帐户映射。 启用时 nfs，UUUA 创建自定义的安全标识符 (Sid) 来表示未映射的用户。 映射的用户帐户使用标准 Windows 安全标识符 (Sid) 和未映射的用户使用自定义 NFS Sid。

## 系统要求

NFS 服务器可以安装在 Windows Server 2012 的任何版本。 NFS 可用于基于 UNIX 的计算机运行的 NFS 服务器或 NFS 客户端如果这些 NFS 服务器和客户端实现遵循以下协议规范之一：

1. NFS 版本 4.1 协议规范 （如在 RFC [5661](https://tools.ietf.org/html/rfc5661)中定义）
2. NFS 版本 3 协议规范 （如在 RFC [1813年](https://tools.ietf.org/html/rfc1813)中定义）
3. NFS 版本 2 协议规范 （如在 RFC [1094年](https://tools.ietf.org/html/rfc1094)中定义）

## 部署 NFS 基础结构

你需要部署的以下计算机并将其连接局域网 (LAN) 上：

- 运行 Windows Server 2012 将在其安装 NFS 组件的两个主要服务的一个或多个计算机： NFS 服务器和 NFS 客户端。 你可以在同一台计算机或不同的计算机上安装这些组件。
- 一个或多个基于 UNIX 计算机 NFS 服务器和 NFS 客户端软件运行。 基于 UNIX 计算机运行 NFS 服务器承载 NFS 文件共享或导出，通过使用 nfs 客户端的客户端运行 Windows Server 2012 的计算机。 在基于 UNIX 在同一台计算机或不同的基于 UNIX 在计算机上，根据需要，可以安装 NFS 服务器和客户端的软件。
- 运行在 Windows Server 2008 R2 功能级别的域控制器。 域控制器提供了用户身份验证信息和映射的 Windows 环境。
- 域控制器不在部署时，可以使用网络信息服务 (NIS) 服务器来提供 UNIX 环境的用户身份验证信息。 或者，如果你愿意，你可以使用正在运行的用户名称映射的服务的计算机存储的密码和组文件。

### 使用服务器管理器在服务器上安装网络文件系统

1. 从添加角色和功能向导，服务器角色，选择**文件和存储服务**，如果尚未安装。
2. 在**文件和 iSCSI 服务**，选择**文件服务器**和**NFS 服务器**。 选择要包含所选的 NFS 功能的**添加功能**。
3. 选择**安装**在服务器上安装 NFS 组件。

### 使用 Windows PowerShell 的服务器上安装网络文件系统

1. 启动 Windows PowerShell。 右键单击任务栏上的 PowerShell 图标，然后选择**以管理员身份运行**。
2. 运行以下 Windows PowerShell 命令：

```PowerShell
Import-Module ServerManager
Add-WindowsFeature FS-NFS-Service
Import-Module NFS
```

## 配置 NFS 身份验证

使用 NFS 版本 4.1 和 NFS 3.0 协议时，你可以使用以下身份验证和安全选项。

- RPCSEC\_GSS
  - **Krb5**。 使用 Kerberos 版本 5 协议授予访问文件共享之前对用户进行身份验证。
  - **Krb5i**。 使用 Kerberos 版本 5 协议进行身份验证完整性检查 （校验和），这将验证数据不被修改。
  - **Krb5p**使用 Kerberos 版本 5 协议，这将验证 NFS 出于隐私的加密的通信。
- AUTH\_SYS

你还可以选择不使用服务器授权 (AUTH\_SYS)，这会让你可以选择启用未映射的用户访问权限。 使用未映射的用户访问时，你可以指定允许通过 UID 未映射的用户访问 / GID，这是默认设置，或者允许匿名访问。

以下部分中所述上配置 NFS 身份验证的说明。

## 创建一个 NFS 文件共享

你可以创建使用服务器管理器或 Windows PowerShell NFS cmdlet NFS 文件共享。

### 使用服务器管理器中创建了 NFS 文件共享

1. 在登录到服务器作为本地管理员组的成员。
2. 服务器管理器将自动启动。 如果自动启动，选择**开始菜单**，键入**servermanager.exe**，然后依次选择**服务器管理器**。
3. 在左侧，选择**文件和存储服务**，然后选择**共享**。
4. 选择**要创建的文件共享，开始菜单的新建共享向导**。
5. 在**选择配置文件**页上，选择**NFS 共享 – 快速**或**NFS 共享的高级**，然后选择**下一步**。
6. 在**共享位置**页面选择服务器和一个卷，并选择**下一步**。
7. **共享名称**在页面上，为新的共享中，指定一个名称，然后选择**下一步**。
8. 在**身份验证**页上，指定你想要使用此共享的身份验证方法。
9. 在**共享的权限**页上，选择**添加**，然后指定主机、 客户端组或你想要授予权限到共享的网络。
10. 在**权限**配置的访问控制你希望用户能够，类型，并选择**确定**。
11. 在**确认**页上，查看你的配置，并选择**创建**创建 NFS 文件共享。

### 等效的 Windows PowerShell 命令

以下 Windows PowerShell cmdlet 还可以创建一个 NFS 文件共享 (其中`nfs1`是共享的名称和`C:\\shares\\nfsfolder`是文件路径):

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### 已知问题
NFS 版本 4.1 允许文件名称，以便进行创建或复制使用非法字符。 如果你尝试使用 vi 编辑器中打开的文件，它将显示为已损坏。 不能将文件保存从 vi、 重命名、 将其移动或更改权限。 避免使用 illigal 字符。

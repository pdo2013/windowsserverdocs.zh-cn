---
Title: DFS 复制概述
ms.date: 03/08/2019
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: dd381c04b02889a7f2e7b8992ff6050d1b0f078a
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453060"
---
# <a name="dfs-replication-overview"></a>DFS 复制概述

> 适用于：Windows Server 2019，Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008、 Windows Server （半年频道）

DFS 复制是跨多个服务器和站点，可有效地将复制文件夹 （包括那些由 DFS 命名空间路径引用） 的 Windows Server 中的角色服务。 DFS 复制是一种高效、 多主机复制引擎，可用于保持跨有限的带宽网络连接在服务器之间同步的文件夹。 用于 DFS 命名空间，以及用于复制使用的 Windows Server 2008 或更高版本的域功能级别的域中的 Active Directory 域服务 (AD DS) SYSVOL 文件夹，它将作为复制引擎替换文件复制服务 (FRS)。

DFS 复制使用一种称为远程差分压缩 (RDC) 的压缩算法。 RDC 检测对文件中的数据更改，并使 DFS 复制仅复制已更改的文件块而不是整个文件。

复制 SYSVOL 使用 DFS 复制的详细信息，请参阅[将 SYSVOL 复制迁移到 DFS 复制](migrate-sysvol-to-dfsr.md)。

若要使用 DFS 复制，必须创建复制组并将已复制的文件夹添加到组。 下图说明了复制组、 已复制的文件夹和成员。

![包含两个成员之间的连接的复制组，每个都有两个复制文件夹](media/dfsr-overview.gif)

此图显示了复制组是一组服务器称为成员，用于参与一个或多个已复制文件夹的复制。 已复制的文件夹是保持同步每个成员上的文件夹。 在图中，有两个已复制的文件夹：Projects 和 Proposals。 随着数据在每个已复制文件夹中的更改，所做的更改会复制跨复制组的成员之间的连接。 所有成员之间的连接构成复制拓扑。
在一个复制组中创建多个已复制的文件夹可以简化部署已复制的文件夹，因为拓扑、 计划和带宽复制组的限制将应用于每个已复制文件夹的过程。 若要部署其他已复制的文件夹，您可以使用 Dfsradmin.exe 或按照说明在向导中定义的本地路径和新的已复制文件夹的权限。

每个已复制的文件夹具有唯一设置，如文件和子文件夹筛选器，以便你可以筛选出不同的文件和每个已复制文件夹的子文件夹。

存储在每个成员上已复制的文件夹可以位于不同的成员中的卷和已复制的文件夹不必是共享的文件夹或命名空间的一部分。 但是，DFS 管理管理单元中轻松共享已复制的文件夹并根据需要将其发布在现有的命名空间中。

可以通过使用 DFS 管理、 DfsrAdmin 和 Dfsrdiag 命令或调用 WMI 的脚本管理 DFS 复制。

## <a name="requirements"></a>要求

部署 DFS 复制之前，必须先按照如下所述配置服务器：

- 更新 Active Directory 域服务 (AD DS) 架构以包括 Windows Server 2003 R2 或更高版本的架构添加项。 与 Windows Server 2003 R2 或旧架构添加项，不能使用只读的已复制的文件夹。
- 确保复制组中的所有服务器位于同一林中。 不能跨不同林中的服务器进行复制。
- 在用作复制组成员的所有服务器上安装 DFS 复制。
- 请与防病毒软件供应商联系，以检查你的防病毒软件是否与 DFS 复制兼容。
- 查找任何你想要在用 NTFS 文件系统格式化的卷上复制的文件夹。 DFS 复制不支持弹性文件系统 (ReFS) 或 FAT 文件系统。 DFS 复制也不支持存储在群集共享卷上的复制内容。

## <a name="interoperability-with-azure-virtual-machines"></a>Azure 虚拟机的互操作性

在 Azure 中的虚拟机上使用 DFS 复制经过了 Windows Server;但是，有一些限制，必须遵循的要求。

- 使用快照或已保存状态还原运行 DFS 复制的服务器以便复制 SYSVOL 文件夹之外的任何内容会导致 DFS 复制失败，这需要特殊的数据库恢复步骤。 同样，不要导出、克隆或复制虚拟机。 有关详细信息，请参阅 Microsoft 知识库中的文章 [2517913](http://support.microsoft.com/kb/2517913) 以及 [安全地虚拟化 DFSR](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/)。
- 在虚拟机中承载的已复制文件夹中备份数据时，必须从来宾虚拟机中使用备份软件。
- DFS 复制需要访问物理或虚拟化域控制器 – 它不能直接与 Azure AD 通信。
- DFS 复制需要本地复制组成员与 Azure VM 中任何成员之间的 VPN 连接。 你还需要配置本地路由器（例如 Forefront 威胁管理网关）以允许 RPC 端点映射程序（端口 135）和随机分配的端口（介于 49152 与 65535 之间）通过 VPN 连接进行传递。 可以使用 Set-dfsrmachineconfiguration cmdlet 或 Dfsrdiag 命令行工具指定静态端口而不是随机端口。 有关如何为 DFS 复制指定静态端口的详细信息，请参阅 [Set-DfsrServiceConfiguration](https://docs.microsoft.com/powershell/module/dfsr/set-dfsrserviceconfiguration)。 有关为管理 Windows Server 而打开的相关端口的信息，请参阅 Microsoft 知识库中的文章 [832017](http://support.microsoft.com/kb/832017) 。

若要了解如何开始使用 Azure 虚拟机，请访问 [Microsoft Azure 网站](https://docs.microsoft.com/azure/virtual-machines/)。

## <a name="installing-dfs-replication"></a>安装 DFS 复制

DFS 复制是文件和存储服务角色的一部分。 DFS （DFS 管理，Windows PowerShell 和命令行工具的 DFS 复制模块） 的管理工具是单独的远程服务器管理工具一部分安装。

使用安装 DFS 复制[Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md)，服务器管理器或 PowerShell，接下来的部分中所述。

### <a name="to-install-dfs-by-using-server-manager"></a>使用服务器管理器安装 DFS 的步骤

1. 打开服务器管理器，单击 **“管理”** ，然后单击 **“添加角色和功能”** 。 将出现“添加角色和功能向导”。

2. 在 **“服务器选择”** 页面上，选择你想要在其上安装 DFS 的脱机虚拟机的服务器或虚拟硬盘 (VHD)。

3. 选择要安装的角色服务和功能。

    - 若要安装 DFS 复制服务上**服务器角色**页上，选择**DFS 复制**。

    - 若只安装 DFS 管理工具，请在 **“功能”** 页上，展开 **“远程服务器管理工具”** 、 **“角色管理工具”** 、 **“文件服务工具”** ，然后选择 **“DFS 管理工具”** 。

         **DFS 管理工具**DFS 管理管理单元中，DFS 复制和 DFS 命名空间 Windows PowerShell 和命令行工具，但它的模块的安装不在服务器上安装任何 DFS 服务。

### <a name="to-install-dfs-replication-by-using-windows-powershell"></a>若要使用 Windows PowerShell 安装 DFS 复制

使用提升的用户权限打开 Windows PowerShell 会话，然后键入以下命令，其中 < 名称\>是角色服务或你想要安装的功能 （请参阅下的表获取一系列相关角色服务或功能名称）：

```PowerShell
Install-WindowsFeature <name>
```

|角色服务或功能|名称|
|---|---|
|DFS 复制|`FS-DFS-Replication`|
|DFS 管理工具|`RSAT-DFS-Mgmt-Con`|

例如，若要安装远程服务器管理工具功能中的分布式文件系统工具部分，请键入：

```PowerShell
Install-WindowsFeature "RSAT-DFS-Mgmt-Con"
```

若要安装 DFS 复制和远程服务器管理工具功能的分布式文件系统工具部分，请键入：

```PowerShell
Install-WindowsFeature "FS-DFS-Replication", "RSAT-DFS-Mgmt-Con"
```

## <a name="see-also"></a>请参阅

- [DFS 命名空间和 DFS 复制概述](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v%3dws.11))
- [清单：部署 DFS 复制](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772201(v%3dws.11))
- [清单：管理 DFS 复制](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755035(v%3dws.11))
- [部署 DFS 复制](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [管理 DFS 复制](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [DFS 复制疑难解答](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732802(v%3dws.11))

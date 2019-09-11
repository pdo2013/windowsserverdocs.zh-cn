---
title: 清理 AD DS 服务器元数据
description: 使用内置工具清除已删除域控制器中的元数据
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e8adbaf07976569fdea86156e15f246aad2e4fe0
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868239"
---
# <a name="clean-up-active-directory-domain-controller-server-metadata"></a>清理 Active Directory 域控制器服务器元数据

适用于：Windows Server

在强制删除 Active Directory 域服务（AD DS）后，元数据清除是必需的过程。 在已强制删除的域控制器的域中的域控制器上执行元数据清除。 元数据清除将从标识域控制器的 AD DS 中删除数据到复制系统。 元数据清除还会删除文件复制服务（FRS）和分布式文件系统（DFS）复制连接，并尝试传输或获取已停用域的任何操作主机（也称为灵活单主机操作或 FSMO）角色控制器保存。

有三个选项可用于清理服务器元数据：

- 使用 GUI 工具清理服务器元数据
- 使用命令行清除服务器元数据
- 使用脚本清理服务器元数据

> [!NOTE]
> 如果你在使用上述任一方法执行元数据清理时收到 "访问被拒绝" 错误，请确保域控制器的 "计算机" 对象和 "NTDS 设置" 对象不受意外删除的保护。 若要验证此操作，请右键单击 "计算机" 对象或 "NTDS 设置" 对象，单击 "**属性**"，单击 "**对象**"，并清除 "**防止对象被意外删除**" 复选框。 在 Active Directory 用户和计算机 "中，如果单击"**查看**"，然后单击"**高级功能**"，则会显示对象的"**对象**"选项卡。

## <a name="clean-up-server-metadata-using-gui-tools"></a>使用 GUI 工具清理服务器元数据

使用 Windows Server 附带的远程服务器管理工具（RSAT）或 Active Directory 用户和计算机控制台（Dsa.msc）从域控制器组织单位（OU）中删除域控制器计算机帐户时，自动执行服务器元数据的清除。 在 Windows Server 2008 之前，必须执行单独的元数据清理过程。

你还可以使用 Active Directory 站点和服务控制台（Dssite.msc）删除域控制器的计算机帐户，这也会自动完成元数据清理。 但是，仅当首次在 Dssite.msc 中的计算机帐户下删除 NTDS 设置对象时，Active Directory 站点和服务才会自动删除元数据。

只要您使用的是 Windows Server 2008 或更高版本的 Dsa.msc 或 Dssite.msc，就可以为运行早期版本的 Windows 操作系统的域控制器自动清除元数据。

**Domain Admins**中的成员身份或同等身份是完成这些过程所需的最低要求。

## <a name="clean-up-server-metadata-using-activedirectory-users-and-computers"></a>使用 Active Directory 用户和计算机清除服务器元数据

1. 打开“Active Directory 用户和计算机”。
2. 如果已确定复制伙伴准备执行此过程，并且未连接到已删除的域控制器的复制伙伴，并且该控制器的元数据正在被清除，则右键单击**Active Directory 用户和计算机**"节点，然后单击 "**更改域控制器**"。 单击要删除其元数据的域控制器的名称，然后单击 **"确定"** 。
3. 展开已被强制删除的域控制器的域，然后单击 "**域控制器**"。
4. 在详细信息窗格中，右键单击要清除其元数据的域控制器的计算机对象，然后单击 "**删除**"。
5. 在 " **Active Directory 域服务**" 对话框中，确认要删除的域控制器的名称已显示，并单击 **"是"** 确认删除计算机对象。
6. 在 "**删除域控制器**" 对话框中，选择 "**此域控制器永久脱机，不能再使用 Active Directory 域服务安装向导（DCPROMO）降级**"，然后单击 "**删除**"。
7. 如果域控制器是全局编录服务器，请在 "**删除域控制器**" 对话框中，单击 **"是"** 继续删除。
8. 如果域控制器当前包含一个或多个操作主机角色，请单击 **"确定"** 将该角色或角色移至显示的域控制器。 不能更改此域控制器。 如果要将该角色移到另一个域控制器，则必须在完成服务器元数据清理过程后移动该角色。

## <a name="clean-up-server-metadata-using-activedirectory-sites-and-services"></a>使用 Active Directory 站点和服务清除服务器元数据

1. 打开“Active Directory 站点和服务”。
2. 如果已确定复制伙伴准备执行此过程，并且没有连接到已删除的域控制器的复制伙伴，并且该控制器的元数据已被清除，则右键单击**Active Directory 站点和服务**"，然后然后单击 "**更改域控制器**"。 单击要删除其元数据的域控制器的名称，然后单击 **"确定"** 。
3. 展开被强制删除的域控制器的站点，展开 "**服务器**"，展开域控制器的名称，右键单击 "NTDS 设置" 对象，然后单击 "**删除**"。
4. 在 " **Active Directory 站点和服务**" 对话框中，单击 **"是"** 以确认删除 NTDS 设置。
5. 在 "**删除域控制器**" 对话框中，选择 "**此域控制器永久脱机，不能再使用 Active Directory 域服务安装向导（DCPROMO）降级**"，然后单击 "**删除**"。
6. 如果域控制器是全局编录服务器，请在 "**删除域控制器**" 对话框中，单击 **"是"** 继续删除。
7. 如果域控制器当前包含一个或多个操作主机角色，请单击 **"确定"** 将该角色或角色移至显示的域控制器。
8. 右键单击被强制删除的域控制器，然后单击 "删除"。
9. 在 " **Active Directory 域服务**" 对话框中，单击 **"是"** 以确认删除域控制器。

## <a name="clean-up-server-metadata-using-the-command-line"></a>使用命令行清除服务器元数据

作为替代方法，您可以使用 Ntdsutil 来清除元数据，这是一个命令行工具，该工具会自动安装在安装了 Active Directory 轻型目录服务（AD LDS）的所有域控制器和服务器上。 在安装了 RSAT 的计算机上，也可以使用 ntdsutil.exe。

## <a name="to-clean-up-server-metadata-by-using-ntdsutil"></a>使用 Ntdsutil 清理服务器元数据

1. 以管理员身份打开命令提示符：在 "**开始**" 菜单上，右键单击 "**命令提示符**"，然后单击 "**以管理员身份运行**"。 如果出现 "**用户帐户控制**" 对话框，请提供企业管理员凭据（如果需要），然后单击 "**继续**"。
2. 在命令提示符下，键入以下命令，然后按 Enter：

   `ntdsutil`

3. 在 `ntdsutil:` 提示符下，键入以下命令，然后按 Enter：

   `metadata cleanup`

4. 在 `metadata cleanup:` 提示符下，键入以下命令，然后按 Enter：

   `remove selected server <ServerName>`

5. 在 "**服务器删除配置" 对话框**中，查看信息和警告，然后单击 **"是"** 以删除服务器对象和元数据。

   此时，Ntdsutil 确认已成功删除域控制器。 如果收到一条错误消息，指出找不到该对象，则可能已在之前删除域控制器。

6. `metadata cleanup:`在并`ntdsutil:`提示时，键入`quit`，然后按 enter。

7. 确认删除域控制器：

   打开 Active Directory 用户和计算机。 在已删除的域控制器的域中，单击 "**域控制器**"。 在详细信息窗格中，不应出现您删除的域控制器的对象。

   打开 Active Directory 站点和服务 "。 导航到 "**服务器**" 容器，并确认删除的域控制器的服务器对象不包含 NTDS 设置对象。 如果服务器对象下未显示子对象，则可以删除该服务器对象。 如果出现子对象，请不要删除服务器对象，因为另一个应用程序正在使用该对象。

## <a name="see-also"></a>请参阅

* [降级域控制器](Demoting-Domain-Controllers-and-Domains--Level-200-.md)
* [Ntdsutil 命令参考](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753343(v=ws.10))

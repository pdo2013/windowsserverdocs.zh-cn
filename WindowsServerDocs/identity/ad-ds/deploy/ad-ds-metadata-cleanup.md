---
title: 清理 AD DS 服务器元数据
description: 使用内置工具，以清除元数据从已删除的域控制器
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fbb6e720c9289c608d71d3c36695ba623a9df5f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818078"
---
# <a name="clean-up-active-directory-domain-controller-server-metadata"></a>清除 Active Directory 域控制器服务器元数据

适用于：Windows Server

元数据清理后强制删除 Active Directory 域服务 (AD DS) 的所需的过程。 强制删除域控制器的域中的域控制器上执行元数据清理。 元数据清除从标识到复制系统的域控制器的 AD DS 中删除数据。 元数据清除还会删除文件复制服务 (FRS) 和分布式文件系统 (DFS) 复制连接，并尝试转移或占用任何操作主机 （也称为灵活单主机操作或 FSMO） 角色的已停用的域控制器持有。

有三个选项，以清理服务器元数据：

- 通过使用 GUI 工具来清理服务器元数据
- 清理服务器元数据中使用命令行
- 通过使用脚本来清理服务器元数据

> [!NOTE]
> 如果使用任何一种方法执行元数据清理时收到"拒绝访问"错误，，请确保受意外删除的未保护的计算机对象和域控制器的 NTDS 设置对象。 若要验证此，右键单击该计算机对象或 NTDS 设置对象，请单击**属性**，单击**对象**，然后清除**防止对象被意外删除**复选框。 在 Active Directory 用户和计算机**对象**如果您单击显示对象的选项卡**视图**，然后单击**高级功能**。

## <a name="clean-up-server-metadata-using-gui-tools"></a>清理服务器元数据使用 GUI 工具

当使用远程服务器管理工具 (RSAT) 或 Active Directory 用户和计算机控制台 (Dsa.msc) 已包含在 Windows Server 从域控制器组织单位 (OU) 中删除域控制器计算机帐户自动执行清理服务器元数据。 在 Windows Server 2008 之前，必须执行单独的元数据清除过程。

您还可以使用 Active Directory 站点和服务控制台 (Dssite.msc) 若要删除域控制器的计算机帐户，这也会自动完成元数据清除。 但是，Active Directory 站点和服务元数据会自动删除仅时首先删除下方 Dssite.msc 中的计算机帐户的 NTDS 设置对象。

只要使用 Windows Server 2008 或更高版本的 RSAT 版本的 Dsa.msc 或 Dssite.msc，可以清理元数据自动运行早期版本的 Windows 操作系统的域控制器。

中的成员身份**Domain Admins**，或等效身份是完成这些过程所需的最小。

## <a name="clean-up-server-metadata-using-activedirectory-users-and-computers"></a>清理服务器元数据中使用 Active Directory 用户和计算机

1. 打开“Active Directory 用户和计算机” 。
2. 如果确定了复制伙伴中准备此过程，如果你未连接到已删除的域控制器在清理，其元数据的复制伙伴右键单击**Active Directory 用户和计算机**节点，，然后单击**更改域控制器**。 单击想要删除的元数据，然后单击域控制器的名称**确定**。
3. 展开域的域控制器，强行删除，然后单击**域控制器**。
4. 在细节窗格中，右键单击你想要清理，然后单击其元数据的域控制器的计算机对象**删除**。
5. 在中**Active Directory 域服务**对话框框中，确认你想要删除的域控制器的名称会显示，然后单击**是**以确认删除计算机对象。
6. 在中**删除域控制器**对话框中，选择**此域控制器永久脱机，不再可以使用 Active Directory 域服务安装向导 (DCPROMO)降级**，然后依次**删除**。
7. 如果域控制器是全局编录服务器，在**删除域控制器**对话框中，单击**是**若要继续删除操作。
8. 如果域控制器当前包含一个或多个操作主机角色，请单击**确定**将角色移动到显示的域控制器。 不能更改此域控制器。 如果你想要将角色移动到不同的域控制器，必须将角色移动后完成服务器元数据清除过程。

## <a name="clean-up-server-metadata-using-activedirectory-sites-and-services"></a>清理服务器元数据中使用 Active Directory 站点和服务

1. 打开“Active Directory 站点和服务”。
2. 如果确定了复制伙伴中准备此过程，如果你未连接到已删除的域控制器在清理，其元数据的复制伙伴右键单击**Active Directory 站点和服务**，然后单击**更改域控制器**。 单击想要删除的元数据，然后单击域控制器的名称**确定**。
3. 强制删除域控制器的站点扩展中，展开**服务器**，展开域控制器的名称、 NTDS 设置对象中，右键单击，然后单击**删除**。
4. 在中**Active Directory 站点和服务**对话框中，单击**是**NTDS 设置删除操作进行确认。
5. 在中**删除域控制器**对话框中，选择**此域控制器永久脱机，不再可以使用 Active Directory 域服务安装向导 (DCPROMO)降级**，然后依次**删除**。
6. 如果域控制器是全局编录服务器，在**删除域控制器**对话框中，单击**是**若要继续删除操作。
7. 如果域控制器当前包含一个或多个操作主机角色，请单击**确定**将角色移动到显示的域控制器。
8. 右键单击已强制删除域控制器，然后单击删除。
9. 在中**Active Directory 域服务**对话框中，单击**是**确认域控制器删除。

## <a name="clean-up-server-metadata-using-the-command-line"></a>清理服务器元数据中使用命令行

或者，你可以清理元数据通过使用 Ntdsutil.exe，所有域控制器和 Active Directory 轻型目录服务 (AD LDS) 安装的服务器自动安装的命令行工具。 Ntdsutil.exe，还可以在安装 RSAT 的计算机。

## <a name="to-clean-up-server-metadata-by-using-ntdsutil"></a>若要通过使用 Ntdsutil 来清理服务器元数据

1. 以管理员身份打开命令提示符：上**启动**菜单中，右键单击**命令提示符**，然后单击**以管理员身份运行**。 如果**用户帐户控制**出现对话框，请提供凭据的企业管理员，如果需要，并单击**继续**。
2. 在命令提示符下，键入以下命令，然后按 Enter：

   `ntdsutil`

3. 在 `ntdsutil:` 提示符下，键入以下命令，然后按 Enter：

   `metadata cleanup`

4. 在 `metadata cleanup:` 提示符下，键入以下命令，然后按 Enter：

   `remove selected server <ServerName>`

5. 在中**服务器中删除配置对话框**，查看信息和警告，然后单击**是**若要删除的服务器对象和元数据。

   在此情况下，Ntdsutil 确认域控制器已成功删除。 如果收到一个错误消息，指示找不到对象时，域控制器可能已被删除之前。

6. 在`metadata cleanup:`并`ntdsutil:`提示中，键入`quit`，然后按 ENTER。

7. 若要确认删除域控制器：

   打开 Active Directory 用户和计算机。 在已删除的域控制器的域中，单击**域控制器**。 在详细信息窗格中，不应出现一个对象，用于已删除的域控制器。

   打开 Active Directory 站点和服务。 导航到**服务器**容器，并确认已删除的域控制器的服务器对象不包含的 NTDS 设置对象。 如果任何子对象不显示的服务器对象下，可以删除服务器对象。 如果子对象显示，因为另一个应用程序使用对象完成删除服务器对象。

## <a name="see-also"></a>请参阅

* [降级域控制器](Demoting-Domain-Controllers-and-Domains--Level-200-.md)
* [Ntdsutil 命令参考](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753343(v=ws.10))

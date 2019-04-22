---
title: 使用继承的权限执行基于访问的枚举
description: 本文介绍如何使用继承的权限执行基于访问的枚举
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e6bd7a018a7f3a245581b5a9c63494048c7187a2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812128"
---
# <a name="using-inherited-permissions-with-access-based-enumeration"></a>使用继承的权限执行基于访问的枚举

> 适用于：Windows Server 2019，Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

默认情况下，用于 DFS 文件夹的权限从命名空间服务器的本地文件系统继承。 权限继承自系统驱动器的根目录，并授予域\\用户组读取权限。 因此，即使在启用基于访问的枚举后，命名空间中的所有文件夹仍对所有域用户可见。

## <a name="advantages-and-limitations-of-inherited-permissions"></a>继承的权限的优点和限制

使用继承的权限控制哪些用户可以查看 DFS 命名空间中的文件夹主要有下面两个优点：

-   你可以将继承的权限快速应用于多个文件夹，而不必使用脚本。
-   你可以将继承的权限应用于命名空间根目录和不含目标的文件夹。

尽管有诸多优点，但是，DFS 命名空间中的继承的权限也有很多限制，这使其不适用于大多数环境：

-   对继承的权限的修改内容无法复制到其他命名空间服务器。 因此，仅在独立命名空间或以下环境中使用继承的权限：可以实施第三方复制系统，以使访问控制列表 (ACL) 在所有命名空间服务器上保持同步。
-   DFS 管理和 **Dfsutil** 无法查看或修改继承的权限。 因此，若要管理命名空间，除了使用 DFS 管理或 **Dfsutil**，你还必须使用 Windows 资源管理器或 **Icacls** 命令。
-   使用继承的权限时，你不能修改包含目标的文件夹的权限，但是使用 **Dfsutil** 命令除外。 DFS 命名空间将使用其他工具或方法从包含目标的文件夹中自动删除权限。
-   如果你在使用继承的权限时设置包含目标的文件夹的权限，则你对包含目标的文件夹设置的 ACL 会结合使用从文件系统中的该文件夹的父项继承的权限。 你必须检查这两组权限，以确定网络权限是什么。

> [!NOTE]
> 使用继承的权限时，最简单的方法是对命名空间根目录和不含目标的文件夹设置权限。 然后，对包含目标的文件夹使用继承的权限，以便这些文件夹可从其父项继承所有的权限。

## <a name="using-inherited-permissions"></a>使用继承的权限

若要限制哪些用户可以查看 DFS 文件夹，你必须执行以下任务之一：

-   **设置文件夹中，禁用继承的显式权限。** 若要使用 DFS 管理或 **Dfsutil** 命令对包含目标（链接）的文件夹设置显式权限，请参阅[对命名空间启用基于访问的枚举](enable-access-based-enumeration-on-a-namespace.md)。
-   **修改本地文件系统中的父项的继承权限**。 若要修改由包含目标的文件夹继承的权限，如果你已对该文件夹设置显式权限，请从显式权限切换到继承的权限，如以下过程所述。 然后，使用 Windows 资源管理器或 **Icacls** 命令，以修改包含目标的文件夹从其中继承权限的文件夹的权限。

> [!NOTE]
> 如果用户已知包含目标的文件夹的 DFS 路径，则基于访问的枚举不会阻止他们获得对文件夹目标的引用。 使用 Windows 资源管理器或 **Icacls** 命令对命名空间根目录或不含目标的文件夹设置的权限，可控制用户是否可以访问 DFS 文件夹或命名空间根目录。 但是，这些权限不会阻止用户直接访问包含目标的文件夹。 只有共享权限或共享文件夹本身的 NTFS 文件系统权限，才能阻止用户访问该文件夹目标。

## <a name="to-switch-from-explicit-permissions-to-inherited-permissions"></a>从显式权限切换到继承的权限

1.  在控制台树中的**命名空间**节点下，找到要控制其可见性的文件夹（包含目标），并右键单击该文件夹，然后单击**属性**。

2.  单击“高级”选项卡。

3.  单击**使用从本地文件系统继承的权限**，然后在**确认使用继承的权限**对话框中单击**确定**。 执行此操作时将删除对此文件夹显式设置的所有权限，同时从命名空间服务器的本地文件系统中还原继承的 NTFS 权限。

4.  若要更改 DFS 命名空间中的文件夹或命名空间根目录的继承的权限，请使用 Windows 资源管理器或 **ICacls** 命令。

## <a name="see-also"></a>请参阅

-   [创建 DFS Namespace](create-a-dfs-namespace.md)
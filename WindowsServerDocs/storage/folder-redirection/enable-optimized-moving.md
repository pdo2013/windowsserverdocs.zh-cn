---
title: 启用重定向文件夹的优化移动
description: 如何将重定向文件夹的优化移动到新的文件共享。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: edf6596f7daaa2f496b8b4da36e98ee72b05dfcd
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867258"
---
# <a name="enable-optimized-moves-of-redirected-folders"></a>启用重定向文件夹的优化移动

>适用于：Windows 10，Windows 8，Windows 8.1，Windows Server 2019，Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server （半年频道）

本主题介绍如何将重定向的文件夹（文件夹重定向）的优化移动到新的文件共享。 如果启用此策略设置，当管理员移动文件共享承载重定向的文件夹并更新组策略中重定向文件夹的目标路径时，将在本地脱机文件缓存中简单地重命名缓存内容，而不会有任何延迟或用户可能会丢失数据。

以前，管理员可以在组策略中更改重定向文件夹的目标路径，并让客户端计算机在下次登录时复制文件，从而导致登录延迟。 或者，管理员可以移动文件共享，并更新组策略中重定向文件夹的目标路径。 但是，移动后开始时在客户端计算机上进行的任何更改都将丢失。

## <a name="prerequisites"></a>先决条件

优化后的移动具有以下要求：

- 必须设置文件夹重定向。 有关详细信息，请参阅[脱机文件部署文件夹重定向](deploy-folder-redirection.md)。
- 客户端计算机必须运行 Windows 10，Windows 8.1，Windows 8，Windows Server 2019，Windows Server 2016，Windows Server 2012 R2，Windows Server 2012 或 Windows Server （半年频道）。

## <a name="step-1-enable-optimized-move-in-group-policy"></a>步骤 1：启用优化移动组策略

若要优化文件夹重定向数据的重定位，请使用组策略为相应的组策略对象（GPO）启用 "对**文件夹重定向服务器上的脱机文件缓存进行优化" 服务器路径更改策略设置中的 "启用优化内容移动**"。 如果将此策略设置配置为 "**已禁用**" 或 "**未配置**"，将导致客户端将所有文件夹重定向内容复制到新位置，然后在服务器路径更改时从旧位置删除内容。

下面介绍了如何启用经过优化的重定向文件夹移动：

1. 在组策略管理 "中，右键单击为" 文件夹重定向 "设置创建的 GPO （例如，**文件夹重定向和漫游用户配置文件设置**），然后选择"**编辑**"。
2. 在 "**用户配置**" 下，导航到 "**策略**"，然后依次**管理模板**、"**系统**"、"**文件夹重定向**"。
3. 右键单击 "在**文件夹重定向服务器路径更改时启用脱机文件缓存中优化的内容移动**"，然后选择 "**编辑**"。
4. 选择 "**已启用**"，然后选择 **"确定"** 。

## <a name="step-2-relocate-the-file-share-for-redirected-folders"></a>步骤 2：重定位重定向文件夹的文件共享

在移动包含用户重定向文件夹的文件共享时，会导入，以采取预防措施来确保正确地重新定位文件夹。

>[!IMPORTANT]
>如果用户的文件正在使用中，或者如果移动时未保留完整的文件状态，则当通过网络复制文件、脱机文件生成的同步冲突，甚至数据丢失时，用户可能会遇到性能不佳的问题。

1. 提前通知用户，承载其重定向文件夹的服务器将发生更改，并建议他们执行以下操作：

      - 同步其脱机文件缓存的内容并解决所有冲突。
      - 打开提升的命令提示符，输入**GpUpdate/target： User/force**，然后注销并重新登录，以确保将最新组策略设置应用于客户端计算机

        >[!NOTE]
        >默认情况下，客户端计算机每隔90分钟更新组策略，因此，如果你为客户端计算机提供充足的时间来接收更新的策略，则不需要让用户使用**GpUpdate**。
2. 从服务器中删除文件共享，以确保不会使用文件共享中的文件。 为服务器管理器此，请在 "文件和存储服务" 的 "**共享**" 页上，右键单击相应的文件共享，然后选择 "**删除**"。

    用户将使用脱机文件脱机工作，直到移动完成，并收到组策略的更新文件夹重定向设置。

3. 使用具有备份权限的帐户将文件共享的内容移到新位置，方法是使用保留文件时间戳的方法，例如备份和还原实用程序。 若要使用**Robocopy**命令，请打开提升的命令提示符，然后键入以下命令，其中```<Source>```是```<Destination>```文件共享的当前位置，是新位置：

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >**Xcopy**命令不会保留所有文件状态。
4. 编辑文件夹重定向策略设置，更新每个要重新定位的重定向文件夹的目标文件夹位置。 有关详细信息，请参阅[部署文件夹重定向与脱机文件](deploy-folder-redirection.md)的步骤4。
5. 通知用户托管其重定向文件夹的服务器已更改，并且应使用**GpUpdate/target： User/force**命令，然后注销并重新登录以获取更新的配置并恢复正确的文件同步。

    用户至少应登录一次所有计算机，以确保数据在每个脱机文件缓存中正确重定位。

## <a name="more-information"></a>详细信息

* [部署文件夹重定向与脱机文件](deploy-folder-redirection.md)
* [部署漫游用户配置文件](deploy-roaming-user-profiles.md)
* [文件夹重定向、脱机文件和漫游用户配置文件概述](folder-redirection-rup-overview.md)
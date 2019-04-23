---
title: 启用优化的移动的重定向的文件夹
description: 如何执行的优化的移动重定向到新的文件共享的文件夹。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 98fd5d50645ad454204dcf9dabf58e97c246ab1a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853988"
---
# <a name="enable-optimized-moves-of-redirected-folders"></a>启用优化的移动的重定向的文件夹

>适用于：Windows 10、 Windows 8、 Windows 8.1、 Windows Server 2012、 Windows Server 2012 R2 和 Windows Server 2016

本主题介绍如何执行重定向的文件夹 （文件夹重定向） 优化的移动到新的文件共享。 如果启用此策略设置，当管理员将移动托管重定向的文件夹的文件共享并更新组策略中的重定向文件夹的目标路径，而无需任何延迟在本地脱机文件缓存中将只需缓存的内容已重命名或用户的潜在数据丢失。

以前，管理员可以更改组策略，并让客户端计算机将复制受影响的用户下次登录时，文件中的重定向文件夹的目标路径导致延迟的登录。 或者，管理员无法移动文件共享和更新组策略中的重定向文件夹的目标路径。 但是，任何本地进行的更改之间的移动开始和首次同步客户端计算机上移动后将会丢失。

## <a name="prerequisites"></a>先决条件

优化的移动具有以下要求：

- 文件夹重定向，必须先设置。 有关详细信息请参阅[部署脱机文件的文件夹重定向](deploy-folder-redirection.md)。
- 客户端计算机必须运行 Windows 10、 Windows 8.1，Windows 8、 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。

## <a name="step-1-enable-optimized-move-in-group-policy"></a>第 1 步：启用组策略中的优化的移动

若要优化的文件夹重定向数据重定位，使用组策略启用**启用优化移动中脱机文件缓存内容的文件夹重定向服务器路径更改**相应的组策略的策略设置对象 (GPO)。 配置此策略设置设置为**已禁用**或**未配置**会导致将文件夹重定向的所有内容都复制到新位置，然后将内容从旧位置删除，如果客户端服务器路径更改。

下面介绍了如何启用优化移动重定向的文件夹：

1. 在组策略管理，右键单击你创建的文件夹重定向设置的 GPO (例如，**文件夹重定向和漫游用户配置文件设置**)，然后选择**编辑**。
2. 下**用户配置**，导航到**策略**，然后**管理模板**，然后**系统**，然后**文件夹重定向**。
3. 右键单击**启用优化移动中脱机文件缓存内容的文件夹重定向服务器路径更改**，然后选择**编辑**。
4. 选择**已启用**，然后选择**确定**。

## <a name="step-2-relocate-the-file-share-for-redirected-folders"></a>步骤 2：重新定位该文件共享的重定向的文件夹

如果移动的文件共享的包含用户的重定向文件夹，则导入采取预防措施以确保文件夹正确重定位。

>[!IMPORTANT]
>如果用户的文件中使用或通过网络生成的脱机文件或甚至数据丢失的同步冲突复制文件的完整文件状态不会保留在移动中，如果用户可能会遇到性能不佳。

1. 提前通知用户在托管其重定向的文件夹的服务器将更改，并建议他们执行以下操作：

      - 同步其脱机文件缓存的内容并解决任何冲突。
      - 打开提升的命令提示符中，输入**GpUpdate /Target:User /Force**，然后注销并重新登录以确保最新的组策略设置应用于客户端计算机

        >[!NOTE]
        >默认情况下，客户端计算机更新组策略每 90 分钟，因此如果您允许足够的时间客户端计算机接收更新后的策略，不需要让用户使用**GpUpdate**。
2. 从服务器以确保文件共享中的没有文件正在使用中删除文件共享。 为此，在服务器管理器中，在**共享**页上的文件和存储服务，右键单击相应的文件共享，然后选择**删除**。

    用户将工作脱机使用脱机文件，直到移动操作完成，并且它们接收来自组策略更新的文件夹重定向设置。

3. 使用具有备份特权的帐户，将文件共享的内容移动到新位置使用的方法，可保留文件时间戳，如备份和还原实用程序。 若要使用**Robocopy**命令，打开提升的命令提示符，然后键入以下命令，其中```<Source>```是当前的文件共享位置和```<Destination>```是新的位置：

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >**Xcopy**命令不会保留所有的文件状态。
4. 编辑文件夹重定向策略设置，更新每个你想要重新定位的重定向文件夹的目标文件夹位置。 有关详细信息，请参阅步骤 4[部署脱机文件的文件夹重定向](deploy-folder-redirection.md)。
5. 通知的用户已更改承载其重定向的文件夹的服务器，并且它们应使用**GpUpdate /Target:User /Force**命令，然后注销并重新登录以获取更新的配置和恢复正确的文件同步。

    用户应登录到的所有计算机至少一次以确保数据在每个脱机文件缓存中获取正确的定位。

## <a name="more-information"></a>详细信息

* [部署使用脱机文件的文件夹重定向](deploy-folder-redirection.md)
* [部署漫游用户配置文件](deploy-roaming-user-profiles.md)
* [文件夹重定向、 脱机文件和漫游用户配置文件概述](folder-redirection-rup-overview.md)
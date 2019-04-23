---
title: 管理 RDS 集合中的用户
description: 了解如何管理远程桌面服务中的用户。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2727e1ab-69b8-46f3-9f6d-2540324fe596
author: christianmontoya
ms.author: chrimo
ms.date: 03/27/2018
manager: scottman
ms.openlocfilehash: 4b45061697926a3003712a88610cb17ef3c00c45
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859638"
---
# <a name="manage-users-in-your-rds-collection"></a>管理 RDS 集合中的用户

>适用于：Windows 服务器 （半年频道），Windows Server 2016

作为管理员，你可以直接管理哪些用户有权访问特定集合。 这样一来，您可以使用标准的应用程序的信息工作者，创建一个集合，但然后使用适用于工程师的图形密集型建模应用程序创建单独的集合。 有两个主要步骤来管理远程桌面服务 (RDS) 部署中的用户访问权限：

1.  [在 Active Directory 中创建用户和组](#create-your-users-and-groups-in-active-directory)
2.  [将用户和组分配到集合](#assign-users-and-groups-to-collections)


## <a name="create-your-users-and-groups-in-active-directory"></a>在 Active Directory 中创建用户和组

在 RDS 部署中，Active Directory 域服务 (AD DS) 是所有用户、 组和域中的其他对象的源。 你可以管理 Active Directory 直接与 PowerShell 中，也可以使用内置的易用性和灵活性添加的用户界面工具。 以下步骤将指导您安装这些工具 — 如果没有已安装，然后使用这些工具来管理用户和组。

### <a name="install-ad-ds-tools"></a>安装 AD DS 工具

以下步骤详述了如何在已运行 AD DS 的服务器上安装 AD DS 工具。 安装后，你可以然后创建用户或创建组。

1. 连接到运行 Active Directory 域服务的服务器。 为 Azure 部署：
   1. 在 Azure 门户中，单击**浏览 > 资源组**，然后单击部署的资源组
   2. 选择 AD 虚拟机。
   3. 单击**Connect > 打开**打开远程桌面客户端。 如果**Connect**就会变灰，虚拟机可能不具有公共 IP 地址。 为了让它一个执行以下步骤，然后重试此步骤。
      1. 单击**设置 > 网络接口**，然后单击相应的网络接口。
      2. 单击**设置 > IP 地址**。
      3. 有关**公共 IP 地址**，选择**已启用**，然后单击**IP 地址**。
      4. 如果你有想要使用现有公共 IP 地址，它从列表中选择。 否则，请单击**新建**，输入一个名称，然后单击**确定**并**保存**。
   4. 在客户端，单击**Connect**，然后单击**使用另一帐户**。 输入域管理员帐户的用户名和密码。
   5. 单击**是**当系统询问有关证书的信息。
2. 安装 AD DS 工具：
   1. 在服务器管理器中，单击**管理 > 添加角色和功能**。
   2. 单击**基于角色或基于功能的安装**，然后单击当前 AD 服务器。 请按照步骤，直到你到达**功能**选项卡。
   3. 展开**远程服务器管理工具 > 角色管理工具 > AD DS 和 AD LDS 工具**，然后选择**AD DS 工具**。
   4. 选择**如果需要自动重新启动目标服务器**，然后单击**安装**。

### <a name="create-a-group"></a>创建一个组

可以使用 AD DS 组授予对一组用户需要使用相同的远程资源的访问权限。

1. 在服务器管理器中运行 AD DS 的服务器上，单击**工具 > Active Directory 用户和计算机**。
2. 展开左侧的窗格，可以查看子文件夹中的域。
3. 右键单击你想要创建组，并单击的文件夹**新建 > 组**。
4. 输入一个相应的组的名称，然后选择**Global**并**安全**。

### <a name="create-a-user-and-add-to-a-group"></a>创建一个用户并将添加到组
1. 在服务器管理器中运行 AD DS 的服务器上，单击**工具 > Active Directory 用户和计算机**。
2. 展开左侧的窗格，可以查看子文件夹中的域。
3. 右键单击**用户**，然后单击**新建 > 用户**。
4. 输入最小值、 名字和用户登录名。
5. 输入并确认用户的密码。 将相应的用户选项设置等**用户必须更改密码，下次登录时须**。
6. 将新用户添加到组：
   1. 在中**用户**文件夹，右键单击新用户。
   2. 单击**添加到组**。
   3. 输入你想要将用户添加的组的名称。

## <a name="assign-users-and-groups-to-collections"></a>将用户和组分配到集合
现在，已在 Active Directory 中创建的用户和组，可以添加一些有关谁有权访问的远程桌面集合在部署中的粒度。

1. 连接到运行前面所述的步骤的远程桌面连接代理 （RD 连接代理） 角色的服务器。
2. 将其他远程桌面服务器添加到托管服务器的 RD 连接代理池：
   1. 在服务器管理器中，单击**管理 > 添加服务器**。
   2. 单击**立即查找**。
   3. 单击正在运行的远程桌面服务角色，在部署中每个服务器，然后单击**确定**。
3. 编辑集合以向特定用户或组分配访问权限：
   1. 在服务器管理器中，单击**远程桌面服务 > 概述**，然后单击特定集合。
   2. 下**属性**，单击**任务 > 编辑属性**。
   3. 单击**用户组**。
   4. 单击**添加**和输入的用户或组你想要有权访问集合。 您也可以用户和组在此窗口中通过删除选择的用户或组你想要删除，并单击**删除**。 
   
   >[!NOTE] 
   > 用户组窗口永远不能为空。 若要缩小的用户有权访问集合的范围，首先必须更广泛的组中删除之前添加特定用户或组。
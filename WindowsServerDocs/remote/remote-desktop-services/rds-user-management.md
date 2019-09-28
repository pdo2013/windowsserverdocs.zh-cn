---
title: 管理 RDS 集合中的用户
description: 了解如何管理远程桌面服务中的用户。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 870a6360f685c2de31485135202b0f1415c90d85
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403852"
---
# <a name="manage-users-in-your-rds-collection"></a>管理 RDS 集合中的用户

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

管理员可以直接管理哪些用户有权访问特定的集合。 这样，便可以创建一个包含信息工作者所用的标准应用程序的集合，然后创建一个包含工程师所用的图形密集型建模应用程序的独立集合。 管理远程桌面服务 (RDS) 部署中的用户访问权限需要执行两个主要步骤：

1.  [在 Active Directory 中创建用户和组](#create-your-users-and-groups-in-active-directory)
2.  [将用户和组分配到集合](#assign-users-and-groups-to-collections)


## <a name="create-your-users-and-groups-in-active-directory"></a>在 Active Directory 中创建用户和组

在 RDS 部署中，Active Directory 域服务 (AD DS) 是域中所有用户、组和其他对象的源。 可以直接使用 PowerShell 管理 Active Directory，也可以使用更简单灵活的内置 UI 工具。 以下步骤将引导你安装这些工具（如果尚未安装），然后使用这些工具来管理用户和组。

### <a name="install-ad-ds-tools"></a>安装 AD DS 工具

以下步骤详细说明如何在已运行了 AD DS 的服务器上安装 AD DS 工具。 安装这些工具后，即可创建用户或组。

1. 连接到运行 Active Directory 域服务的服务器。 对于 Azure 部署：
   1. 在 Azure 门户中，单击“浏览”>“资源组”，然后单击部署的资源组。 
   2. 选择 AD 虚拟机。
   3. 单击“连接”>“打开”以打开远程桌面客户端。  如果“连接”灰显，可能表示虚拟机没有公共 IP 地址。  若要为它提供一个公共 IP 地址，请执行以下步骤，然后重试上述步骤。
      1. 单击“设置”>“网络接口”，然后单击相应的网络接口。 
      2. 单击“设置”>“IP 地址”。 
      3. 对于“公共 IP 地址”，请选择“已启用”，然后单击“IP 地址”。   
      4. 若要使用现有的公共 IP 地址，请从列表中选择它。 否则，请单击“新建”并输入名称，然后依次单击“确定”、“保存”。   
   4. 在客户端中单击“连接”，然后单击“使用另一个帐户”。   输入域管理员帐户的用户名和密码。
   5. 收到有关证书的提示时，请单击“是”  。
2. 安装 AD DS 工具：
   1. 在服务器管理器中，单击“管理”>“添加角色和功能”。 
   2. 单击“基于角色或基于功能的安装”，然后单击当前的 AD 服务器。  完成每个步骤，直到进入“功能”选项卡。 
   3. 展开“远程服务器管理工具”>“角色管理工具”>“AD DS 和 AD LDS 工具”，然后选择“AD DS 工具”。  
   4. 选择“如果需要，自动重新启动目标服务器”，然后单击“安装”。  

### <a name="create-a-group"></a>创建一个组

可以使用 AD DS 组向需要使用相同远程资源的一组用户授予访问权限。

1. 在运行 AD DS 的服务器上的服务器管理器中，单击“工具”>“Active Directory 用户和计算机”。 
2. 展开左窗格中的域以查看其子文件夹。
3. 右键单击要在其中创建组的文件夹，然后单击“新建”>“组”。 
4. 输入适当的组名称，然后选择“全局”和“安全性”。  

### <a name="create-a-user-and-add-to-a-group"></a>创建用户并将其添加到组
1. 在运行 AD DS 的服务器上的服务器管理器中，单击“工具”>“Active Directory 用户和计算机”。 
2. 展开左窗格中的域以查看其子文件夹。
3. 右键单击“用户”，然后单击“新建”>“用户”。  
4. 最起码需要输入名字和用户登录名。
5. 输入并确认该用户的密码。 设置相应的用户选项，例如“用户下次登录时须更改密码”。 
6. 将新用户添加到组：
   1. 在“用户”文件夹中，右键单击新用户。 
   2. 单击“添加到组”。 
   3. 输入要将用户添加到的组的名称。

## <a name="assign-users-and-groups-to-collections"></a>将用户和组分配到集合
在 Active Directory 中创建用户和组后，可以在谁有权访问部署中的远程桌面集合方面添加一些粒度级。

1. 遵循前面所述的步骤，连接到运行远程桌面连接代理（RD 连接代理）角色的服务器。
2. 将其他远程桌面服务器添加到托管服务器的 RD 连接代理池：
   1. 在服务器管理器中，单击“管理”>“添加服务器”。 
   2. 单击“立即查找”。 
   3. 单击部署中正在运行远程桌面服务角色的每个服务器，然后单击“确定”。 
3. 编辑集合，以向特定的用户或组分配访问权限：
   1. 在服务器管理器中单击“远程桌面服务”>“概述”，然后单击特定的集合。 
   2. 在“属性”下，单击“任务”>“编辑属性”。  
   3. 单击“用户组”。 
   4. 单击“添加”，然后输入你希望其有权访问该集合的用户或组。  还可以在此窗口中删除用户和组，方法是选择要删除的用户或组，然后单击“删除”。  
   
   >[!NOTE] 
   > “用户组”窗口永远不可为空。 若要缩小有权访问集合的用户的范围，必须先添加特定的用户或组，然后删除更广泛的组。
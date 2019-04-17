---
title: 为访问控制创建用户角色
description: 本主题介绍的 IP 地址管理 (IPAM) 管理指南中的 Windows Server 2016 的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae6a42db-a104-401b-a8e6-b85c47d30b46
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fa0ed71d399ad638a648946952fe170d93f69ceb
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-user-role-for-access-control"></a>为访问控制创建用户角色

>适用于：Windows Server（半年通道），Windows Server 2016

可以使用本主题中 IPAM 客户端控制台创建一个新访问控制用户角色。  
  
在会员**管理员**，或等效的最低要求执行此过程。  
  
> [!NOTE]  
> 创建角色后，你可以创建访问策略以分配到某个特定用户或组 Active Directory 角色。 有关详细信息，请参阅[创建访问策略](../../technologies/ipam/Create-an-Access-Policy.md)。  
  
### <a name="to-create-a-role"></a>若要创建角色  
  
1.  在服务器管理器中，单击**IPAM**。 显示 IPAM 客户端控制台。  
  
2.  在导航窗格中，单击**访问控制**，在较低的导航窗格中，单击**角色**。  
  
    ![访问控制角色](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_01.jpg)  
  
3.  右键单击**角色**，然后单击**添加用户角色**。  
  
    ![添加用户角色](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_02.jpg)  
  
4.  **添加或编辑角色**对话框中打开。 在**名称**，使清除角色函数角色为键入一个名称。 例如，如果你想要创建了允许管理员进行管理 DNS SRV 资源记录的作用，你可能会命名角色**IPAMSrv**。 如果需要请向下滚动**操作**以找到你想要定义角色操作的类型。 例如，向下滚动到**DNS 资源录制管理操作**。  
  
    ![DNS 资源录制管理操作](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_03.jpg)  
  
5.  展开**DNS 资源录制管理操作**，然后找到**SRV 记录操作**。  
  
    ![SRV 记录操作](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_04.jpg)  
  
6.  展开并选择**SRV 记录操作**，然后单击**确定**。  
  
    ![选择 SRV 记录操作](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_05.jpg)  
  
7.  在客户端 IPAM 控制台中，单击你刚创建的角色。 在**详细信息视图中，**显示角色许可的范围内的操作。  
  
    ![新的角色详细信息](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_06.jpg)  
  
## <a name="see-also"></a>请参阅  
[基于角色访问控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  



---
title: 为访问控制创建用户角色
description: 本主题是 Windows Server 2016 中的 IP 地址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae6a42db-a104-401b-a8e6-b85c47d30b46
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3798b074a0ca7e20602da7986fe6b54e81da5495
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284096"
---
# <a name="create-a-user-role-for-access-control"></a>为访问控制创建用户角色

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题可用于在 IPAM 客户端控制台中创建新的访问控制用户角色。  
  
Administrators  组成员或同等身份是执行此过程的最低要求。  
  
> [!NOTE]  
> 创建角色后，可以创建访问策略将角色分配到特定用户或 Active Directory 组。 有关详细信息，请参阅[创建访问策略](../../technologies/ipam/Create-an-Access-Policy.md)。  
  
### <a name="to-create-a-role"></a>若要创建角色  
  
1.  在服务器管理器中，单击**IPAM**。 IPAM 客户端控制台将显示。  
  
2.  在导航窗格中，单击**访问控制**，然后在下部导航窗格中，单击**角色**。  
  
    ![访问控制角色](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_01.jpg)  
  
3.  右键单击**角色**，然后单击**添加用户角色**。  
  
    ![添加用户角色](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_02.jpg)  
  
4.  **添加或编辑角色**对话框随即打开。 在中**名称**，键入角色使清除角色功能的名称。 例如，如果你想要创建一个角色，使管理员能够管理 DNS SRV 资源记录，你可能会命名角色**IPAMSrv**。 如果需要向下滚动**操作**来查找要为角色定义的操作的类型。 对于此示例中，向下滚动到**DNS 资源记录管理操作**。  
  
    ![DNS 资源记录管理操作](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_03.jpg)  
  
5.  展开**DNS 资源记录管理操作**，然后找到**SRV 记录操作**。  
  
    ![SRV 记录操作](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_04.jpg)  
  
6.  展开，然后选择**SRV 记录 operations**，然后单击**确定**。  
  
    ![选择 SRV 记录操作](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_05.jpg)  
  
7.  在 IPAM 客户端控制台中，单击刚创建的角色。 在中**详细信息视图**显示角色允许的操作。  
  
    ![新角色的详细信息](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_06.jpg)  
  
## <a name="see-also"></a>请参阅  
[基于角色的访问控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  



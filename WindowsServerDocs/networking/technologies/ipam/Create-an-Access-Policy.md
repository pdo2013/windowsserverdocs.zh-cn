---
title: 创建访问策略
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
ms.assetid: 854bd064-2f86-4678-a940-a04b3e48ae10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ac8229952c7e038f9af8fc4f9287b1821db887ac
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="create-an-access-policy"></a>创建访问策略

>适用于：Windows Server（半年通道），Windows Server 2016

本主题可用于创建 IPAM 客户端主机访问策略。  
  
在会员**管理员**，或等效的最低要求执行此过程。  
  
> [!NOTE]  
> 你可以在 Active Directory 创建特定用户或组用户访问策略。 当您创建访问策略时，你必须选择内置的 IPAM 角色或已创建自定义角色。 自定义角色的详细信息，请参阅[为访问控制创建用户角色](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md)。  
  
### <a name="to-create-an-access-policy"></a>若要创建访问策略  
  
1.  在服务器管理器中，单击**IPAM**。 显示 IPAM 客户端控制台。  
  
2.  在导航窗格中，单击**访问控制**。 在较低的导航窗格中，右键单击**访问策略**，然后单击**添加访问策略**。  
  
    ![添加访问策略](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)  
  
3.  **添加访问策略**对话框中打开。 在**用户设置**，单击**添加**。  
  
    ![添加访问策略](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)  
  
4.  **选择用户或组**对话框中打开。 单击**位置**。  
  
    ![用户或组位置](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)  
  
5.  **位置**对话框中打开。 浏览到包含的用户帐户的位置，该位置，请选择，然后单击**确定**。 **位置**关闭对话框。  
  
    ![选择位置](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)  
  
6.  在**选择用户或组**对话框中，在**输入要选择的对象名称**，键入你要为其创建访问策略的用户帐户名。 单击**确定**。  
  
7.  在**添加访问策略**中**用户设置**，**用户别名**现在包含应用该策略的用户帐户。 在**访问设置**，单击**新建**。  
  
    ![新访问设置](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)  
  
8.  在**添加访问策略**，**访问设置**更改为**新设置**。  
  
    ![对话框名称将更改为新的设置](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)  
  
9. 单击**选择角色**以展开角色列表。 选择一种内置的角色，或者，如果你已创建新的角色，可以选择其中一个你创建的角色。 例如，如果你创建适用于用户 IPAMSrv 角色，请单击**IPAMSrv**。  
  
    ![选择角色](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)  
  
10. 单击**添加设置**。  
  
    ![添加新的设置](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)  
  
11. 角色将添加到访问策略。 若要创建其他访问策略，请单击**应用**，然后为你想要创建每个策略重复这些步骤。 如果你不想要创建其他策略，请单击**确定**。  
  
    ![单击应用或确定](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)  
  
12. 在 IPAM 客户端控制台显示器窗格中，验证创建的新访问策略。  
  
    ![查看新访问策略](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)  
  
## <a name="see-also"></a>请参阅  
[基于角色访问控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  



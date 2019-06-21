---
title: 创建访问策略
description: 本主题是 Windows Server 2016 中的 IP 地址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 854bd064-2f86-4678-a940-a04b3e48ae10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3e72d47dc3c32db7465f7c47b16dcdc777636fd9
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282224"
---
# <a name="create-an-access-policy"></a>创建访问策略

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题可用于在 IPAM 客户端控制台中创建访问策略。  
  
Administrators  组成员或同等身份是执行此过程的最低要求。  
  
> [!NOTE]  
> 可以在 Active Directory 中创建访问策略为特定用户或用户组。 创建访问策略时，必须选择一个内置的 IPAM 角色或已创建的自定义角色。 自定义角色的详细信息，请参阅[创建用户角色的访问控制](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md)。  
  
### <a name="to-create-an-access-policy"></a>若要创建访问策略  
  
1.  在服务器管理器中，单击**IPAM**。 IPAM 客户端控制台将显示。  
  
2.  在导航窗格中，单击**访问控制**。 在下部导航窗格中，右键单击**访问策略**，然后单击**添加访问策略**。  
  
    ![添加访问策略](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)  
  
3.  **添加访问策略**对话框随即打开。 在中**用户设置**，单击**添加**。  
  
    ![添加访问策略](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)  
  
4.  **选择用户或组**对话框随即打开。 单击**位置**。  
  
    ![用户或组的位置](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)  
  
5.  **位置**对话框随即打开。 浏览到包含用户帐户的位置，选择的位置，然后单击**确定**。 **位置**对话框将关闭。  
  
    ![选择位置](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)  
  
6.  在中**选择用户或组**对话框中**输入要选择的对象名称**，键入你想要创建访问策略的用户帐户名。 单击 **“确定”** 。  
  
7.  在中**添加访问策略**，在**用户设置**，**用户别名**现在包含对其应用策略的用户帐户。 在中**访问设置**，单击**新建**。  
  
    ![新的访问设置](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)  
  
8.  在中**添加访问策略**，**访问设置**更改为**新设置**。  
  
    ![对话框名称更改为新设置](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)  
  
9. 单击**选择角色**展开角色列表。 选择一个内置角色，或者，如果你已创建新的角色，选择你创建的角色之一。 例如，如果创建 IPAMSrv 角色要应用于用户，请单击**IPAMSrv**。  
  
    ![选择角色](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)  
  
10. 单击**添加设置**。  
  
    ![添加新设置](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)  
  
11. 将角色添加到的访问策略。 若要创建其他的访问策略，请单击**应用**，然后为你想要创建每个策略重复这些步骤。 如果不想创建更多的策略，请单击**确定**。  
  
    ![单击应用或确定](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)  
  
12. 在 IPAM 客户端控制台的显示窗格中，验证创建新的访问策略。  
  
    ![查看新的访问策略](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)  
  
## <a name="see-also"></a>请参阅  
[基于角色的访问控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  



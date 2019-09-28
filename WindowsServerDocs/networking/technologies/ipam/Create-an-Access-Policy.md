---
title: 创建访问策略
description: 本主题是 Windows Server 2016 中的 IP 地址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 854bd064-2f86-4678-a940-a04b3e48ae10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: da5cc366a08f9a3f5b69952a2dff1f717fb1647b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355356"
---
# <a name="create-an-access-policy"></a>创建访问策略

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用本主题在 IPAM 客户端控制台中创建访问策略。  
  
Administrators组成员或同等身份是执行此过程的最低要求。  
  
> [!NOTE]  
> 您可以为特定用户或 Active Directory 中的用户组创建访问策略。 创建访问策略时，必须选择内置 IPAM 角色或已创建的自定义角色。 有关自定义角色的详细信息，请参阅[创建访问控制的用户角色](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md)。  
  
### <a name="to-create-an-access-policy"></a>创建访问策略  
  
1.  在服务器管理器中，单击 " **IPAM**"。 IPAM 客户端控制台随即出现。  
  
2.  在导航窗格中，单击 "**访问控制**"。 在下部导航窗格中，右键单击 "**访问策略**"，然后单击 "**添加访问策略**"。  
  
    ![添加访问策略](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)  
  
3.  "**添加访问策略**" 对话框将打开。 在 "**用户设置**" 中，单击 "**添加**"。  
  
    ![添加访问策略](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)  
  
4.  此时将打开 "**选择用户或组**" 对话框。 单击 "**位置**"。  
  
    ![用户或组的位置](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)  
  
5.  "**位置**" 对话框将打开。 浏览到包含该用户帐户的位置，选择该位置，然后单击 **"确定"** 。 "**位置**" 对话框将关闭。  
  
    ![选择位置](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)  
  
6.  在 "**选择用户或组**" 对话框的 "**输入要选择的对象名称**" 中，键入要为其创建访问策略的用户帐户名称。 单击 **“确定”** 。  
  
7.  在 "**添加访问策略**" 的 "**用户设置**" 中，"**用户别名**" 现在包含策略应用到的用户帐户。 在 "**访问设置**" 中，单击 "**新建**"。  
  
    ![新访问设置](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)  
  
8.  在 "**添加访问策略**" 中，**访问设置**更改为**新设置**。  
  
    ![对话框名称更改为新设置](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)  
  
9. 单击 "**选择角色**" 展开角色列表。 选择其中一个内置角色，或者，如果已创建新的角色，请选择一个创建的角色。 例如，如果已创建要应用于用户的 IPAMSrv 角色，请单击 " **IPAMSrv**"。  
  
    ![选择角色](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)  
  
10. 单击 "**添加设置**"。  
  
    ![添加新设置](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)  
  
11. 该角色将添加到访问策略中。 若要创建其他访问策略，请单击 "**应用**"，然后对要创建的每个策略重复上述步骤。 如果不想创建其他策略，请单击 **"确定"** 。  
  
    ![单击 "应用" 或 "确定"](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)  
  
12. 在 IPAM 客户端控制台的 "显示" 窗格中，验证是否创建了新的访问策略。  
  
    ![查看新的访问策略](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)  
  
## <a name="see-also"></a>请参阅  
[基于角色的访问控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  



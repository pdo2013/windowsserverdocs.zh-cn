---
ms.assetid: 7d230527-f4fe-4572-8838-0b354ee0b06b
title: "添加索赔说明"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0e388ef656d3b690da62b077cb9f9e678a771e64
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-claim-description"></a>添加索赔说明

>适用于：Windows Server 2016，Windows Server 2012 R2

帐户合作伙伴组织中, 管理员创建的索赔代表组或角色中的用户的会员表示用户，例如，用户员工标识号有关的某些数据。

在资源合作伙伴组织中，管理员创建相应的索赔表示组，可以被识别为资源用户的用户。 因为传出索赔帐户合作伙伴组织地图传入的索赔均在资源合作伙伴组织中，在资源的合作伙伴能够接受帐户合作伙伴提供了的凭据。 

你可以使用下面的过程中添加索赔。

在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。

## <a name="to-add-a-claim-description"></a>若要添加的索赔说明

1. 在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。 

2.  展开**服务**和右键单击上**添加索赔描述**。
![添加索赔描述](media\Add-a-Claim-Description\claimdesc1.png)

3.  在添加声称描述对话框中，在**显示名称**，输入一个唯一的名称，表明的组或此声明的角色。

4.  添加**简称**。

5.  在**声称标识符**，键入程序与组或您将使用此声明的角色 URI。

6.  下**描述**，键入文本最佳描述此声明的用途。

7.  根据你的组织的需求，根据需要，将发布到联盟元数据的此声明中选择任一以下复选框:


    - 若要将注意到此服务器可接受此声明的合作伙伴能够此声明的发布，请单击**作为索赔类型此联合身份验证服务可接受联盟元数据中发布此声明**。
    - 若要将注意到此服务器可以发出此声明的合作伙伴能够此声明的发布，请单击**为此联合身份验证服务可发送的索赔类型联盟元数据中发布此声明**。

8.  单击**确定**。

![添加索赔描述](media\Add-a-Claim-Description\claimdesc2.png)

  
## <a name="see-also"></a>请参阅  
[广告 FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 

---
ms.assetid: 7d230527-f4fe-4572-8838-0b354ee0b06b
title: 添加声明说明
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 00ae720a933289e3cd4bde5fe9d20610e38ae726
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866039"
---
# <a name="add-a-claim-description"></a>添加声明说明


在帐户伙伴组织中，管理员创建声明来表示用户在组或角色中的成员身份，或表示有关用户的一些数据，例如，用户的员工标识号。

在资源伙伴组织中，管理员创建相应的声明来表示可被识别为资源用户的组和用户。 因为帐户伙伴组织中的传出声明映射到资源伙伴组织中的传入声明，资源伙伴可以接受帐户伙伴提供的凭据。 

你可以使用以下过程添加声明。

本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。

## <a name="to-add-a-claim-description"></a>添加声明说明

1. 在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。 

2. 展开 "**服务**"，然后在右侧单击 "**添加声明说明**"。
   ![添加声明说明](media/Add-a-Claim-Description/claimdesc1.png)

3. 在 "添加声明说明" 对话框中的 "**显示名称**" 中，键入标识此声明的组或角色的唯一名称。

4. 添加**短名称**。

5. 在 "**声明标识符**" 中，键入与将使用的声明的组或角色关联的 URI。

6. 在 "**说明**" 下，键入最能说明此声明用途的文本。

7. 根据组织的需要，根据需要选中以下复选框之一，将此声明发布到联合元数据中：


~~~
- To publish this claim to make partners aware that this server can accept this claim, click **Publish this claim in federation metadata as a claim type that this Federation Service can accept**.
- To publish this claim to make partners aware that this server can issue this claim, click **Publish this claim in federation metadata as a claim type that this Federation Service can send**.
~~~

8. 单击 **“确定”** 。

![添加声明说明](media/Add-a-Claim-Description/claimdesc2.png)


## <a name="see-also"></a>请参阅  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 

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
ms.openlocfilehash: 1023ca7da02d2a1f6af42f68892dc4c5c8f1a2bf
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444376"
---
# <a name="add-a-claim-description"></a>添加声明说明


在帐户伙伴组织中，管理员创建声明来表示用户的组或角色中的成员身份或以表示有关用户，例如，用户的雇员标识号的一些数据。

在资源伙伴组织中，管理员创建相应的声明来表示组和用户可以被识别为资源的用户。 因为传出声明在帐户伙伴组织映射到资源伙伴组织中的传入声明资源伙伴是可以接受帐户伙伴提供的凭据。 

可以使用以下过程来添加声明。

本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。

## <a name="to-add-a-claim-description"></a>若要添加声明说明

1. 在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。 

2. 展开**服务**并在右侧，单击**添加声明说明**。
   ![添加声明说明](media/Add-a-Claim-Description/claimdesc1.png)

3. 在添加声明说明对话框中，在**显示名称**，键入用于标识组或此声明的角色的唯一名称。

4. 添加**简短名称**。

5. 在中**声明标识符**，键入与组或你将使用的声明的角色相关联的 URI。

6. 下**说明**，键入最适当地描述此声明的用途的文本。

7. 具体取决于你组织的需要，可以选择以下复选框，根据需要，若要为联合身份验证元数据中发布此声明：


~~~
- To publish this claim to make partners aware that this server can accept this claim, click **Publish this claim in federation metadata as a claim type that this Federation Service can accept**.
- To publish this claim to make partners aware that this server can issue this claim, click **Publish this claim in federation metadata as a claim type that this Federation Service can send**.
~~~

8. 单击 **“确定”** 。

![添加声明说明](media/Add-a-Claim-Description/claimdesc2.png)


## <a name="see-also"></a>请参阅  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 

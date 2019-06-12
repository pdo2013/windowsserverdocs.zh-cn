---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: 创建非声明感知信赖方信任
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e94b07d7fa654732526d0b43daadc9ad0ad4f3a8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444928"
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>创建非声明感知信赖方信任


在 AD FS 管理管理单元\-中，非\-声明\-感知信赖方信任是创建用于表示联合身份验证服务和一个单独的网站之间的信任对象\-基于不应用程序声明\-识别和访问通过 Web 应用程序代理。  
  
非\-声明\-感知信赖方信任是组成的标识符、 名称和身份验证和授权规则，通过 Web 应用程序代理访问信赖方信任时的信赖方信任。 这些 web\-基于不依赖于声明，换而言之，这些集成 Windows 身份验证的应用程序\-基于应用程序，可以强制实施访问时基于声明的访问权限的授权规则通过 Web 应用程序代理在企业网络的外部。  
  
若要添加新非\-声明\-感知信赖方信任，使用 AD FS 管理管理单元\-在中，执行以下过程。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
## <a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>若要手动创建了非声明感知信赖方信任 
1. 在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  下**操作**，单击**添加信赖方信任**。  
![信赖方](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  上**欢迎**页上，选择**非声明感知**然后单击**启动**。  
![信赖方](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon1.PNG) 
  
4.  上**指定显示名称**页上，键入一个名称中的**显示名称**下**说明**键入此信赖方信任的描述，然后单击**下一步**.  
![信赖方](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon2.PNG)

5. 在“配置标识符”  页上，为此信赖方指定一个或多个标识符、单击“添加”  以将其添加到列表中，然后单击“下一步”  。  
![信赖方](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon3.PNG)

6.  上**选择访问控制策略**选择一个策略，然后单击**下一步**。  有关访问控制策略的详细信息，请参阅[AD FS 中的访问控制策略](Access-Control-Policies-in-AD-FS.md)。 
![信赖方](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon4.PNG)

7. 在“准备好添加信任”  页上，复查设置，然后单击“下一步”  来保存信赖方信任的信息。  
   ![信赖方](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon5.PNG) 

8. 在“完成”  页面上，单击“关闭”  。 执行此操作会自动显示“编辑声明规则”对话框  。  
![信赖方](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon6.PNG)  
  
## <a name="see-also"></a>请参阅  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 

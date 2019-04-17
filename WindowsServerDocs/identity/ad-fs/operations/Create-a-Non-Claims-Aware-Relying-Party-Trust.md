---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: "创建非索赔注意到信赖聚会信任"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f46675ff4c471af743fd8782c1e3036e7c546256
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>创建非声明感知依赖聚会信任

>适用于：Windows Server 2016，Windows Server 2012 R2

在广告 FS 管理 snap\ 在、 non\ claims\ 感知依赖方信任是创建代表联合身份验证服务和单个 web\ 基于应用程序不 claims\ 感知和访问 Web 应用程序代理之间信任的对象。  
  
Non\ claims\ 感知信赖方信任是信赖的方信任，其中包括标识符、 名称和规则身份验证和授权的访问信赖的方信任通过 Web 应用程序代理时。 这些 web\ 基于应用程序，不依赖于索赔，换句话说，基于 Windows 的集成 Authentication\ 这些应用程序，可以具有授权规则强制访问通过 Web 应用程序代理公司的网络之外时基于索赔的访问。  
  
若要使用广告 FS 管理 snap\ 中添加新 non\ claims\ 感知信赖方信任，，请执行以下步骤。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
##<a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>若要手动创建非索赔感知依赖方信任 
1. 在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  下**操作**，单击**添加依赖方信任**。  
![依赖方](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  在**欢迎**页面上，选择**非索赔注意到**单击**开始**。  
![依赖方](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon1.PNG) 
  
4.  在**指定的显示名称**页上中, 键入一个名称**显示名称**下**笔记**键入这个信赖的方信任的说明，然后单击**下一步**。  
![依赖方](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon2.PNG)

5. 在**配置标识符**页、 为该信赖方指定一个或多个标识符上，单击**添加**以将其添加到列表，然后单击**下一步**。  
![依赖方](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon3.PNG)

6.  在**选择访问控制策略**选择某个策略，然后单击**下一步**。  有关访问控件策略的详细信息，请参阅[广告 FS 中访问控制策略](Access-Control-Policies-in-AD-FS.md)。 
![依赖方](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon4.PNG)

7. 在**添加信任准备已就绪**页上，查看设置，，然后单击**下一步**保存你依赖方信任的信息。  
   ![依赖方](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon5.PNG) 

8. 在**完成**页上，单击**关闭**。 此操作将自动显示**编辑索赔规则**对话框。  
![依赖方](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon6.PNG)  
  
## <a name="see-also"></a>请参阅  
[广告 FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 

---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: 创建非声明感知信赖方信任
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b0ea877170a07db6abe9ac82e72d1722600ec933
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358108"
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>创建非声明感知信赖方信任


在 AD FS 管理 snap @ no__t-0in 中，非 @ no__t-1claims @ no__t-2aware 信赖方信任是创建的对象，用于表示联合身份验证服务与非声明 @ no__t-3based 的单个 web @ no__t 应用程序之间的信任关系，以及通过 Web 应用程序代理进行访问。  
  
非 @ no__t-0claims @ no__t-1aware 信赖方信任是信赖方信任，由标识符、名称和用于身份验证和授权的规则组成，在通过 Web 应用程序代理访问信赖方信任时进行身份验证和授权。 这些不依赖于声明的 web @ no__t 0based 应用程序，换言之，这些集成的 Windows 身份验证 @ no__t 应用程序可以具有授权规则，这些规则可强制实施基于声明的访问通过 Web 应用程序代理的公司网络。  
  
若要添加新的非 @ no__t-0claims @ no__t-1aware 信赖方信任，请使用 AD FS 管理 snap @ no__t-2in，执行以下过程。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
## <a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>手动创建非声明感知信赖方信任 
1. 在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在 "**操作**" 下，单击 "**添加信赖方信任**"。  
![relying 参与方 @ no__t-1   

3.  在**欢迎**页上，选择 "**不感知声明**"，然后单击 "**启动**"。  
![relying 参与方 @ no__t-1 
  
4.  在 "**指定显示名称**" 页上，在 "**显示名称**" 中的 "**备注**" 下键入此信赖方信任的描述，然后单击 "**下一步**"。  
![relying 参与方 @ no__t-1

5. 在“配置标识符”页上，为此信赖方指定一个或多个标识符、单击“添加”以将其添加到列表中，然后单击“下一步”。  
![relying 参与方 @ no__t-1

6.  在 "**选择访问控制策略**" 中选择一个策略，然后单击 "**下一步**"。  有关访问控制策略的详细信息，请参阅[AD FS 中的访问控制策略](Access-Control-Policies-in-AD-FS.md)。 
![relying 参与方 @ no__t-1

7. 在“准备好添加信任” 页上，复查设置，然后单击“下一步” 来保存信赖方信任的信息。  
   ![relying 参与方 @ no__t-1 

8. 在“完成”页面上，单击“关闭”。 执行此操作会自动显示“编辑声明规则”对话框。  
![relying 参与方 @ no__t-1  
  
## <a name="see-also"></a>请参阅  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 

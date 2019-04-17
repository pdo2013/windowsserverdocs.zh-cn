---
ms.assetid: 5728847d-dcef-4694-9080-d63bfb1fe24b
title: "广告 FS 中访问控制策略"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0b4549192bc4dab9edf3b2a10421325144ed0cd2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>在 Windows Server 2012 R2 和 Windows Server 2012 广告 FS 访问控制策略

>适用于： Windows Server 2012 R2 和 Windows Server 2012 

这篇文章中所述的策略请使用两种类型的索赔  
  
1.  索赔广告 FS 创建根据广告金融服务和 Web 应用程序代理可以检查和验证，如 IP 地址的客户端直接连接到广告 FS 或 WAP 的信息。  
  
2.  索赔广告 FS 创建根据转发给广告 FS 客户端 HTTP 标题以信息  
  
>**重要**： 的策略，述下方将阻止 Windows 10 域加入和需要访问以下其他端点的方案上登录

广告 FS 端点和所需的 Windows 10 域加入登录上
- [联合身份验证服务的姓名] / adfs/服务/信任/2005年/windowstransport
- [联合身份验证服务的姓名] / adfs/服务/信任月 13/windowstransport
- [联合身份验证服务的姓名] / adfs/服务/信任/2005年/usernamemixed
- [联合身份验证服务的姓名] / adfs/服务/信任月 13/usernamemixed 
- [联合身份验证服务的姓名] / adfs/服务/信任/2005年/certificatemixed
- [联合身份验证服务的姓名] / adfs/服务/信任月 13/certificatemixed

若要解决，更新拒绝基于允许上述端点的例外端点索赔任何策略。

例如，规则下方：

`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  

将更新为：

`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  此类别中的索赔应仅用于实施业务策略，而不是安全策略，以保护你的网络的访问权限。  它有可能未授权的客户端发送标题虚假信息的方法来访问。  
  
始终应与另一种身份验证方法，如用户名和密码或多个因素身份验证这篇文章中所述的策略。  
  
## <a name="client-access-policies-scenarios"></a>客户端访问策略方案  
  
|**方案**|**描述**| 
| --- | --- | 
|方案 1： 阻止所有访问外部 Office 365|Office 365 访问允许从所有客户端内部公司的网络，但外部客户端的请求被拒绝根据外部客户端的 IP 地址。|  
|方案 2： 阻止所有访问外部 Exchange ActiveSync 除的 Office 365|从所有客户端内部公司的网络，以及从任何客户端的外部设备，如智能手机，请使用的 Exchange ActiveSync 允许 office 365 访问。 阻止所有其他外部客户端，如使用 Outlook，这些。|  
|方案 3： 阻止所有外部访问 Office 365 除基于浏览器的应用程序|阻止外部 Office 365、 对访问如 Outlook Web 访问或 SharePoint Online 被动 （基于浏览器） 的应用除外。|  
|方案 4： 阻止所有外部访问 Office 365 除外指定 Active Directory 组|这种情况下用于测试和验证客户端访问策略部署。 它仅用于一个或多个 Active Directory 组成员可以阻止外部访问 Office 365。 此外可以用于提供的一组成员仅外部的访问。|  
  
## <a name="enabling-client-access-policy"></a>启用客户端访问策略  
 若要使客户端访问策略，在 Windows Server 2012 R2 的广告 FS 中，您必须更新依赖方信任 Microsoft Office 365 身份平台。 选择其中一个示例方案下面配置上的索赔规则**Microsoft Office 365 身份平台**信赖方信任最适合你的组织的需求。  
  
###  <a name="scenario1"></a>方案 1： 阻止所有访问外部 Office 365  
 此客户端访问策略方案允许从所有内部客户端和块访问所有外部的客户端基于外部客户端的 IP 地址。 可以使用下面的过程依赖为你的选择方案方信任 Office 365 添加正确的发行授权规则。  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>若要创建规则阻止所有访问外部 Office 365  
  
1.  从**服务器管理器**，单击**工具**，然后单击**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS\Trust 关系**，单击**信赖方信任**，右键单击**Microsoft Office 365 身份平台**信任，，然后单击**编辑索赔规则**。  
  
3.  在**编辑索赔规则**对话框中，选择**颁发授权规则**选项卡，然后单击**添加规则**启动索赔规则向导。  
  
4.  上**选择规则模板**页面上下,**索赔规则模板**，选择**发送使用自定义规则索赔**，然后单击**下一步**。  
  
5.  在**配置规则**页面上下,**声明规则名称**，此规则，例如的显示名称"如果所需的范围之外的任何 IP 索赔拒绝"的类型。 下**自定义规则**、 键入或粘贴下面的索赔规则语言语法 （替换"x ms-转发的客户端-ip"有效的 IP expression 与上方的值）：  
`c1:[Type == " https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  单击**完成**。 验证该新规则列表中显示颁发授权规则之前为默认**允许所有用户访问**规则 （拒绝规则将优先即使早些时候在列表中显示）。  如果您没有允许访问规则默认值，你可以添加一个在你使用的索赔规则语言，如下所示的列表的末尾：  </br>
    
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true"); ` 

7.  若要在保存新规则，**编辑索赔规则**对话框中，单击**确定**。 结果列表中应该如下所示。  
  
     ![颁发 Auth 规则](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1")  
  
###  <a name="scenario2"></a>方案 2： 阻止所有访问外部 Exchange ActiveSync 除的 Office 365  
 下面的示例允许所有 Office 365 应用程序，包括来自内部包括 Outlook 的客户端的 Exchange Online 访问。 由的客户端 IP 地址，如智能手机的 Exchange ActiveSync 客户端除外，它会阻止来自客户端驻留以外的公司的网络的访问权限。  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>若要创建规则阻止所有访问外部 Exchange ActiveSync 除的 Office 365  
  
1.  从**服务器管理器**，单击**工具**，然后单击**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS\Trust 关系**，单击**信赖方信任**，右键单击**Microsoft Office 365 身份平台**信任，，然后单击**编辑索赔规则**。  
  
3.  在**编辑索赔规则**对话框中，选择**颁发授权规则**选项卡，然后单击**添加规则**启动索赔规则向导。  
  
4.  上**选择规则模板**页面上下,**索赔规则模板**，选择**发送使用自定义规则索赔**，然后单击**下一步**。  
  
5.  在**配置规则**页面上下,**声明规则名称**，键入的显示名称为此规则的示例"如果所需的范围之外的任何 IP 索赔发出 ipoutsiderange 索赔"。 下**自定义规则**、 键入或粘贴下面的索赔规则语言语法 （替换"x ms-转发的客户端-ip"有效的 IP expression 与上方的值）：  
    
    `c1:[Type == " https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  单击**完成**。 确认新规则中显示**颁发授权规则**列表。  
  
7.  接下来，在**编辑索赔规则**对话框中，在**颁发授权规则**选项卡上，单击**添加规则**要重新开始索赔规则向导。  
  
8.  上**选择规则模板**页面上下,**索赔规则模板**，选择**发送使用自定义规则索赔**，然后单击**下一步**。  
  
9. 在**配置规则**页面上下,**声明规则名称**，此规则，例如的显示名称"如果 IP 所需的范围之外，并且没有非 EAS x ms 的客户端应用程序索赔，拒绝"的类型。 下**自定义规则**、 键入或粘贴以下索赔规则语言语法：  
  

    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  
  
10. 单击**完成**。 确认新规则中显示**颁发授权规则**列表。  
  
11. 接下来，在**编辑索赔规则**对话框中，在**颁发授权规则**选项卡上，单击**添加规则**要重新开始索赔规则向导。  
  
12. 上**选择规则模板**页面上下,**索赔规则模板，**选择**发送使用自定义规则索赔**，然后单击**下一步**。  
  
13. 在**配置规则**页面上下,**声明规则名称**键入此规则的显示名称，例如"检查是否存在应用程序索赔"。 下**自定义规则**、 键入或粘贴以下索赔规则语言语法：  
  
    ```  
    NOT EXISTS([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");  
    ```  
  
14. 单击**完成**。 确认新规则中显示**颁发授权规则**列表。  
  
15. 接下来，在**编辑索赔规则**对话框中，在**颁发授权规则**选项卡上，单击**添加规则**要重新开始索赔规则向导。  
  
16. 上**选择规则模板**页面上下,**索赔规则模板，**选择**发送使用自定义规则索赔**，然后单击**下一步**。  
  
17. 在**配置规则**页面上下,**声明规则名称**，键入此规则的显示名称，例如"拒绝 ipoutsiderange true 和应用程序用户无法"。 下**自定义规则**、 键入或粘贴以下索赔规则语言语法：  
  
`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://custom/xmsapplication", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`</br>  
18. 单击**完成**。 确认新规则下方以前规则立即显示，并且之前为默认允许所有用户访问规则颁发授权规则列表 （拒绝规则将优先即使早些时候在列表中显示） 中。  </br>如果您没有允许访问规则默认值，你可以添加一个在你使用的索赔规则语言，如下所示的列表的末尾：</br></br>      `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`</br></br>
19. 若要在保存新规则，**编辑索赔规则**对话框中，单击确定。 结果列表中应该如下所示。  
  
     ![颁发授权规则](media/Access-Control-Policies-W2K12/clientaccess2.png )  
  
###  <a name="scenario3"></a>方案 3： 阻止所有外部访问 Office 365 除基于浏览器的应用程序  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>若要创建规则阻止所有外部访问 Office 365 除基于浏览器的应用程序  
  
1.  从**服务器管理器**，单击**工具**，然后单击**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS\Trust 关系**，单击**信赖方信任**，右键单击**Microsoft Office 365 身份平台**信任，，然后单击**编辑索赔规则**。  
  
3.  在**编辑索赔规则**对话框中，选择**颁发授权规则**选项卡，然后单击**添加规则**启动索赔规则向导。  
  
4.  上**选择规则模板**页面上下,**索赔规则模板**，选择**发送使用自定义规则索赔**，然后单击**下一步**。  
  
5.  在**配置规则**页面上下,**声明规则名称**，键入的显示名称为此规则的示例"如果所需的范围之外的任何 IP 索赔发出 ipoutsiderange 索赔"。 下**自定义规则**、 键入或粘贴下面的索赔规则语言语法 （替换"x ms-转发的客户端-ip"有效的 IP expression 与上方的值）：  </br>
`c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`   
6.  单击**完成**。 确认新规则中显示**颁发授权规则**列表。  
  
7.  接下来，在**编辑索赔规则**对话框中，在**颁发授权规则**选项卡上，单击**添加规则**要重新开始索赔规则向导。  
  
8.  上**选择规则模板**页面上下,**索赔规则模板，**选择**发送使用自定义规则索赔**，然后单击**下一步**。  
  
9. 在**配置规则**页面上下,**声明规则名称**，此规则，例如的显示名称"如果所需的范围之外 IP 并且端点不可/adfs 月 1 拒绝"的类型。 下**自定义规则**、 键入或粘贴以下索赔规则语言语法：  
  
 
    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  
  
10. 单击**完成**。 验证该新规则列表中显示颁发授权规则之前为默认**允许所有用户访问**规则 （拒绝规则将优先即使早些时候在列表中显示）。  </br></br> 如果您没有允许访问规则默认值，你可以添加一个在你使用的索赔规则语言，如下所示的列表的末尾：  
  
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`
  
11. 若要在保存新规则，**编辑索赔规则**对话框中，单击**确定**。 结果列表中应该如下所示。  
  
     ![颁发](media/Access-Control-Policies-W2K12/clientaccess3.png)  
  
###  <a name="scenario4"></a>方案 4： 阻止所有外部访问 Office 365 除外指定 Active Directory 组  
 下面的示例中启用访问从内部基于 IP 地址的客户端。 它会阻止来自于驻留公司的网络之外，外部客户端 IP 地址的客户端的访问权限、 除外中指定的 Active Directory Group.Use 这些个人以下步骤来添加到正确的发行授权规则**Microsoft Office 365 身份平台**使用索赔规则向导信赖方信任：  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>若要创建规则除阻止 Office 365 中的所有外部访问命名 Active Directory 组  
  
1.  从**服务器管理器**，单击**工具**，然后单击**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS\Trust 关系**，单击**信赖方信任**，右键单击**Microsoft Office 365 身份平台**信任，，然后单击**编辑索赔规则**。  
  
3.  在**编辑索赔规则**对话框中，选择**颁发授权规则**选项卡，然后单击**添加规则**启动索赔规则向导。  
  
4.  上**选择规则模板**页面上下,**索赔规则模板**，选择**发送使用自定义规则索赔**，然后单击**下一步**。  
  
5.  在**配置规则**页面上下,**声明规则名称**，键入的显示名称为此规则的示例"如果所需的范围之外的任何 IP 索赔发出 ipoutsiderange 索赔。" 下**自定义规则**、 键入或粘贴下面的索赔规则语言语法 （替换"x ms-转发的客户端-ip"有效的 IP expression 与上方的值）：  
  
      
    `c1:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  单击**完成**。 确认新规则中显示**颁发授权规则**列表。  
  
7.  接下来，在**编辑索赔规则**对话框中，在**颁发授权规则**选项卡上，单击**添加规则**要重新开始索赔规则向导。  
  
8.  上**选择规则模板**页面上下,**索赔规则模板，**选择**发送使用自定义规则索赔**，然后单击**下一步**。  
  
9. 在**配置规则**页面上下,**声明规则名称**键入此规则的显示名称，例如"组 SID 检查"。 下**自定义规则**、 键入或粘贴下面的索赔规则语言语法 (替换"groupsid"与你所使用的广告组实际 SID):  
   
    `NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");`  

10. 单击**完成**。 确认新规则中显示**颁发授权规则**列表。  
  
11. 接下来，在**编辑索赔规则**对话框中，在**颁发授权规则**选项卡上，单击**添加规则**要重新开始索赔规则向导。  
  
12. 上**选择规则模板**页面上下,**索赔规则模板，**选择**发送使用自定义规则索赔**，然后单击**下一步**。  
  
13. 在**配置规则**页面上下,**声明规则名称**，键入此规则的显示名称，例如"拒绝无法使用 ipoutsiderange true 且 groupsid 用户"。 下**自定义规则**、 键入或粘贴以下索赔规则语言语法：  
   
    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://custom/groupsid", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  

14. 单击**完成**。 确认新规则下方以前规则立即显示，并且之前为默认允许所有用户访问规则颁发授权规则列表 （拒绝规则将优先即使早些时候在列表中显示） 中。  </br></br>如果您没有允许访问规则默认值，你可以添加一个在你使用的索赔规则语言，如下所示的列表的末尾：  
   
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`  

15. 若要在保存新规则，**编辑索赔规则**对话框中，单击确定。 结果列表中应该如下所示。  
  
     ![颁发](media/Access-Control-Policies-W2K12/clientaccess4.png)  
  
##  <a name="buildingip"></a>生成 IP 地址范围 expression  
 当前仅通过 Exchange Online，它对广告 FS 传递验证请求时填充标题设置 HTTP 标题填充 x ms-转发的客户端-ip 索赔。 声明值可能是以下情况之一：  
  
> [!NOTE]
>  Exchange Online 当前支持的只 IPV4 和不 IPV6 地址。  
  
-   一个 IP 地址： 直接连接到联机更换费用的客户端路由器的 IP 地址  
  
> [!NOTE]
>  -   公司的网络上的客户端的 IP 地址将显示为组织的站代理或网关的外部界面 IP 地址。  
> -   作为企业客户的内部或外部取决于 VPN 或 DA.配置的客户端，可能会通过 VPN，或通过 Microsoft DirectAccess (DA) 连接到公司的网络的客户端  
  
-   一个或多个 IP 地址： 当 Exchange Online 无法确定连接的客户端的 IP 地址，它将设置基于 x-转发-有关标头值，可以包含在基于 HTTP 请求和支持的许多客户端、 负载平衡和代理服务器市场上的标准标头的值。  
  
> [!NOTE]
>  1.  多个 IP 地址，表明客户 IP 地址和传递请求，每个代理服务器的地址会用逗号分隔。  
> 2.  在列表中并非将与相关的 Exchange Online 基础结构的 IP 地址。  
  
### <a name="regular-expressions"></a>常规表情  
 当你将需要匹配范围内的 IP 地址时，它有必要构建常规 expression 进行比较。 在下一系列中的步骤，我们将提供有关如何构建此类 expression 匹配下列地址范围 （请注意，你将需要更改这些示例匹配 IP 公共区域） 的示例：  
  
-   192.168.1.1 – 192.168.1.25  
  
-   10.0.0.1 – 10.0.0.14  
  
 首先，将匹配一个 IP 地址的基本模式如下所示： \b###\\.###\\.###\\.###\b  
  
 将扩展这种情况，我们可以匹配或 expression 使用两种不同的 IP 地址，如下所示： \b###\\.###\\.###\\.###\b 和 #124;\b###\\.###\\.###\\.###\b  
  
 因此，是一个示例要 （例如 192.168.1.1 或 10.0.0.1） 只是两个地址相匹配： \b192\\.168\\.1\\.1\b 和 #124;\b10\\.0\\.0\\.1\b  
  
 这将为你提供的技术，你可以通过它输入任意数量的地址。 其中的各种地址需要允许，例如 192.168.1.1 – 192.168.1.25，匹配必须完成按字符字符： \b192\\.168\\.1\\。([1 9] & #124; 1 [0 到 9] & #124; 2 [0 5]) \b  
  
 请注意以下问题：  
  
-   IP 地址视为字符串并不是一个数字。  
  
-   此规则分别，如下所示： \b192\\.168\\.1\\。  
  
-   此匹配 192.168.1 任何值的开始。  
  
-   以下匹配最终小数点后地址部分所需区域：  
  
    -   （[1 9] 匹配以地址 1 9  
  
    -   & #124; 1 [0 到 9] 匹配结束 10 19 中的地址  
  
    -   & #124;2[0-5]) 以 20 25 匹配地址  
  
-   注意，圆括号必须正确放置于，以便在不启动匹配的 IP 地址的其他部分。  
  
-   与好友 192 块，我们可以编写 10 块类似 expression: \b10\\.0\\.0\\。([1 9] & #124; 1 [0 4]) \b  
  
-   搭配制定它们，以下 expression 应该"192.168.1.1~25"和"10.0.0.1~14"所有地址： \b192\\.168\\.1\\。([1 9] & #124; 1 [0 到 9] & #124; 2 [0 5]) \b & #124;\b10\\.0\\.0\\。([1 9] & #124; 1 [0 4]) \b  
  
### <a name="testing-the-expression"></a>测试 Expression  
 Regex 表情会变得非常棘手，因此我们强烈建议使用 regex 验证工具。 如果您执行搜索"在线 regex expression builder"internet，你将找到将允许你试用示例数据针对你表情的多个好在线工具。  
  
 当测试 expression 时，很重要，了解如何希望必须匹配。 Exchange online 系统可能会发送许多 IP 地址，请用逗号分隔。 上面提供的表情将适用于此。 但是，请务必对此功能的看法测试 regex 表情时。 例如，用户可以使用下面的示例输入验证上方的示例：  
  
 192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20  
  
 10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1  
  
## <a name="claim-types"></a>声称类型  
 在 Windows Server 2012 R2 的广告 FS 提供信息请求上下文使用下面的索赔类型：  
  
### <a name="x-ms-forwarded-client-ip"></a>X MS-转发的客户端-IP  
 声明类型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`  
  
 此广告 FS 索赔表示"最佳企图"有助于确定发出请求用户（例如，Outlook 客户端）的 IP 地址。 此声明可能包含多个 IP 地址，其中包括每个请求转发的代理服务器的地址。  此声明填充 HTTP 从。 声明值可以是以下情况之一：  
  
-   一个 IP 地址-直接连接到联机更换费用的客户端路由器的 IP 地址  
  
> [!NOTE]
>  公司的网络上的客户端的 IP 地址将显示为组织的站代理或网关的外部界面 IP 地址。  
  
-   一个或多个 IP 地址  
  
    -   如果无法确定 Exchange Online 连接的客户端的 IP 地址，它将设置基于 x-转发-有关标头的值纳入 HTTP 基于非标准标题请求和支持的许多客户端、负载平衡和代理服务器市场上的值。  
  
    -   多个 IP 地址，表明客户 IP 地址和每个传递请求的代理服务器的地址会用逗号分隔。  
  
> [!NOTE]
>  不会出现在列表中与相关的 Exchange Online 基础结构的 IP 地址。  
  
> [!WARNING]
>  当前支持的只 IPV4 地址; Exchange Online它不支持 IPV6 地址。  
  
### <a name="x-ms-client-application"></a>X MS 的客户端应用程序  
 声明类型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`  
  
 此广告 FS 索赔表示协议终止的客户端，它松对应正在使用的应用程序使用。  此声明填充当前 HTTP 标题只能通过 Exchange Online，它对广告 FS 传递验证请求时填充标题设置。 具体取决于该应用程序，将为此声明的值下列情况之一：  
  
-   对于使用 Exchange 活动同步的设备，值为 Microsoft.Exchange.ActiveSync。  
  
-   Microsoft Outlook 客户端的使用可能会导致的任何以下值：  
  
    -   Microsoft.Exchange.Autodiscover  
  
    -   Microsoft.Exchange.OfflineAddressBook  
  
    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  
  
    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  
  
-   其他可能值的此标头如下：  
  
    -   Microsoft.Exchange.Powershell  
  
    -   Microsoft.Exchange.SMTP  
  
    -   Microsoft.Exchange.Pop  
  
    -   Microsoft.Exchange.Imap  
  
### <a name="x-ms-client-user-agent"></a>X-MS 的客户端的用户代理  
 声明类型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`  
  
 此广告 FS 索赔提供字符串代表客户端使用访问该服务的设备类型。 当客户想要阻止访问的某些设备（如特定类型的智能手机一样），可以使用此。  此声明的示例值包括（但不是限于）以下值。  
  
 下面是示例 x ms 用户代理值可能包含的客户端其 x-ms 的客户端应用程序是"Microsoft.Exchange.ActiveSync"  
  
-   涡流/1.0  
  
-   Apple-iPad1C1/812.1  
  
-   Apple-iPhone3C1/811.2  
  
-   Apple-iPhone/704.11  
  
-   Moto-DROID2/4.5.1  
  
-   SAMSUNGSPHD700/100.202  
  
-   Android / 0.3  
  
 还有可能此值为空。  
  
### <a name="x-ms-proxy"></a>X MS 代理  
 声明类型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`  
  
 此广告 FS 索赔表示请求已通过 Web 应用程序代理。  此声明填充由后端联合身份验证服务传递验证请求时填充标头的 Web 应用程序代理。 广告 FS 然后将其转换为索赔。  
  
 声明值是传递请求的 Web 应用程序代理 DNS 名称。  
  
### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork  
 声明类型： `https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`  
  
 上述 x-ms 代理类似声称类型，则此声明类型指示是否请求已通过 web 应用程序代理。 与 x ms 代理 insidecorporatenetwork 是使用真正的指示直接与从内部企业网络联合身份验证服务请求的布尔值。  
  
### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-端点-绝对路径 (活动 vs 被动)  
 声明类型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`  
  
 此声明类型可用于确定源自而不是"无源"（web 浏览器基于）客户端的"活动"（丰富）客户端的请求。 这使外部请求从如 Outlook Web Access、SharePoint Online 或 Office 365 门户时，将阻止来自丰富的客户端，如 Microsoft Outlook 请求允许浏览器基于应用程序。  
  
 此声明的值为收到此请求广告 FS 服务的名称。  
  
## <a name="see-also"></a>请参阅  
 [广告 FS 操作](../../ad-fs/AD-FS-2016-Operations.md)

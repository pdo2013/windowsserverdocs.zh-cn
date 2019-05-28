---
ms.assetid: 5728847d-dcef-4694-9080-d63bfb1fe24b
title: AD FS 中的访问控制策略
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13969958c9b4e0539993142d680cb6c34dc10750
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190249"
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>在 Windows Server 2012 R2 和 Windows Server 2012 AD FS 访问控制策略


本文中所述的策略进行使用的两种类型的声明  
  
1.  声明 AD FS 创建根据 AD FS 和 Web 应用程序代理可以检查和验证，如直接连接到 AD FS 或 WAP 客户端的 IP 地址的信息。  
  
2.  AD FS 声明创建根据 HTTP 标头作为客户端转发到 AD FS 的信息  
  
>**重要**:按如下所述的策略将阻止 Windows 10 域加入和登录过程需要访问以下额外的终结点的方案

AD FS 终结点上所需的 Windows 10 域加入和登录
- [联合身份验证服务名称] / adfs/services/信任/2005年/windowstransport
- [federation service name]/adfs/services/trust/13/windowstransport
- [联合身份验证服务名称] / adfs/services/信任/2005年/usernamemixed
- [联合身份验证服务名称] / adfs/services/信任/13/usernamemixed 
- [联合身份验证服务名称] / adfs/services/信任/2005年/certificatemixed
- [联合身份验证服务名称] / adfs/services/信任/13/certificatemixed

若要解决，更新的任何策略拒绝基于终结点声明，以允许更高版本的终结点的异常。

例如，以下规则：

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  

将更新为：

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  此类别中的声明仅应该用于实现业务策略，而不是安全策略来保护对你的网络访问。  未经授权的客户端发送错误的信息的标头作为一种方法来访问它。  
  
本文中所述的策略始终应该用于另一种身份验证方法，如用户名和密码或多重身份验证。  
  
## <a name="client-access-policies-scenarios"></a>客户端访问策略方案  
  
|**方案**|**说明**| 
| --- | --- | 
|方案 1：阻止对 Office 365 的所有外部访问|Office 365 访问权限允许从内部企业网络上的所有客户端，但从外部客户端的请求会被拒绝基于外部客户端的 IP 地址。|  
|方案 2：阻止对 Exchange ActiveSync 以外的 Office 365 的所有外部访问|从内部企业网络上的所有客户端以及从任何外部的客户端设备，如智能手机，利用 Exchange ActiveSync 允许 office 365 访问权限。 所有其他外部客户端，例如那些使用 Outlook 时，会被阻止。|  
|方案 3：阻止除基于浏览器的应用程序对 Office 365 的所有外部访问|阻止外部对 Office 365 访问 Outlook Web Access 或 SharePoint Online 等被动 （基于浏览器的） 应用程序除外。|  
|方案 4:阻止指定的 Active Directory 组除了对 Office 365 的所有外部访问|此方案中用于测试和验证客户端访问策略部署。 它仅为一个或多个 Active Directory 组的成员会阻止对 Office 365 外部访问。 此外可以用于提供仅对组成员的外部访问。|  
  
## <a name="enabling-client-access-policy"></a>启用客户端访问策略  
 若要启用 Windows Server 2012 R2 中的 AD FS 中的客户端访问策略，则必须更新信赖方信任 Microsoft Office 365 标识平台。 选择以下配置上的声明规则的示例方案之一**Microsoft Office 365 标识平台**最符合组织的需求的信赖方信任。  
  
###  <a name="scenario1"></a> 方案 1:阻止对 Office 365 的所有外部访问  
 此客户端访问策略方案允许访问所有内部客户端和块从基于外部客户端的 IP 地址的所有外部客户端。 可以使用以下过程将正确的颁发授权规则添加到 Office 365 信赖方信任所选方案。  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>若要创建规则以阻止对 Office 365 的所有外部访问  
  
1.  从**服务器管理器**，单击**工具**，然后单击**AD FS 管理**。  
  
2.  在控制台树中下, **AD fs\ 信任关系**，单击**信赖方信任**，右键单击**Microsoft Office 365 标识平台**信任，并单击**编辑声明规则**。  
  
3.  在中**编辑声明规则**对话框中，选择**颁发授权规则**选项卡，然后依次**添加规则**启动声明规则向导。  
  
4.  上**选择规则模板**页面上，在**声明规则模板**，选择**使用自定义规则发送声明**，然后单击**下一步**。  
  
5.  上**配置规则**页面上，在**声明规则名称**，此规则，例如显示名称"超出了所需的范围，任何 IP 声明是否拒绝"的类型。 下**自定义规则**，键入或粘贴以下声明规则语言语法 （替换上面的"x-ms-转发的客户端的 ip"是有效的 IP 表达式的值）：  
`c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  单击 **“完成”** 。 验证新规则显示在颁发授权规则列表之前为默认值**允许访问所有用户**（拒绝规则将优先即使看起来列表中前面） 的规则。  如果没有默认值允许访问规则，可以添加一个，如下所示使用声明规则语言列表的末尾：  </br>
    
    `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true"); ` 

7.  若要保存新规则，在**编辑声明规则**对话框中，单击**确定**。 结果列表应如以下所示。  
  
     ![颁发授权规则](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1")  
  
###  <a name="scenario2"></a> 方案 2:阻止对 Exchange ActiveSync 以外的 Office 365 的所有外部访问  
 以下示例允许访问所有 Office 365 应用程序，包括 Exchange Online，从内部客户端包括 Outlook。 由客户端 IP 地址，如智能手机的 Exchange ActiveSync 客户端除外，它会阻止从客户端驻留在企业网络外部访问。  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>若要创建规则以阻止对 Exchange ActiveSync 以外的 Office 365 的所有外部访问  
  
1.  从**服务器管理器**，单击**工具**，然后单击**AD FS 管理**。  
  
2.  在控制台树中下, **AD fs\ 信任关系**，单击**信赖方信任**，右键单击**Microsoft Office 365 标识平台**信任，并单击**编辑声明规则**。  
  
3.  在中**编辑声明规则**对话框中，选择**颁发授权规则**选项卡，然后依次**添加规则**启动声明规则向导。  
  
4.  上**选择规则模板**页面上，在**声明规则模板**，选择**使用自定义规则发送声明**，然后单击**下一步**。  
  
5.  上**配置规则**页面上，在**声明规则名称**，键入此规则的显示名称为示例"所需的范围之外的任何 IP 声明是否发出 ipoutsiderange 声明"。 下**自定义规则**，键入或粘贴以下声明规则语言语法 （替换上面的"x-ms-转发的客户端的 ip"是有效的 IP 表达式的值）：  
    
    `c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  单击 **“完成”** 。 验证新规则显示在**颁发授权规则**列表。  
  
7.  接下来，在**编辑声明规则**对话框中，在**颁发授权规则**选项卡上，单击**添加规则**再次启动声明规则向导。  
  
8.  上**选择规则模板**页面上，在**声明规则模板**，选择**使用自定义规则发送声明**，然后单击**下一步**。  
  
9. 上**配置规则**页面上，在**声明规则名称**，此规则，例如显示名称"所需的范围之外的 IP，并且没有非 EAS x ms-客户端应用程序声明，如果拒绝的类型"。 下**自定义规则**，键入或粘贴以下声明规则语言语法：  
  

    `c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  
  
10. 单击 **“完成”** 。 验证新规则显示在**颁发授权规则**列表。  
  
11. 接下来，在**编辑声明规则**对话框中，在**颁发授权规则**选项卡上，单击**添加规则**再次启动声明规则向导。  
  
12. 上**选择规则模板**页面上，在**声明规则模板**选择**使用自定义规则发送声明**，然后单击**下一步**。  
  
13. 上**配置规则**页面上，在**声明规则名称**中，键入此规则的显示名称，例如"检查是否存在应用程序声明"。 下**自定义规则**，键入或粘贴以下声明规则语言语法：  
  
    ```  
    NOT EXISTS([Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");  
    ```  
  
14. 单击 **“完成”** 。 验证新规则显示在**颁发授权规则**列表。  
  
15. 接下来，在**编辑声明规则**对话框中，在**颁发授权规则**选项卡上，单击**添加规则**再次启动声明规则向导。  
  
16. 上**选择规则模板**页面上，在**声明规则模板**选择**使用自定义规则发送声明**，然后单击**下一步**。  
  
17. 上**配置规则**页面上，在**声明规则名称**中，键入此规则的显示名称，例如"拒绝用户具有 ipoutsiderange true 和应用程序失败"。 下**自定义规则**，键入或粘贴以下声明规则语言语法：  
  
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/xmsapplication", Value == "fail"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`</br>  
18. 单击 **“完成”** 。 验证新规则紧下方上一个规则，然后之前为默认值允许访问所有用户中的规则 （拒绝规则将优先即使看起来列表中前面） 的颁发授权规则列表。  </br>如果没有默认值允许访问规则，可以添加一个，如下所示使用声明规则语言列表的末尾：</br></br>      `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`</br></br>
19. 若要保存新规则，在**编辑声明规则**对话框框中，单击确定。 结果列表应如以下所示。  
  
     ![颁发授权规则](media/Access-Control-Policies-W2K12/clientaccess2.png )  
  
###  <a name="scenario3"></a> 方案 3:阻止除基于浏览器的应用程序对 Office 365 的所有外部访问  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>若要创建规则以阻止除基于浏览器的应用程序对 Office 365 的所有外部访问  
  
1.  从**服务器管理器**，单击**工具**，然后单击**AD FS 管理**。  
  
2.  在控制台树中下, **AD fs\ 信任关系**，单击**信赖方信任**，右键单击**Microsoft Office 365 标识平台**信任，并单击**编辑声明规则**。  
  
3.  在中**编辑声明规则**对话框中，选择**颁发授权规则**选项卡，然后依次**添加规则**启动声明规则向导。  
  
4.  上**选择规则模板**页面上，在**声明规则模板**，选择**使用自定义规则发送声明**，然后单击**下一步**。  
  
5.  上**配置规则**页面上，在**声明规则名称**，键入此规则的显示名称为示例"所需的范围之外的任何 IP 声明是否发出 ipoutsiderange 声明"。 下**自定义规则**，键入或粘贴以下声明规则语言语法 （替换上面的"x-ms-转发的客户端的 ip"是有效的 IP 表达式的值）：  </br>
`c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`   
6.  单击 **“完成”** 。 验证新规则显示在**颁发授权规则**列表。  
  
7.  接下来，在**编辑声明规则**对话框中，在**颁发授权规则**选项卡上，单击**添加规则**再次启动声明规则向导。  
  
8.  上**选择规则模板**页面上，在**声明规则模板**选择**使用自定义规则发送声明**，然后单击**下一步**。  
  
9. 上**配置规则**页面上，在**声明规则名称**，此规则，例如显示名称"如果没有所需的范围之外的 IP 和终结点不是/adfs/ls，拒绝"的类型。 下**自定义规则**，键入或粘贴以下声明规则语言语法：  
  
 
    `c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  
  
10. 单击 **“完成”** 。 验证新规则显示在颁发授权规则列表之前为默认值**允许访问所有用户**（拒绝规则将优先即使看起来列表中前面） 的规则。  </br></br> 如果没有默认值允许访问规则，可以添加一个，如下所示使用声明规则语言列表的末尾：  
  
    `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`
  
11. 若要保存新规则，在**编辑声明规则**对话框中，单击**确定**。 结果列表应如以下所示。  
  
     ![颁发](media/Access-Control-Policies-W2K12/clientaccess3.png)  
  
###  <a name="scenario4"></a> 方案 4:阻止指定的 Active Directory 组除了对 Office 365 的所有外部访问  
 以下示例启用基于 IP 地址的内部客户端访问。 阻止来自客户端驻留在企业网络外部的外部客户端 IP 地址的访问，除了那些个人可以在指定的 Active Directory Group.Use 以下步骤来添加正确的颁发授权规则**Microsoft Office 365 标识平台**信赖方信任使用声明规则向导：  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>若要创建规则以阻止对 Office 365 的所有外部访问除指定 Active Directory 组  
  
1.  从**服务器管理器**，单击**工具**，然后单击**AD FS 管理**。  
  
2.  在控制台树中下, **AD fs\ 信任关系**，单击**信赖方信任**，右键单击**Microsoft Office 365 标识平台**信任，并单击**编辑声明规则**。  
  
3.  在中**编辑声明规则**对话框中，选择**颁发授权规则**选项卡，然后依次**添加规则**启动声明规则向导。  
  
4.  上**选择规则模板**页面上，在**声明规则模板**，选择**使用自定义规则发送声明**，然后单击**下一步**。  
  
5.  上**配置规则**页面上，在**声明规则名称**，键入此规则的显示名称为示例"所需的范围之外的任何 IP 声明是否发出 ipoutsiderange 声明。" 下**自定义规则**，键入或粘贴以下声明规则语言语法 （替换上面的"x-ms-转发的客户端的 ip"是有效的 IP 表达式的值）：  
  
      
    `c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  单击 **“完成”** 。 验证新规则显示在**颁发授权规则**列表。  
  
7.  接下来，在**编辑声明规则**对话框中，在**颁发授权规则**选项卡上，单击**添加规则**再次启动声明规则向导。  
  
8.  上**选择规则模板**页面上，在**声明规则模板**选择**使用自定义规则发送声明**，然后单击**下一步**。  
  
9. 上**配置规则**页面上，在**声明规则名称**中，键入此规则的显示名称，例如"检查组 SID"。 下**自定义规则**，键入或粘贴以下声明规则语言语法 (替换为"groupsid"与实际使用的 AD 组的 SID):  
   
    `NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");`  

10. 单击 **“完成”** 。 验证新规则显示在**颁发授权规则**列表。  
  
11. 接下来，在**编辑声明规则**对话框中，在**颁发授权规则**选项卡上，单击**添加规则**再次启动声明规则向导。  
  
12. 上**选择规则模板**页面上，在**声明规则模板**选择**使用自定义规则发送声明**，然后单击**下一步**。  
  
13. 上**配置规则**页面上，在**声明规则名称**中，键入此规则的显示名称，例如"拒绝用户具有 ipoutsiderange true 和 groupsid 失败"。 下**自定义规则**，键入或粘贴以下声明规则语言语法：  
   
    `c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/groupsid", Value == "fail"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  

14. 单击 **“完成”** 。 验证新规则紧下方上一个规则，然后之前为默认值允许访问所有用户中的规则 （拒绝规则将优先即使看起来列表中前面） 的颁发授权规则列表。  </br></br>如果没有默认值允许访问规则，可以添加一个，如下所示使用声明规则语言列表的末尾：  
   
    `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`  

15. 若要保存新规则，在**编辑声明规则**对话框框中，单击确定。 结果列表应如以下所示。  
  
     ![颁发](media/Access-Control-Policies-W2K12/clientaccess4.png)  
  
##  <a name="buildingip"></a> 生成 IP 地址范围表达式  
 X-ms-转发的客户端的 ip 声明填充从当前设置只能由 Exchange Online，它将身份验证请求传递给 AD FS 时填充该标头的 HTTP 标头。 声明的值可能是以下值之一：  
  
> [!NOTE]
>  Exchange Online 目前支持仅 IPV4 和不侦听 IPV6 地址。  
  
-   单个 IP 地址：直接连接到 Exchange Online 客户端的 IP 地址  
  
> [!NOTE]
>  -   企业网络上的客户端的 IP 地址将显示为组织的出站代理服务器或网关的外部接口 IP 地址。  
> -   通过 VPN 或通过 Microsoft DirectAccess (DA) 连接到公司网络的客户端作为内部企业客户端或根据配置的 VPN 或 DA.的外部客户端可能会出现  
  
-   一个或多个 IP 地址：在 Exchange Online 不能确定连接的客户端的 IP 地址，它将设置基于 x-转发-对于标头的值，可以包含在基于 HTTP 的请求和多个客户端，负载均衡器支持的非标准标头的值和市场上的代理。  
  
> [!NOTE]
>  1.  将由逗号分隔多个 IP 地址，指示客户端 IP 地址和传递请求，每个代理的地址。  
> 2.  Exchange Online 基础结构相关的 IP 地址将不在列表中。  
  
### <a name="regular-expressions"></a>正则表达式  
 如果必须以匹配的 IP 地址范围，有必要构造正则表达式进行比较。 在下一步的一系列步骤，我们将提供有关如何构造此类表达式以匹配以下地址范围 （请注意，您将需要更改这些示例，以便匹配你的公共 IP 范围） 的示例：  
  
-   192.168.1.1 – 192.168.1.25  
  
-   10.0.0.1 – 10.0.0.14  
  
 首先，将匹配单个 IP 地址的基本模式如下所示： \b###\\。 # # #\\。 # # #\\。 # # # \b  
  
 将此扩展，我们可以与匹配的 OR 表达式使用两个不同的 IP 地址，如下所示： \b###\\。 # # #\\。 # # #\\。 # # # \b&#124;\b###\\。 # # #\\。 # # #\\。 # # # \b  
  
 因此，是一个示例，只需两个地址 （例如 192.168.1.1 或 10.0.0.1） 相匹配： \b192\\.168\\.1\\。 1\b&#124;\b10\\.0\\.0\\。 1\b  
  
 这样，您可以按其输入任意数量的地址的方法。 其中一个地址的范围需要允许，例如 192.168.1.1 – 192.168.1.25，匹配必须完成字符的字符： \b192\\.168\\.1\\。 ([1-9]&#124;1 [0-9]&#124;2 [0-5]) \b  
  
 请注意以下事项：  
  
-   IP 地址被视为字符串并不是数字。  
  
-   该规则细分，如下所示： \b192\\.168\\.1\\。  
  
-   此匹配任何以 192.168.1 开头的值。  
  
-   以下匹配最终小数点后所需的地址部分的范围：  
  
    -   （[1-9] 匹配项以地址 1-9  
  
    -   &#124;1 [0-9] 匹配以 10-19 结尾的地址  
  
    -   &#124;以 20-25 2[0-5]) 匹配地址  
  
-   请注意，括号必须正确定位，以便不开始匹配的 IP 地址的其他部分。  
  
-   与匹配 192 块，我们可以编写 10 个块的类似表达式： \b10\\.0\\.0\\。 ([1-9]&#124;1 [0-4]) \b  
  
-   并将它们放在一起，下面的表达式应匹配"192.168.1.1~25"和"10.0.0.1~14"的所有地址： \b192\\.168\\.1\\。 ([1-9]&#124;1 [0-9]&#124;2 [0-5]) \b&#124;\b10\\.0\\.0\\。 ([1-9]&#124;1 [0-4]) \b  
  
### <a name="testing-the-expression"></a>测试表达式  
 正则表达式的表达式可能会变得十分复杂，因此，我们强烈建议使用正则表达式验证工具。 如果执行"在线正则表达式表达式生成器"的 internet 搜索，您会发现多个良好的联机实用工具，您可以试用您针对示例数据的表达式。  
  
 测试表达式时，必须了解出现的情况必须匹配。 Exchange online 系统可能会发送多个 IP 地址，用逗号隔开。 在上面提供的表达式将适用于此。 但是，务必测试您正则表达式的表达式时，思考一下这个。 例如，一个可以使用下面的示例输入验证上面的示例：  
  
 192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20  
  
 10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1  
  
## <a name="claim-types"></a>声明类型  
 Windows Server 2012 R2 中的 AD FS 提供了使用以下声明类型的请求上下文信息：  
  
### <a name="x-ms-forwarded-client-ip"></a>X-MS-Forwarded-Client-IP  
 声明类型： `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`  
  
 此 AD FS 声明所表示的次认定发出请求的 IP 地址的用户 （例如，Outlook 客户端） 的"最佳尝试"。 此声明可以包含多个 IP 地址，包括每个代理将请求转发地址。  通过 HTTP 填充此声明。 声明的值可以是以下值之一：  
  
-   单个 IP 地址-直接连接到 Exchange Online 的客户端的 IP 地址  
  
> [!NOTE]
>  企业网络上的客户端的 IP 地址将显示为组织的出站代理服务器或网关的外部接口 IP 地址。  
  
-   一个或多个 IP 地址  
  
    -   如果无法确定 Exchange Online 的连接的客户端的 IP 地址，它会设置值的基础 x 转发的标头，可以包含在基于 HTTP 的非标准标头请求和支持的多个客户端，负载均衡器的值和市场上的代理。  
  
    -   将由逗号分隔多个 IP 地址，该值指示客户端 IP 地址和每个传递请求的代理服务器的地址。  
  
> [!NOTE]
>  Exchange Online 基础结构相关的 IP 地址不会出现在列表中。  
  
> [!WARNING]
>  Exchange Online 目前支持仅的 IPV4 地址。它不支持 IPV6 地址。  
  
### <a name="x-ms-client-application"></a>X-MS-Client-Application  
 声明类型： `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`  
  
 此 AD FS 声明表示对应于正在使用的应用程序的松散的最终客户端使用的协议。  此声明填充从当前的 HTTP 标头只能通过 Exchange Online，填充标头将身份验证请求传递给 AD FS 时设置。 根据应用程序，此声明的值将是以下之一：  
  
-   对于使用 Exchange Active Sync 的设备，则值为 Microsoft.Exchange.ActiveSync。  
  
-   使用 Microsoft Outlook 客户端可能会导致任何以下值：  
  
    -   Microsoft.Exchange.Autodiscover  
  
    -   Microsoft.Exchange.OfflineAddressBook  
  
    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  
  
    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  
  
-   此标头的其他可能值包括：  
  
    -   Microsoft.Exchange.Powershell  
  
    -   Microsoft.Exchange.SMTP  
  
    -   Microsoft.Exchange.Pop  
  
    -   Microsoft.Exchange.Imap  
  
### <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent  
 声明类型： `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`  
  
 此 AD FS 声明提供表示客户端正在使用以访问服务的设备类型的字符串。 这可用来在客户想要阻止某些设备 （例如，特定类型的智能手机） 的访问权限时。  此声明的示例值包括 （但不限于） 以下值。  
  
 下面是示例的 x ms 用户代理值可能包含其 x-ms-客户端应用程序是"Microsoft.Exchange.ActiveSync"为客户端  
  
-   顶点/1.0  
  
-   Apple-iPad1C1/812.1  
  
-   Apple-iPhone3C1/811.2  
  
-   Apple-iPhone/704.11  
  
-   Moto-DROID2/4.5.1  
  
-   SAMSUNGSPHD700/100.202  
  
-   Android/0.3  
  
 还有可能此值为空。  
  
### <a name="x-ms-proxy"></a>X-MS-Proxy  
 声明类型： `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`  
  
 此 AD FS 声明指示请求了通过 Web 应用程序代理。  此声明将身份验证请求传递到后端联合身份验证服务时将填充标头的 Web 应用程序代理由填充。 AD FS 然后将其转换为声明。  
  
 声明的值是传递请求的 Web 应用程序代理的 DNS 名称。  
  
### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork  
 声明类型： `http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`  
  
 类似于上述 x-ms 代理声明类型，此声明类型指示请求是否已通过 web 应用程序代理。 与 x ms 代理不同 insidecorporatenetwork 是一个布尔值 True 指示直接对从公司网络内部联合身份验证服务的请求。  
  
### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-终结点的绝对路径 （活动与被动）  
 声明类型： `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`  
  
 此声明类型可以用于确定从与"被动"（web 浏览器基于的） 客户端的"活动"（丰富） 客户端发出的请求。 这使从基于浏览器的应用程序如 Outlook Web Access、 SharePoint Online，或 Office 365 门户会阻止来自如 Microsoft Outlook 的丰富客户端的请求而允许的外部请求。  
  
 声明的值是接收请求的 AD FS 服务的名称。  
  
## <a name="see-also"></a>请参阅  
 [AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)

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
ms.openlocfilehash: 43a42c211557a41400fada17baaab6a0d5ab822a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866102"
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>Windows Server 2012 R2 和 Windows Server 2012 中的访问控制策略 AD FS


本文中所述的策略使用两种类型的声明  

1.  声明 AD FS 基于 AD FS 和 Web 应用程序代理可以检查和验证的信息进行创建，例如直接连接到 AD FS 或 WAP 的客户端的 IP 地址。  

2.  AD FS 基于转发给客户端 AD FS 作为 HTTP 标头的信息创建声明  

>**重要提示**:下面所述的策略将阻止 Windows 10 域加入和登录方案，这些方案需要访问以下附加终结点

AD FS Windows 10 域加入和登录所需的终结点
- [联合身份验证服务名称]/adfs/services/trust/2005/windowstransport
- [联合身份验证服务名称]/adfs/services/trust/13/windowstransport
- [联合身份验证服务名称]/adfs/services/trust/2005/usernamemixed
- [联合身份验证服务名称]/adfs/services/trust/13/usernamemixed 
- [联合身份验证服务名称]/adfs/services/trust/2005/certificatemixed
- [联合身份验证服务名称]/adfs/services/trust/13/certificatemixed

>**重要提示**：仅应为 intranet 访问启用/adfs/services/trust/2005/windowstransport 和/adfs/services/trust/13/windowstransport 终结点，因为它们应为 intranet 终结点，这些终结点使用 HTTPS 上的 WIA 绑定。 向 extranet 公开它们可能会允许对这些终结点的请求绕过锁定保护。 应在代理上禁用这些终结点（即从 extranet 禁用）以保护 AD 帐户锁定。 

若要解决此问题，请根据终结点声明更新拒绝的任何策略，以允许针对上述终结点的异常。

例如，下面的规则：

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  

将更新为：

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  此类别中的声明只应用于实现业务策略，而不应作为保护网络访问权限的安全策略。  未经授权的客户端可以发送包含虚假信息的标头以获取访问权限。  

本文中所述的策略应始终与其他身份验证方法一起使用，例如用户名和密码或多重身份验证。  

## <a name="client-access-policies-scenarios"></a>客户端访问策略方案  

|**方案**|**说明**| 
| --- | --- | 
|方案 1：阻止对 Office 365 的所有外部访问|允许从公司内部网络上的所有客户端访问 Office 365，但会根据外部客户端的 IP 地址拒绝外部客户端发出的请求。|  
|方案 2：阻止对 Office 365 的所有外部访问（Exchange ActiveSync 除外）|可以从内部公司网络上的所有客户端以及通过使用 Exchange ActiveSync 的任何外部客户端设备（如智能手机）访问 Office 365。 所有其他外部客户端（如使用 Outlook 的客户端）都将被阻止。|  
|方案 3：阻止对 Office 365 的所有外部访问（基于浏览器的应用程序除外）|阻止对 Office 365 的外部访问，但不包括被动（基于浏览器）的应用程序（如 Outlook Web 访问或 SharePoint Online）。|  
|方案4：阻止对 Office 365 的所有外部访问（指定的 Active Directory 组除外）|此方案用于测试和验证客户端访问策略部署。 它仅阻止一个或多个 Active Directory 组的成员对 Office 365 的外部访问。 它还可以用于提供仅对组成员的外部访问权限。|  

## <a name="enabling-client-access-policy"></a>启用客户端访问策略  
 若要在 Windows Server 2012 R2 的 AD FS 中启用客户端访问策略，必须更新 Microsoft Office 365 标识平台信赖方信任。 选择以下示例方案之一，以配置最符合组织需求的**Microsoft Office 365 标识平台**信赖方信任上的声明规则。  

###  <a name="scenario1"></a>方案1：阻止对 Office 365 的所有外部访问  
 此客户端访问策略方案允许从所有内部客户端进行访问，并基于外部客户端的 IP 地址阻止所有外部客户端。 你可以使用以下过程将正确的颁发授权规则添加到所选方案的 Office 365 信赖方信任中。  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>创建规则以阻止对 Office 365 的所有外部访问  

1.  在**服务器管理器**中，单击 "**工具**"，然后单击 " **AD FS 管理**"。  

2.  在控制台树中的 " **AD Fs\ 信任关系**" 下，单击 "**信赖方信任**"，右键单击**Microsoft Office 365 "标识平台**信任"，然后单击 "**编辑声明规则**"。  

3.  在 "**编辑声明规则**" 对话框中，选择 "**颁发授权规则**" 选项卡，然后单击 "**添加规则**" 以启动声明规则向导。  

4.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "**使用自定义规则发送声明**"，然后单击 "**下一步**"。  

5.  在 "**配置规则**" 页上的 "**声明规则名称**" 下，键入此规则的显示名称，例如 "如果在所需范围之外有任何 IP 声明，拒绝"。 在 "**自定义规则**" 下，键入或粘贴以下声明规则语言语法（将上面的值替换为有效的 ip 表达式）：  
`c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  单击 **“完成”** 。 验证新规则是否出现在 "颁发授权规则" 列表中，然后再转到 "默认**允许访问所有用户**" 规则（"拒绝" 规则将优先，即使它在列表的前面显示）。  如果你没有默认的允许访问规则，则可以使用声明规则语言在列表末尾添加一个，如下所示：  </br>

    `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true"); ` 

7.  若要保存新规则，请在 "**编辑声明规则**" 对话框中，单击 **"确定"** 。 生成的列表应如下所示。  

     ![颁发身份验证规则](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1")  

###  <a name="scenario2"></a>方案2：阻止对 Office 365 的所有外部访问（Exchange ActiveSync 除外）  
 下面的示例允许从包括 Outlook 在内的内部客户端访问所有的 Office 365 应用程序，包括 Exchange Online。 它阻止从位于企业网络外部的客户端进行访问，如客户端 IP 地址所示，智能手机等 Exchange ActiveSync 客户端除外。  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>创建规则以阻止对 Office 365 的所有外部访问（Exchange ActiveSync 除外）  

1.  在**服务器管理器**中，单击 "**工具**"，然后单击 " **AD FS 管理**"。  

2.  在控制台树中的 " **AD Fs\ 信任关系**" 下，单击 "**信赖方信任**"，右键单击**Microsoft Office 365 "标识平台**信任"，然后单击 "**编辑声明规则**"。  

3.  在 "**编辑声明规则**" 对话框中，选择 "**颁发授权规则**" 选项卡，然后单击 "**添加规则**" 以启动声明规则向导。  

4.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "**使用自定义规则发送声明**"，然后单击 "**下一步**"。  

5.  在 "**配置规则**" 页上的 "**声明规则名称**" 下，键入此规则的显示名称，例如 "如果在所需范围外有任何 IP 声明，请颁发 ipoutsiderange 声明"。 在 "**自定义规则**" 下，键入或粘贴以下声明规则语言语法（将上面的值替换为有效的 ip 表达式）：  

    `c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  单击 **“完成”** 。 验证新规则是否出现在 "**颁发授权规则**" 列表中。  

7.  接下来，在 "**编辑声明规则**" 对话框中的 "**颁发授权规则**" 选项卡上，单击 "**添加规则**" 以重新启动声明规则向导。  

8.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "**使用自定义规则发送声明**"，然后单击 "**下一步**"。  

9. 在 "**配置规则**" 页上的 "**声明规则名称**" 下，键入此规则的显示名称，例如 "如果在所需范围外有一个 IP，并且有一个非 EAS x ms-客户端应用程序声明，拒绝"。 在 "**自定义规则**" 下，键入或粘贴以下声明规则语言语法：  


~~~
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  
~~~

10. 单击 **“完成”** 。 验证新规则是否出现在 "**颁发授权规则**" 列表中。  

11. 接下来，在 "**编辑声明规则**" 对话框中的 "**颁发授权规则**" 选项卡上，单击 "**添加规则**" 以重新启动声明规则向导。  

12. 在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "**使用自定义规则发送声明**"，然后单击 "**下一步**"。  

13. 在 "**配置规则**" 页上的 "**声明规则名称**" 下，键入此规则的显示名称，例如 "检查是否存在应用程序声明"。 在 "**自定义规则**" 下，键入或粘贴以下声明规则语言语法：  

   ```  
   NOT EXISTS([Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");  
   ```  

14. 单击 **“完成”** 。 验证新规则是否出现在 "**颁发授权规则**" 列表中。  

15. 接下来，在 "**编辑声明规则**" 对话框中的 "**颁发授权规则**" 选项卡上，单击 "**添加规则**" 以重新启动声明规则向导。  

16. 在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "**使用自定义规则发送声明**"，然后单击 "**下一步**"。  

17. 在 "**配置规则**" 页上的 "**声明规则名称**" 下，键入此规则的显示名称，例如 "拒绝具有 ipoutsiderange 的用户和应用程序失败"。 在 "**自定义规则**" 下，键入或粘贴以下声明规则语言语法：  

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/xmsapplication", Value == "fail"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`</br>  
18. 单击 **“完成”** 。 验证新规则显示在 "颁发授权规则" 列表中的 "默认允许访问所有用户" 规则之前，并显示在 "颁发授权规则" 列表中的 "默认允许访问所有用户" 规则之前。  </br>如果你没有默认的允许访问规则，则可以使用声明规则语言在列表末尾添加一个，如下所示：</br></br>      `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`</br></br>
19. 若要保存新规则，请在 "**编辑声明规则**" 对话框中，单击 "确定"。 生成的列表应如下所示。  

    ![颁发授权规则](media/Access-Control-Policies-W2K12/clientaccess2.png )  

###  <a name="scenario3"></a>方案3：阻止对 Office 365 的所有外部访问（基于浏览器的应用程序除外）  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>创建规则以阻止对 Office 365 的所有外部访问（基于浏览器的应用程序除外）  

1.  在**服务器管理器**中，单击 "**工具**"，然后单击 " **AD FS 管理**"。  

2.  在控制台树中的 " **AD Fs\ 信任关系**" 下，单击 "**信赖方信任**"，右键单击**Microsoft Office 365 "标识平台**信任"，然后单击 "**编辑声明规则**"。  

3.  在 "**编辑声明规则**" 对话框中，选择 "**颁发授权规则**" 选项卡，然后单击 "**添加规则**" 以启动声明规则向导。  

4.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "**使用自定义规则发送声明**"，然后单击 "**下一步**"。  

5.  在 "**配置规则**" 页上的 "**声明规则名称**" 下，键入此规则的显示名称，例如 "如果在所需范围外有任何 IP 声明，请颁发 ipoutsiderange 声明"。 在 "**自定义规则**" 下，键入或粘贴以下声明规则语言语法（将上面的值替换为有效的 ip 表达式）：  </br>
`c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`   
6.  单击 **“完成”** 。 验证新规则是否出现在 "**颁发授权规则**" 列表中。  

7.  接下来，在 "**编辑声明规则**" 对话框中的 "**颁发授权规则**" 选项卡上，单击 "**添加规则**" 以重新启动声明规则向导。  

8.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "**使用自定义规则发送声明**"，然后单击 "**下一步**"。  

9. 在 "**配置规则**" 页上的 "**声明规则名称**" 下，键入此规则的显示名称，例如 "如果在所需范围外有 IP，并且终结点不是/adfs/ls，deny"。 在 "**自定义规则**" 下，键入或粘贴以下声明规则语言语法：  


~~~
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  
~~~

10. 单击 **“完成”** 。 验证新规则是否出现在 "颁发授权规则" 列表中，然后再转到 "默认**允许访问所有用户**" 规则（"拒绝" 规则将优先，即使它在列表的前面显示）。  </br></br> 如果你没有默认的允许访问规则，则可以使用声明规则语言在列表末尾添加一个，如下所示：  

   `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`

11. 若要保存新规则，请在 "**编辑声明规则**" 对话框中，单击 **"确定"** 。 生成的列表应如下所示。  

    ![颁发](media/Access-Control-Policies-W2K12/clientaccess3.png)  

###  <a name="scenario4"></a>方案4：阻止对 Office 365 的所有外部访问（指定的 Active Directory 组除外）  
 以下示例启用了基于 IP 地址的内部客户端的访问。 它阻止从公司网络外部的客户端访问具有外部客户端 IP 地址的客户端，指定 Active Directory 组中的个人除外。使用以下步骤将正确的颁发授权规则添加到**使用声明规则向导 Microsoft Office 365 标识平台**信赖方信任：  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>创建规则以阻止对 Office 365 的所有外部访问，指定 Active Directory 组除外  

1.  在**服务器管理器**中，单击 "**工具**"，然后单击 " **AD FS 管理**"。  

2.  在控制台树中的 " **AD Fs\ 信任关系**" 下，单击 "**信赖方信任**"，右键单击**Microsoft Office 365 "标识平台**信任"，然后单击 "**编辑声明规则**"。  

3.  在 "**编辑声明规则**" 对话框中，选择 "**颁发授权规则**" 选项卡，然后单击 "**添加规则**" 以启动声明规则向导。  

4.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "**使用自定义规则发送声明**"，然后单击 "**下一步**"。  

5.  在 "**配置规则**" 页上的 "**声明规则名称**" 下，键入此规则的显示名称，例如 "如果在所需范围外有任何 IP 声明，请颁发 ipoutsiderange 声明"。 在 "**自定义规则**" 下，键入或粘贴以下声明规则语言语法（将上面的值替换为有效的 ip 表达式）：  


~~~
`c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  
~~~

6. 单击 **“完成”** 。 验证新规则是否出现在 "**颁发授权规则**" 列表中。  

7. 接下来，在 "**编辑声明规则**" 对话框中的 "**颁发授权规则**" 选项卡上，单击 "**添加规则**" 以重新启动声明规则向导。  

8. 在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "**使用自定义规则发送声明**"，然后单击 "**下一步**"。  

9. 在 "**配置规则**" 页上的 "**声明规则名称**" 下，键入此规则的显示名称，例如 "检查组 SID"。 在 "**自定义规则**" 下，键入或粘贴以下声明规则语言语法（将 "groupsid" 替换为所使用的 AD 组的实际 SID）：  

    `NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");`  

10. 单击 **“完成”** 。 验证新规则是否出现在 "**颁发授权规则**" 列表中。  

11. 接下来，在 "**编辑声明规则**" 对话框中的 "**颁发授权规则**" 选项卡上，单击 "**添加规则**" 以重新启动声明规则向导。  

12. 在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "**使用自定义规则发送声明**"，然后单击 "**下一步**"。  

13. 在 "**配置规则**" 页上的 "**声明规则名称**" 下，键入此规则的显示名称，例如 "拒绝用户具有 ipoutsiderange true 和 groupsid fail"。 在 "**自定义规则**" 下，键入或粘贴以下声明规则语言语法：  

   `c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/groupsid", Value == "fail"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  

14. 单击 **“完成”** 。 验证新规则显示在 "颁发授权规则" 列表中的 "默认允许访问所有用户" 规则之前，并显示在 "颁发授权规则" 列表中的 "默认允许访问所有用户" 规则之前。  </br></br>如果你没有默认的允许访问规则，则可以使用声明规则语言在列表末尾添加一个，如下所示：  

   `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`  

15. 若要保存新规则，请在 "**编辑声明规则**" 对话框中，单击 "确定"。 生成的列表应如下所示。  

     ![颁发](media/Access-Control-Policies-W2K12/clientaccess4.png)  

##  <a name="buildingip"></a>构建 IP 地址范围表达式  
 从当前由 Exchange Online 设置的 HTTP 标头中填充了 x ms 转发的客户端 ip 声明，这会在将身份验证请求传递到 AD FS 时填充标头。 声明的值可以是下列值之一：  

> [!NOTE]
>  Exchange Online 当前只支持 IPV4，而不支持 IPV6 地址。  

-   单个 IP 地址：直接连接到 Exchange Online 的客户端的 IP 地址  

> [!NOTE]
> - 企业网络上的客户端的 IP 地址将显示为组织的出站代理或网关的外部接口 IP 地址。  
>   -   通过 VPN 或 Microsoft DirectAccess （DA）连接到公司网络的客户端可以显示为内部企业客户端，也可以作为外部客户端出现，具体取决于 VPN 或 DA 的配置。  

-   一个或多个 IP 地址：当 Exchange Online 无法确定正在连接的客户端的 IP 地址时，它将基于 x 转发的标头的值设置值，这是基于 HTTP 的请求中可包含的一个非标准标头，许多客户端和负载均衡器都支持该标头。以及市场上的代理。  

> [!NOTE]
> 1. 多个 IP 地址（表示通过请求的每个代理的客户端 IP 地址和地址）将用逗号分隔。  
>    2. 与 Exchange Online 基础结构相关的 IP 地址将不会在列表中。  

### <a name="regular-expressions"></a>正则表达式  
 当必须与某个范围的 IP 地址匹配时，需要构造一个正则表达式来执行比较。 在接下来的一系列步骤中，我们将为如何构造此类表达式来匹配以下地址范围提供示例（请注意，必须更改这些示例以匹配公共 IP 范围）：  

- 192.168.1.1 –192.168.1.25  

- 10.0.0.1 –10.0.0.14  

  首先，将匹配单个 IP 地址的基本模式如下： \b # # #\\. # # #\\. # # #\\. # # # \b  

  扩展此项，我们可以将两个不同的 IP 地址与 OR 表达式匹配，如下所示\\： \b # #\\#. # #\\#. # # #&#124;.\\# # # \b \b # #\\#. # #\\#. # # #. # # # \b  

  因此，只匹配两个地址（如192.168.1.1 或10.0.0.1）的示例为： \b192\\. 168\\.1\\.1 \ b&#124;\b10\\.0 .0\\\\  

  这为你提供了可用于输入任意数量的地址的方法。 如果需要允许的地址范围（例如192.168.1.1 –192.168.1.25），则匹配必须按字符： \b192\\. 168\\.1\\进行。（[1-9]&#124;1 [0-9]&#124;2 [0-5]） \b  

  请注意以下事项：  

- IP 地址被视为字符串而不是数字。  

- 规则按如下方式分解： 168\\\\\b192 .1\\。  

- 这与任何以192.168.1 开头的值匹配。  

- 以下内容与最后一个小数点后面的地址部分所需的范围匹配：  

  -   （[1-9] 匹配以1-9 结尾的地址  

  -   &#124;1 [0-9] 匹配以10-19 结尾的地址  

  -   &#124;2 [0-5]）匹配以20-25 结尾的地址  

- 请注意，括号必须正确定位，以便不会开始匹配 IP 地址的其他部分。  

- 使用匹配的192块，我们可以为10块编写类似的表达式： \b10\\.0\\.0\\。（[1-9]&#124;1 [0-4]） \b  

- 将其放在一起，以下表达式应匹配 "192.168.1.1 ~ 25" 和 "10.0.0.1 ~ 14"： 168\\\\\b192 .1\\的所有地址。（[1-9]&#124;1 [0-9]&#124;2 [0-5]） \b&#124;\b10\\.0\\.0\\（[1-9]&#124;1 [0-4]） \b  

### <a name="testing-the-expression"></a>测试表达式  
 正则表达式表达式可能会变得相当复杂，因此强烈建议使用 regex 验证工具。 如果您在 internet 上搜索 "联机 regex 表达式生成器"，您将看到多个有效的联机实用程序，您可以使用这些实用程序来尝试使用示例数据。  

 测试表达式时，必须了解需要匹配的内容，这一点非常重要。 Exchange online 系统可能会发送多个 IP 地址，用逗号分隔。 上面提供的表达式适用于这种情况。 但是，在测试正则表达式时要考虑这一点。 例如，可以使用以下示例输入来验证上述示例：  

 192.168.1.1、192.168.1.2、192.169.1.1。 192.168.12.1、192.168.1.10、192.168.1.25、192.168.1.26、192.168.1.30、1192.168.1.20  

 10.0.0.1、10.0.0.5、10.0.0.10、10.0.1.0、10.0.1.1、110.0.0.1、10.0.0.14、10.0.0.15、10.0.0.10、10、0.0。1  

## <a name="claim-types"></a>声明类型  
 Windows Server 2012 R2 中的 AD FS 使用以下声明类型提供请求上下文信息：  

### <a name="x-ms-forwarded-client-ip"></a>X 毫秒-转发的客户端-IP  
 声明类型：`http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`  

 此 AD FS 声明在认定上表示一个 "最佳尝试"，该地址为发出请求的用户（例如，Outlook 客户端）的 IP 地址。 此声明可包含多个 IP 地址，包括转发请求的每个代理的地址。  此声明从 HTTP 填充。 声明的值可以是下列值之一：  

-   单个 IP 地址-直接连接到 Exchange Online 的客户端的 IP 地址  

> [!NOTE]
>  企业网络上的客户端的 IP 地址将显示为组织的出站代理或网关的外部接口 IP 地址。  

-   一个或多个 IP 地址  

    -   如果 Exchange Online 无法确定正在连接的客户端的 IP 地址，它将基于 x 转发的标头的值设置值，该标头可以包含在基于 HTTP 的请求中，并且受多个客户端、负载均衡器和市场上的代理。  

    -   指示客户端 IP 地址的多个 IP 地址和传递请求的每个代理的地址将用逗号分隔。  

> [!NOTE]
>  与 Exchange Online 基础结构相关的 IP 地址将不会出现在列表中。  

> [!WARNING]
>  Exchange Online 目前只支持 IPV4 地址;它不支持 IPV6 地址。  

### <a name="x-ms-client-application"></a>X-客户端-应用程序  
 声明类型：`http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`  

 此 AD FS 声明表示最终客户端使用的协议，该协议与所使用的应用程序松耦合。  此声明是从当前仅由 Exchange Online 设置的 HTTP 标头中填充的，它会在将身份验证请求传递到 AD FS 时填充标头。 根据应用程序的不同，此声明的值将是下列值之一：  

-   如果使用 Exchange Active Sync 的设备，则值为 ""。  

-   使用 Microsoft Outlook 客户端可能会导致以下任何值：  

    -   Microsoft. 自动发现  

    -   OfflineAddressBook  

    -   RPCMicrosoft. WebServices。  

    -   RPCMicrosoft. WebServices。  

-   此标头的其他可能的值包括：  

    -   Microsoft. Powershell  

    -   Microsoft. SMTP  

    -   "  

    -   Microsoft。  

### <a name="x-ms-client-user-agent"></a>X-MS-客户端-用户代理  
 声明类型：`http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`  

 此 AD FS 声明提供了一个字符串，表示客户端用于访问服务的设备类型。 当客户想要阻止某些设备（如智能手机）的访问时，可以使用此方法。  此声明的示例值包括（但不限于）以下值。  

 下面是一个示例，说明 x ms 用户代理值可能包含的客户端（其 x ms-客户端应用程序为 "  

- Vortex/1。0  

- Apple-iPad1C1/812。1  

- Apple-iPhone3C1/811。2  

- Apple-iPhone/704.11  

- Moto-DROID2/4.5。1  

- SAMSUNGSPHD700/100.202  

- Android/0。3  

  此值也可能为空。  

### <a name="x-ms-proxy"></a>X-MS-Proxy  
 声明类型：`http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`  

 此 AD FS 声明指示请求已通过 Web 应用程序代理。  此声明由 Web 应用程序代理填充，Web 应用程序代理在将身份验证请求传递到后端联合身份验证服务时填充标头。 然后 AD FS 将其转换为声明。  

 声明的值是传递请求的 Web 应用程序代理的 DNS 名称。  

### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork  
 声明类型：`http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`  

 类似于上述 x ms 代理声明类型，此声明类型指示请求是否已通过 web 应用程序代理。 与 x ms 代理不同，insidecorporatenetwork 是一个布尔值，它指示从企业网络内部直接向联合身份验证服务发出的请求。  

### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-终结点绝对路径（活动 vs 被动）  
 声明类型：`http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`  

 此声明类型可用于确定源自 "active" （胖）客户端的请求与 "被动" （基于 web 浏览器）客户端。 这样，就会阻止来自基于浏览器的应用程序（例如 Outlook Web 访问、SharePoint Online 或 Office 365 门户）的外部请求，同时会阻止来自丰富客户端（如 Microsoft Outlook）的请求。  

 声明的值是接收请求的 AD FS 服务的名称。  

## <a name="see-also"></a>请参阅  
 [AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)

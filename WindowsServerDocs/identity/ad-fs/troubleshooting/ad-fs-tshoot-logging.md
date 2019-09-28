---
title: AD FS 故障排除-审核事件和日志记录
description: 本文档介绍如何使用各种 AD FS 日志来解决问题
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5985fc022a084e0e36e12ea60f18d1650c8c6b51
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366198"
---
# <a name="ad-fs-troubleshooting---events-and-logging"></a>AD FS 故障排除-事件和日志记录
AD FS 提供了两个可用于故障排除的主要日志。  它们分别是：

- 管理日志
- 跟踪日志  
 
下面将介绍其中的每个日志。

## <a name="admin-log"></a>管理日志
管理日志提供了有关发生的问题以及在默认情况下启用的高级信息。

### <a name="to-view-the-admin-log"></a>查看管理日志
1.  打开事件查看器
2.  展开 "**应用程序和服务日志**"。
3.  展开**AD FS**。
4.  单击 "**管理员**"。

![审核增强功能](media/ad-fs-tshoot-logging/event1.PNG)  

## <a name="trace-log"></a>跟踪日志
跟踪日志是记录详细消息的位置, 在进行故障排除时, 将是最有用的日志。 由于可以在很短的时间内生成很多跟踪日志信息, 这可能会影响系统性能, 因此默认情况下禁用跟踪日志。 

### <a name="to-enable-and-view-the-trace-log"></a>启用和查看跟踪日志
1.  打开事件查看器
2.  右键单击 "**应用程序和服务日志**", 然后选择 "查看", 然后单击 "**显示分析和调试日志**"。  这会在左侧显示更多节点。
![审核增强功能](media/ad-fs-tshoot-logging/event2.PNG)  
3.  展开 AD FS 跟踪
4.  右键单击 "调试", 然后选择 "**启用日志**"。
![审核增强功能](media/ad-fs-tshoot-logging/event3.PNG)  


## <a name="event-auditing-information-for-ad-fs-on-windows-server-2016"></a>Windows Server 2016 上 AD FS 事件审核信息  
默认情况下, Windows Server 2016 中的 AD FS 已启用基本级别的审核。  使用基本审核时, 管理员将看到单个请求的5个或更少事件。  这会使管理员必须查看的事件数明显减少, 以便查看单个请求。   使用 PowerShell cmdlt 可以提高或降低审核级别:  

```PowerShell
Set-AdfsProperties -AuditLevel 
```

下表说明了可用的审核级别。  

|审核级别|PowerShell 语法|描述|  
|----- | ----- | ----- |
|无|Set-adfsproperties-AuditLevel None|审核处于禁用状态, 并且不会记录任何事件。|  
|Basic (默认值)|Set-adfsproperties-AuditLevel Basic|对于单个请求, 将只记录5个以上的事件|  
|Verbose|Set-adfsproperties-AuditLevel Verbose|将记录所有事件。  这会为每个请求记录大量的信息。|  
  
若要查看当前审核级别, 可以使用 PowerShell cmdlt:Set-adfsproperties。  
  
![审核增强功能](media/ad-fs-tshoot-logging/ADFS_Audit_1.PNG)  
  
使用 PowerShell cmdlt 可以提高或降低审核级别:Set-adfsproperties-AuditLevel。  
  
![审核增强功能](media/ad-fs-tshoot-logging/ADFS_Audit_2.png)  
  
## <a name="types-of-events"></a>事件类型  
AD FS 事件可以是不同的类型, 具体取决于 AD FS 处理的不同类型的请求。 每种类型的事件都具有与之关联的特定数据。  事件的类型可以在登录请求 (即令牌请求) 与系统请求 (包括提取配置信息的服务器调用) 之间进行区分。    

下表描述了事件的基本类型。  
  
|事件类型|事件 ID|描述| 
|----- | ----- | ----- | 
|全新凭据验证成功|1202|联合身份验证服务成功验证新凭据的请求。 这包括 WS-TRUST、WS 联合身份验证、SAML-P (生成 SSO 的第一个阶段) 和 OAuth 授权终结点。|  
|全新凭据验证错误|1203|联合身份验证服务上的新凭据验证失败的请求。 这包括 WS-TRUST、WS-FEDERATION、SAML-P (生成 SSO 的第一个阶段) 和 OAuth 授权终结点。|  
|应用程序令牌成功|1200|联合身份验证服务成功颁发安全令牌的请求。 对于 WS-FEDERATION, 在通过 SSO 项目处理请求时, 将记录 SAML P。 (如 SSO cookie)。|  
|应用程序令牌失败|1201|联合身份验证服务上的安全令牌颁发失败的请求。 对于 WS-FEDERATION, 在通过 SSO 项目处理请求时, 将记录 SAML P。 (如 SSO cookie)。|  
|密码更改请求成功|1204|联合身份验证服务已成功处理密码更改请求的事务。|  
|密码更改请求错误|1205|联合身份验证服务无法处理密码更改请求的事务。| 
|注销成功|1206|描述成功的注销请求。|  
|注销失败|1207|描述失败的注销请求。|  

## <a name="security-auditing"></a>安全审核
AD FS 服务帐户的安全审核有时可以帮助跟踪密码更新、请求/响应日志记录、请求上下文标头和设备注册结果的问题。  默认情况下, 禁用 AD FS 服务帐户的审核。

### <a name="to-enable-security-auditing"></a>启用安全审核
1. 单击 "开始", 依次指向 "**程序**"、"**管理工具**", 然后单击 "**本地安全策略**"。
2. 导航到“安全设置\本地策略\用户权限管理”文件夹，然后双击“生成安全审核”。
3. 在 "**本地安全设置**" 选项卡上, 验证是否列出了 "AD FS 服务帐户"。 如果不存在, 请单击 "添加用户或组" 并将其添加到列表中, 然后单击 "确定"。
4. 使用提升的权限打开命令提示符, 并运行以下命令以启用审核 auditpol/set/subcategory: "已生成应用程序"/failure: enable/success: enable
5. 关闭 "**本地安全策略**", 然后打开 AD FS 管理 "管理单元。
 
若要打开 "AD FS 管理" 管理单元, 请单击 "开始", 指向 "程序", 指向 "管理工具", 然后单击 "AD FS 管理"。
 
6. 在 "操作" 窗格中, 单击 "编辑联合身份验证服务属性"
7. 在 "联合身份验证服务属性" 对话框中, 单击 "事件" 选项卡。
8. 选中 "**成功审核**" 和 "**失败审核**" 复选框。
9. 单击“确定”。

![审核增强功能](media/ad-fs-tshoot-logging/event4.PNG)  
 
>[!NOTE]
>仅当在独立的成员服务器上 AD FS 时, 才使用以上说明。  如果 AD FS 是在域控制器上运行, 而不是使用本地安全策略, 则使用**组策略管理/林/域/域控制器**中的**默认域控制器策略**。  单击 "编辑", 导航到 "**计算机配置 \windows 设置 \ 本地 Rights Management 策略 \** "

## <a name="windows-communication-foundation-and-windows-identity-foundation-messages"></a>Windows Communication Foundation 和 Windows Identity Foundation 消息
除了跟踪日志记录, 有时你可能需要查看 Windows Communication Foundation (WCF) 和 Windows Identity Foundation (WIF) 消息, 以便解决问题。 这可以通过修改 AD FS 服务器上的**IdentityServer**文件来实现。 

此文件位于 **<% system root% > \Windows\ADFS**并采用 XML 格式。 此文件的相关部分如下所示: 
```
<!-- To enable WIF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="Microsoft.IdentityModel" switchValue="Off"> … </source>

<!-- To enable WCF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="System.ServiceModel" switchValue="Off" > … </source>
```


应用这些更改后, 请保存配置, 然后重新启动 AD FS 服务。 通过设置相应的开关启用这些跟踪后, 它们将显示在 Windows 事件查看器的 AD FS 跟踪日志中。

## <a name="correlating-events"></a>关联事件
最难解决的问题之一就是产生大量错误或调试事件的访问问题。

为帮助解决此情况, AD FS 通过使用名为 "活动 ID" 的唯一全局唯一标识符 (GUID), 将记录到事件查看器的所有事件关联到管理日志和调试日志中。 此 ID 是在令牌颁发请求最初向 web 应用程序 (对于使用被动请求者配置文件的应用程序) 或请求直接发送到声明提供程序 (对于使用 WS-TRUST 的应用程序) 发出的请求时生成的。 

![activityid](media/ad-fs-tshoot-logging/activityid1.png)

此活动 ID 在请求的整个持续时间内保持不变, 并记录为该请求的事件查看器中记录的每个事件的一部分。 这意味着：
 - 使用此活动 ID 筛选或搜索事件查看器有助于跟踪与令牌请求对应的所有相关事件
 - 同一活动 ID 记录在不同的计算机上, 使你能够对多台计算机上的用户请求进行故障排除, 如联合服务器代理 (FSP)
 - 如果 AD FS 请求以任何方式失败，则活动 ID 也会显示在用户的浏览器中，从而允许用户将此 ID 传达给技术支持或 IT 支持人员。

![activityid](media/ad-fs-tshoot-logging/activityid2.png)

为了帮助进行故障排除, AD FS 还会在 AD FS 服务器上每次令牌颁发过程失败时记录调用方 ID 事件。 此事件包含以下声明类型之一的声明类型和值, 前提是此信息已作为令牌请求的一部分传递到联合身份验证服务:
- http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountnameh
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upnh
- http://schemas.microsoft.com/ws/2008/06/identity/claims/upn
- http://schemas.xmlsoap.org/claims/UPN
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddressh
- http://schemas.microsoft.com/ws/2008/06/identity/claims/emailaddress 
- http://schemas.xmlsoap.org/claims/EmailAddress
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
- http://schemas.microsoft.com/ws/2008/06/identity/claims/name
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier 

调用方 ID 事件还会记录活动 ID, 以允许你使用该活动 ID 来筛选或搜索特定请求的事件日志。




## <a name="next-steps"></a>后续步骤

- [AD FS 疑难解答](ad-fs-tshoot-overview.md)

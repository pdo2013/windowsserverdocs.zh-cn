---
title: AD FS 进行故障排除-审核事件和日志记录
description: 本文档介绍如何使用不同的 AD FS 日志来解决问题
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 20e2d0747b98e7c7728230d0768506261f5b0d50
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825118"
---
# <a name="ad-fs-troubleshooting---events-and-logging"></a>AD FS 故障排除-事件和日志记录
AD FS 提供了可在故障排除的两个主日志。  它们分别是：

- 管理日志
- 跟踪日志  
 
每个这些日志将如下所述。

## <a name="admin-log"></a>管理日志
管理员日志提供有关问题的发生，并且默认情况下启用的高级信息。

### <a name="to-view-the-admin-log"></a>若要查看管理日志
1.  打开事件查看器
2.  展开**应用程序和服务日志**。
3.  展开**AD FS**。
4.  单击**管理员**。

![审核增强功能](media/ad-fs-tshoot-logging/event1.PNG)  

## <a name="trace-log"></a>跟踪日志
跟踪日志是详细的消息会记录和故障排除时，将最有用的日志的位置。 由于生成大量跟踪日志信息时可以在短时间，可能会影响系统性能，默认情况下禁用跟踪日志。 

### <a name="to-enable-and-view-the-trace-log"></a>若要启用和查看跟踪日志
1.  打开事件查看器
2.  右键单击**应用程序和服务日志**并选择视图，然后单击**显示分析和调试日志**。  这将显示在左侧的其他节点。
![审核增强功能](media/ad-fs-tshoot-logging/event2.PNG)  
3.  展开 AD FS 跟踪
4.  右键单击调试并选择**启用日志**。
![审核增强功能](media/ad-fs-tshoot-logging/event3.PNG)  


## <a name="event-auditing-information-for-ad-fs-on-windows-server-2016"></a>事件适用于 AD FS Windows Server 2016 上的审核信息  
默认情况下，Windows Server 2016 中的 AD FS 具有基本的启用审核。  使用基本审核，管理员将看到针对单个请求的 5 个事件。  这会标记显著缩短管理员具有查看，若要查看单个请求的事件数。   审核级别可以引发或降低使用 PowerShell 脚本：  

```PowerShell
Set-AdfsProperties -AuditLevel 
```

下表介绍了可用的审核级别。  

|审核级别|PowerShell 语法|描述|  
|----- | ----- | ----- |
|无|Set-adfsproperties-AuditLevel None|禁用审核并将记录任何事件。|  
|基本 （默认值）|Set-AdfsProperties - AuditLevel Basic|不能超过 5 个事件将记录的单个请求|  
|Verbose|Set-adfsproperties-AuditLevel 详细|将记录所有事件。  这将记录大量的每个请求的信息。|  
  
若要查看当前的审核级别，可以使用 PowerShell 脚本：Get-AdfsProperties.  
  
![审核增强功能](media/ad-fs-tshoot-logging/ADFS_Audit_1.PNG)  
  
审核级别可以引发或降低使用 PowerShell 脚本：Set-AdfsProperties -AuditLevel.  
  
![审核增强功能](media/ad-fs-tshoot-logging/ADFS_Audit_2.png)  
  
## <a name="types-of-events"></a>类型的事件  
AD FS 事件可以是基于处理的 AD FS 请求不同类型的不同类型。 每种类型的事件具有与之关联的特定数据。  与系统的请求 （包括提取配置信息的服务器调用），可以登录请求 （即令牌请求） 之间区别的事件的类型。    

下表描述了基本类型的事件。  
  
|事件类型|事件 ID|描述| 
|----- | ----- | ----- | 
|新凭据验证成功|1202|其中最新的凭据通过联合身份验证服务已成功验证请求。 这包括 Ws-trust、 WS 联合身份验证 SAML-P （若要生成 SSO 第一个阶段） 和 OAuth 授权终结点。|  
|新凭据验证错误|1203|新凭据验证联合身份验证服务的失败的请求。 这包括 Ws-trust、 WS 联合身份验证、 SAML-P （若要生成 SSO 第一个阶段） 和 OAuth 授权终结点。|  
|应用程序令牌成功|1200|其中的安全令牌的联合身份验证服务已成功发出的请求。 对于 WS 联合身份验证，SAML-P SSO 项目与处理请求时，记录此。 （例如 SSO cookie)。|  
|应用程序令牌失败|1201|安全令牌颁发联合身份验证服务的失败的请求。 对于 WS 联合身份验证，SAML-P SSO 项目与处理请求时，记录此。 （例如 SSO cookie)。|  
|密码更改请求成功|1204|联合身份验证服务已成功处理密码更改请求的其中一个事务。|  
|密码更改请求错误|1205|密码更改请求的其中一个事务失败由联合身份验证服务来处理。| 
|注销成功|1206|描述成功注销请求。|  
|注销失败|1207|描述失败的注销请求。|  

## <a name="security-auditing"></a>安全审核
AD FS 服务帐户的安全审核可以有时帮助您跟踪密码更新、 请求/响应日志记录、 请求上下文标头和设备注册结果的问题。  默认情况下禁用审核的 AD FS 服务帐户。

### <a name="to-enable-security-auditing"></a>若要启用安全审核
1.      单击开始，指向**程序**，依次指向**管理工具**，然后单击**本地安全策略**。
2.       导航到“安全设置\本地策略\用户权限管理”文件夹，然后双击“生成安全审核”。
3.       上**本地安全设置**选项卡上，验证是否列出了 AD FS 服务帐户。 如果不存在，单击添加用户或组并将其添加到列表中，，然后单击确定。
4.       使用提升的权限打开命令提示符并运行以下命令以启用审核 auditpol.exe /set /subcategory:"Application Generated"/failure:enable /success:enable 5。       关闭**本地安全策略**，然后打开 AD FS 管理管理单元中。
 
若要打开 AD FS 管理管理单元中，单击开始、 指向程序、 管理工具，然后单击 AD FS 管理。
 
6.       在操作窗格中单击编辑联合身份验证服务属性 7。       在联合身份验证服务属性对话框中，单击事件选项卡。8.       选择**成功审核**并**失败审核**复选框。
9.       单击“确定”。

![审核增强功能](media/ad-fs-tshoot-logging/event4.PNG)  
 
>[!NOTE]
>仅当 AD FS 是独立的成员服务器上时，使用上面的说明。  如果 AD FS 正在运行的域控制器上，而不是本地安全策略，使用**默认域控制器策略**位于**组策略管理/林/域/域控制器**。  单击编辑并导航到**计算机配置 \ 策略 \windows 设置 \ 安全设置 \ 本地策略 \ 用户权限管理**

## <a name="windows-communication-foundation-and-windows-identity-foundation-messages"></a>Windows Communication Foundation 和 Windows Identity Foundation 消息
除了跟踪日志记录，有时可能需要查看 Windows Communication Foundation (WCF) 和 Windows Identity Foundation (WIF) 消息，以便解决问题。 这可以通过修改**Microsoft.IdentityServer.ServiceHost.Exe.Config** AD FS 服务器上的文件。 

此文件位于 **< %系统根目录 %> \Windows\ADFS**和采用 XML 格式。 该文件的相关部分如下所示： 
```
<!-- To enable WIF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="Microsoft.IdentityModel" switchValue="Off"> … </source>

<!-- To enable WCF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="System.ServiceModel" switchValue="Off" > … </source>
```


应用这些更改后，保存配置，并重新启动 AD FS 服务。 通过设置适当的开关启用这些跟踪后，它们将出现在 Windows 事件查看器中的 AD FS 跟踪日志中。

## <a name="correlating-events"></a>对事件进行关联
若要进行故障排除，最困难的工作之一是生成大量错误或调试事件的访问权限问题。

若要帮助实现此目的，AD FS 关联所有事件记录到事件查看器中管理和调试日志，它们分别对应于特定的请求使用唯一全局唯一标识符 (GUID) 名为活动 id。 令牌颁发请求最初提供给 web 应用程序 （适用于使用被动请求者配置文件的应用程序） 或直接发送到的声明提供程序 （适用于应用程序使用 WS 信任） 的请求时，将生成此 ID。 

![activityid](media/ad-fs-tshoot-logging/activityid1.png)

此活动 ID 请求的整个持续时间内保持不变，并为每个事件的一部分记录在事件查看器为该请求记录。 这意味着：
 - 筛选或搜索事件查看器使用此 ID 可帮助跟踪的所有对应于令牌请求的相关的事件的活动
 - 跨不同的计算机，它允许您将跨多台计算机，例如联合身份验证服务器代理 (FSP) 故障排除的用户请求记录的相同活动 ID
 - 活动 ID 也会在用户的浏览器中 AD FS 请求失败，以任何方式，从而允许用户此 ID 才可支持人员或 IT 支持进行通信。

![activityid](media/ad-fs-tshoot-logging/activityid2.png)

为了帮助进行故障排除过程，AD FS 还将记录调用方 ID 事件时，令牌颁发过程失败的 AD FS 服务器上。 此事件包含的声明类型和值的假设此信息已传递给联合身份验证服务的令牌请求一部分的以下声明类型之一：
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

调用方 ID 事件还记录的活动 ID，以便您可以使用该活动 ID 来筛选或搜索事件日志中的特定请求。




## <a name="next-steps"></a>后续步骤

- [AD FS 进行故障排除](ad-fs-tshoot-overview.md)

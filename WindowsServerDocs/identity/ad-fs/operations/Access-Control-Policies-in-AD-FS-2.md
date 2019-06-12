---
ms.assetid: ''
title: Active Directory 联合身份验证服务 2.0 中的客户端访问控制策略
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 036d6d0543687e7f82caf3dfd2c3bb0b4a981181
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445050"
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>AD FS 2.0 中的客户端访问控制策略
在 Active Directory 联合身份验证服务 2.0 客户端访问策略，可限制或授予用户对资源的访问权限。  本文档介绍如何启用 AD FS 2.0 中的客户端访问策略以及如何配置最常见的方案。

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>启用 AD FS 2.0 中的客户端访问策略

若要启用客户端访问策略，请执行以下步骤。

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>第 1 步：安装 AD FS 服务器的 AD FS 2.0 更新汇总 2 包

下载[Active Directory 联合身份验证服务 (AD FS) 的更新汇总 2 2.0](https://support.microsoft.com/en-us/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0)包并将其安装在所有联合服务器和联合服务器代理。

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>步骤 2：添加五个声明规则到 Active Directory 声明提供方信任

一旦所有 AD FS 服务器和代理上都安装更新汇总 2 后，使用以下过程添加一组声明规则，它向策略引擎提供的新声明类型。

若要执行此操作，你将添加五个接受转换规则为每个新的请求上下文声明类型使用以下过程。

在 Active Directory 声明提供方信任，创建新的接受转换规则，通过每个新的请求上下文声明类型。

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>若要向 Active Directory 中添加声明规则为每个五个上下文的声明提供方信任的声明类型：


1. 单击开始、 指向程序、 管理工具，然后依次 AD FS 2.0 管理。
2. 在控制台树中，AD FS 2.0 \ 信任关系下, 单击声明提供方信任，右键单击 Active Directory，然后单击编辑声明规则。
3. 在编辑声明规则对话框中，选择接受转换规则选项卡，然后单击添加规则以启动规则向导。
4. 声明规则模板下的选择规则模板页上选择传递或筛选传入声明从列表中，然后单击下一步。
5. 在配置规则页上的声明规则名称下，键入此规则; 的显示名称在传入声明类型，键入下面的声明类型 URL，然后选择通过所有声明值的传递。</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. 若要验证规则，在列表中选择并单击编辑规则，然后单击查看规则语言。 声明规则语言应如下所示： `c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. 单击 Finish。
8. 在编辑声明规则对话框中，单击确定以保存规则。
9. 重复步骤 2 到 6 为每个剩余的四个声明类型如下所示，在创建所有五个规则之前创建的其他声明规则。

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


~~~
`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`
~~~

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>步骤 3:更新信赖方信任 Microsoft Office 365 标识平台

选择一个信赖方信任最能满足您的组织的需要在 Microsoft Office 365 标识平台上配置的声明规则的示例方案。

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>适用于 AD FS 2.0 的客户端访问策略方案
以下各节将介绍适用于 AD FS 2.0 中存在的方案

### <a name="scenario-1-block-all-external-access-to-office-365"></a>方案 1：阻止对 Office 365 的所有外部访问

此客户端访问策略方案允许访问所有内部客户端和块从基于外部客户端的 IP 地址的所有外部客户端。 在默认颁发授权规则允许访问所有用户生成的规则集。 可以使用以下过程以向 Office 365 信赖方信任添加颁发授权规则。

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>若要创建一个规则以阻止对 Office 365 的所有外部访问



1. 单击开始、 指向程序、 管理工具，然后依次 AD FS 2.0 管理。
2. 在控制台树中，AD FS 2.0 \ 信任关系下, 单击信赖方信任，右键单击 Microsoft Office 365 标识平台信任，然后单击编辑声明规则。 
3. 在编辑声明规则对话框中，选择颁发授权规则选项卡，然后单击添加规则以启动声明规则向导。
4. 声明规则模板下的选择规则模板页上选择发送声明使用自定义规则，，然后单击下一步。
5. 在配置规则页上的声明规则名称下，键入此规则的显示名称。 在自定义规则下键入或粘贴以下声明规则语言语法： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");` 
6. 单击 Finish。 验证新的规则立即出现以下颁发授权规则列表中的所有用户规则允许访问。
7. 若要保存此规则，在编辑声明规则对话框中，单击确定。

>[!NOTE]
>必须将上面的"公共 ip 地址正则表达式"的值替换为有效的 IP 表达式;请参阅构建的详细信息的 IP 地址范围表达式。


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>方案 2：阻止对 Exchange ActiveSync 以外的 Office 365 的所有外部访问

以下示例允许访问所有 Office 365 应用程序，包括 Exchange Online，从内部客户端包括 Outlook。 由客户端 IP 地址，如智能手机的 Exchange ActiveSync 客户端除外，它会阻止从客户端驻留在企业网络外部访问。 规则集是基于名为允许访问所有用户的默认颁发授权规则。 使用以下步骤将颁发授权规则添加到 Office 365 信赖方信任使用声明规则向导：

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>若要创建一个规则以阻止对 Office 365 的所有外部访问



1. 单击开始、 指向程序、 管理工具，然后依次 AD FS 2.0 管理。
2. 在控制台树中，AD FS 2.0 \ 信任关系下, 单击信赖方信任，右键单击 Microsoft Office 365 标识平台信任，然后单击编辑声明规则。 
3. 在编辑声明规则对话框中，选择颁发授权规则选项卡，然后单击添加规则以启动声明规则向导。
4. 声明规则模板下的选择规则模板页上选择发送声明使用自定义规则，，然后单击下一步。
5. 在配置规则页上的声明规则名称下，键入此规则的显示名称。 在自定义规则下键入或粘贴以下声明规则语言语法： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.Autodiscover"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.ActiveSync"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 单击 Finish。 验证新的规则立即出现以下颁发授权规则列表中的所有用户规则允许访问。
7. 若要保存此规则，在编辑声明规则对话框中，单击确定。

>[!NOTE]
>必须将上面的"公共 ip 地址正则表达式"的值替换为有效的 IP 表达式;请参阅构建的详细信息的 IP 地址范围表达式。

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>方案 3：阻止除基于浏览器的应用程序对 Office 365 的所有外部访问

规则集是基于名为允许访问所有用户的默认颁发授权规则。 使用以下步骤将颁发授权规则添加到信赖方信任声明规则向导使用 Microsoft Office 365 标识平台：

>[!NOTE]
>这种情况下不支持使用第三方代理由于客户端访问策略与被动 （基于 Web 的） 的请求标头的存在限制。

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>若要创建一个规则以阻止除基于浏览器的应用程序对 Office 365 的所有外部访问



1. 单击开始、 指向程序、 管理工具，然后依次 AD FS 2.0 管理。
2. 在控制台树中，AD FS 2.0 \ 信任关系下, 单击信赖方信任，右键单击 Microsoft Office 365 标识平台信任，然后单击编辑声明规则。 
3. 在编辑声明规则对话框中，选择颁发授权规则选项卡，然后单击添加规则以启动声明规则向导。
4. 声明规则模板下的选择规则模板页上选择发送声明使用自定义规则，，然后单击下一步。
5. 在配置规则页上的声明规则名称下，键入此规则的显示名称。 在自定义规则下键入或粘贴以下声明规则语言语法： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 单击 Finish。 验证新的规则立即出现以下颁发授权规则列表中的所有用户规则允许访问。
7. 若要保存此规则，在编辑声明规则对话框中，单击确定。

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>方案 4:阻止指定的 Active Directory 组对 Office 365 的所有外部访问

以下示例启用基于 IP 地址的内部客户端访问。 它会阻止从驻留在企业网络外部的外部客户端 IP 地址的客户端访问、 除外中指定的 Active Directory 组规则的个人集是基于名为允许访问的默认颁发授权规则所有用户。 使用以下步骤将颁发授权规则添加到信赖方信任声明规则向导使用 Microsoft Office 365 标识平台：

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>若要创建一个规则以阻止指定的 Active Directory 组对 Office 365 的所有外部访问



1. 单击开始、 指向程序、 管理工具，然后依次 AD FS 2.0 管理。
2. 在控制台树中，AD FS 2.0 \ 信任关系下, 单击信赖方信任，右键单击 Microsoft Office 365 标识平台信任，然后单击编辑声明规则。 
3. 在编辑声明规则对话框中，选择颁发授权规则选项卡，然后单击添加规则以启动声明规则向导。
4. 声明规则模板下的选择规则模板页上选择发送声明使用自定义规则，，然后单击下一步。
5. 在配置规则页上的声明规则名称下，键入此规则的显示名称。 在自定义规则下键入或粘贴以下声明规则语言语法： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 单击 Finish。 验证新的规则立即出现以下颁发授权规则列表中的所有用户规则允许访问。
7. 若要保存此规则，在编辑声明规则对话框中，单击确定。


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>在上述方案中使用的声明规则语言语法的说明

|                                                                                                   描述                                                                                                   |                                                                     声明规则语言语法                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              默认的 AD FS 规则应用于允许访问所有用户。 此规则应已存在于 Microsoft Office 365 标识平台信赖方信任颁发授权规则列表。              |                                  => issue(Type = "<https://schemas.microsoft.com/authorization/claims/permit>", Value = "true");                                   |
|                               此子句添加到新的自定义规则指定请求是否来自联合服务器代理 （即，它具有 x ms 代理标头）                                |                                                                                                                                                                    |
|                                                                                 建议所有规则都包括此。                                                                                  |                                    exists([Type == "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy>"])                                    |
|                                                         用于建立该请求来自定义可接受范围内的 IP 具有的客户端。                                                         | NOT exists([Type == "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip>", Value=~"customer-provided public ip address regex"]) |
|                                    此子句用于指定是否正在访问的应用程序不是 Microsoft.Exchange.ActiveSync 应拒绝请求。                                     |       NOT exists([Type == "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application>", Value=="Microsoft.Exchange.ActiveSync"])        |
|                                                      此规则可确定是否在调用通过 Web 浏览器中，并且将被拒绝。                                                      |              NOT exists([Type == "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path>", Value == "/adfs/ls/"])               |
| 此规则规定应拒绝 （基于 SID 值） 的特定 Active Directory 组的唯一用户。 将添加到此语句不意味着将允许一组用户，而不考虑位置。 |             exists([Type == "<https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid>", Value =~ "{Group SID value of allowed AD group}"])              |
|                                                                这是一所需的子句，以满足上述所有条件时发出拒绝。                                                                 |                                   => issue(Type = "<https://schemas.microsoft.com/authorization/claims/deny>", Value = "true");                                    |

### <a name="building-the-ip-address-range-expression"></a>生成 IP 地址范围表达式

X-ms-转发的客户端的 ip 声明填充从当前设置只能由 Exchange Online，它将身份验证请求传递给 AD FS 时填充该标头的 HTTP 标头。 声明的值可能是以下值之一：

>[!Note] 
>Exchange Online 目前支持仅 IPV4 和不侦听 IPV6 地址。

单个 IP 地址：直接连接到 Exchange Online 客户端的 IP 地址

>[!Note] 
>企业网络上的客户端的 IP 地址将显示为组织的出站代理服务器或网关的外部接口 IP 地址。

通过 VPN 或通过 Microsoft DirectAccess (DA) 连接到公司网络的客户端作为内部企业客户端或根据配置的 VPN 或 DA.的外部客户端可能会出现

一个或多个 IP 地址：在 Exchange Online 不能确定连接的客户端的 IP 地址，它将设置基于 x-转发-对于标头的值，可以包含在基于 HTTP 的请求和多个客户端，负载均衡器支持的非标准标头的值和市场上的代理。

>[!Note]
>将由逗号分隔多个 IP 地址，指示客户端 IP 地址和传递请求，每个代理的地址。

Exchange Online 基础结构相关的 IP 地址不会出现在列表中。


#### <a name="regular-expressions"></a>正则表达式

如果必须以匹配的 IP 地址范围，有必要构造正则表达式进行比较。 在下一步的一系列步骤，我们将提供有关如何构造此类表达式以匹配以下地址范围 （请注意，您将需要更改这些示例，以便匹配你的公共 IP 范围） 的示例：


- 192.168.1.1 – 192.168.1.25
- 10.0.0.1 – 10.0.0.14

首先，将匹配单个 IP 地址的基本模式如下所示： \b###\.###\.###\.# # # \b

将此扩展，我们可以与匹配的 OR 表达式使用两个不同的 IP 地址，如下所示： \b###\.###\.###\.# # # \b|\b###\. ### \. ### \.# # # \b

因此，是一个示例，只需两个地址 （例如 192.168.1.1 或 10.0.0.1） 相匹配： \b192\.168\.1\.1\b | \b10\.0\.0\.1\b

这样，您可以按其输入任意数量的地址的方法。 如果需要允许，例如 192.168.1.1 – 192.168.1.25，一个地址的范围匹配必须完成字符的字符： \b192\.168\.1\.([1-9] | [0-9] 1 | 2 [0-5]) \b

>[!Note] 
>IP 地址被视为字符串并不是数字。


该规则细分，如下所示： \b192\.168\.1\.

此匹配任何以 192.168.1 开头的值。

以下匹配最终小数点后所需的地址部分的范围：


- （[1-9] 匹配项以地址 1-9
- | 1 [0-9] 匹配以 10-19 结尾的地址
- 以 20-25 |2[0-5]) 匹配地址

>[!Note]
>括号必须正确定位，以便不开始匹配的 IP 地址的其他部分。

与匹配 192 块，我们可以编写 10 个块的类似表达式： \b10\.0\.0\.([1-9] | 1 [0-4]) \b

并将它们放在一起，下面的表达式应匹配"192.168.1.1~25"和"10.0.0.1~14"的所有地址： \b192\.168\.1\.([1-9] | [0-9] 1 | 2 [0-5]) \b|\b10\.0\.0\.([1-9] | 1 [0-4]) \b

#### <a name="testing-the-expression"></a>测试表达式

正则表达式的表达式可能会变得十分复杂，因此，我们强烈建议使用正则表达式验证工具。 如果执行"在线正则表达式表达式生成器"的 internet 搜索，您会发现多个良好的联机实用工具，您可以试用您针对示例数据的表达式。

测试表达式时，必须了解出现的情况必须匹配。 Exchange online 系统可能会发送多个 IP 地址，用逗号隔开。 在上面提供的表达式将适用于此。 但是，务必测试您正则表达式的表达式时，思考一下这个。 例如，一个可以使用下面的示例输入验证上面的示例： 

192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20 

10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1 











## <a name="validating-the-deployment"></a>验证部署

### <a name="security-audit-logs"></a>安全审核日志

若要验证新的请求上下文声明发送，并可供 AD FS 声明处理管道，请启用 AD FS 服务器上的日志记录的审核。 然后在标准的安全审核日志条目发送某些身份验证请求并检查有声明值。 

若要启用的审核安全事件日志的 AD FS 服务器的日志记录，请在 AD fs 2.0 配置审核的步骤。

### <a name="event-logging"></a>事件日志记录

默认情况下，失败的请求记录到应用程序事件日志位于 Applications and Services Logs \ AD FS 2.0 \ Admin.For 适用于 AD FS 事件日志记录的详细信息，请参阅[设置 AD FS 2.0 事件日志记录](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx)。

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>配置详细的 AD FS 跟踪日志

AD FS 跟踪事件将记录到 AD FS 2.0 的调试日志。 若要启用跟踪，请参阅[配置调试跟踪适用于 AD FS 2.0](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx)。

启用跟踪后，使用以下命令行语法来启用详细日志记录级别： wevtutil.exe sl"AD FS 2.0 跟踪/调试"/l:5  

## <a name="related"></a>相关内容
新的声明类型的详细信息请参阅[AD FS 声明类型](AD-FS-Claims-Types.md)。


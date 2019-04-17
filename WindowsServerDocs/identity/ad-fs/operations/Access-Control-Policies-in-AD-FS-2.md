---
ms.assetid: 
title: "客户端访问控制 Active Directory 联合身份验证服务 2.0 策略"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 00b9cd17c7a5c206e06bea12fd90762adb336715
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>在广告 FS 2.0 的客户端访问控制策略
在 Active Directory 联合身份验证服务 2.0 客户端访问策略允许你以限制或授权的用户访问资源。  本文介绍了如何使客户端访问策略，在广告 FS 2.0 以及如何配置最常见的场景。

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>启用广告 FS 2.0 策略客户端访问

若要使客户端访问策略，请按照以下步骤。

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>第 1 步： 安装广告 FS 2.0 更新汇总 2 打包广告 FS 服务器上

下载[Active Directory 联合身份验证服务 (广告 FS) 的更新汇总 2 2.0](https://support.microsoft.com/en-us/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0)打包并将它安装在所有联合身份验证的服务器和联盟服务器代理。

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>第 2 步： 添加五声称 Active Directory 索赔提供商信任到规则

一旦更新汇总 2 已安装所有广告 FS 服务器和代理服务器上，使用以下步骤添加使新的声明类型供策略引擎一套索赔规则。

若要执行此操作，你将会添加五个验收转换规则每个请求上下文索赔新型使用下面的过程。

上 Active Directory 索赔提供商信任，创建一个新的验收转换规则经过的每个新的请求上下文索赔类型。

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>若要添加到 Active Directory 的索赔规则索赔提供商信任的五个上下文每个声称类型：


1. 单击开始指向程序、 管理工具，请指向，然后单击广告 FS 2.0 管理。
2. 控制台树中下广告 FS 2.0\Trust 关系, 中,，单击索赔提供商信任、 Active Directory、 右键单击，然后单击编辑索赔规则。
3. 在编辑索赔规则对话框中，选择验收转换规则选项卡，然后单击添加规则开始规则向导。
4. 在选择规则模板页上下索赔规则模板中，, 选择穿透或筛选传入声明从列表中，，然后单击下一步。
5. 在配置规则页面本名规则索赔，键入的显示名称为此规则;在传入声称类型，键入以下索赔键入的 URL，，然后依次选择完成所有索赔值的 Pass。</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. 来验证规则，在列表中选择并单击编辑规则，，然后单击查看规则语言。 索赔规则语言应该如下所示： `c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. 单击完成。
8. 在编辑索赔规则对话框中，单击确定要将规则保存。
9. 重复步骤 2 到 6 创建其他索赔规则的每个剩余之前创建了所有五个规则下方显示的四个索赔类型。

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>步骤 3： 更新依赖方信任 Microsoft Office 365 身份平台

选择以下配置的索赔规则依赖方信任最适合你的组织的需求的 Microsoft Office 365 身份平台上的示例方案之一。

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>客户端访问策略方案中有关广告 FS 2.0
以下部分介绍了有关广告 FS 2.0 存在方案

### <a name="scenario-1-block-all-external-access-to-office-365"></a>方案 1： 阻止所有访问外部 Office 365

此客户端访问策略方案允许从所有内部客户端和块访问所有外部的客户端基于外部客户端的 IP 地址。 规则集版本上的默认颁发授权规则允许所有用户访问。 可以使用下面的过程中添加到依赖方信任 Office 365 颁发授权规则。

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>若要创建规则阻止所有访问外部 Office 365



1. 单击开始指向程序、 管理工具，请指向，然后单击广告 FS 2.0 管理。
2. 控制台树中下广告 FS 2.0\Trust 关系，, 单击信赖方信任，Microsoft Office 365 身份平台信任中，右键单击，然后单击编辑索赔规则。 
3. 在编辑索赔规则对话框中，依次选择颁发授权规则选项卡，然后单击添加规则开始索赔规则向导。
4. 在选择规则模板页上下索赔规则模板中，, 选择发送索赔使用自定义规则，，然后单击下一步。
5. 在配置规则页面本名规则索赔，键入的显示名称为此规则。 自定义规则下中，键入或粘贴以下索赔规则语言语法： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");` 
6. 单击完成。 确认新规则立即显示以下允许访问颁发授权规则列表中的所有用户规则。
7. 若要将规则，保存在编辑索赔规则对话框中，单击确定。

>[!NOTE]
>您将需要使用有效的 IP expression; 替换"公用 ip 地址 regex"上方的值请参阅生成 IP 地址范围 expression 详细信息。


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>方案 2： 阻止所有访问外部 Exchange ActiveSync 除的 Office 365

下面的示例允许所有 Office 365 应用程序，包括来自内部包括 Outlook 的客户端的 Exchange Online 访问。 由的客户端 IP 地址，如智能手机的 Exchange ActiveSync 客户端除外，它会阻止来自客户端驻留以外的公司的网络的访问权限。 规则集版本上默认颁发授权规则标题为允许所有用户访问。 使用以下步骤添加颁发授权规则依赖使用索赔规则向导方信任 Office 365:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>若要创建规则阻止所有访问外部 Office 365



1. 单击开始指向程序、 管理工具，请指向，然后单击广告 FS 2.0 管理。
2. 控制台树中下广告 FS 2.0\Trust 关系，, 单击信赖方信任，Microsoft Office 365 身份平台信任中，右键单击，然后单击编辑索赔规则。 
3. 在编辑索赔规则对话框中，依次选择颁发授权规则选项卡，然后单击添加规则开始索赔规则向导。
4. 在选择规则模板页上下索赔规则模板中，, 选择发送索赔使用自定义规则，，然后单击下一步。
5. 在配置规则页面本名规则索赔，键入的显示名称为此规则。 自定义规则下中，键入或粘贴以下索赔规则语言语法： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.Autodiscover"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.ActiveSync"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 单击完成。 确认新规则立即显示以下允许访问颁发授权规则列表中的所有用户规则。
7. 若要将规则，保存在编辑索赔规则对话框中，单击确定。

>[!NOTE]
>您将需要使用有效的 IP expression; 替换"公用 ip 地址 regex"上方的值请参阅生成 IP 地址范围 expression 详细信息。

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>方案 3： 阻止所有外部访问 Office 365 除基于浏览器的应用程序

规则集版本上默认颁发授权规则标题为允许所有用户访问。 使用以下步骤添加到依赖使用索赔规则向导方信任 Microsoft Office 365 身份平台颁发授权规则：

>[!NOTE]
>此项 scenario 不支持与第三方代理由于与被动 （基于 Web 的） 请求的客户端访问策略标题限制。

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>若要创建规则阻止所有外部访问 Office 365 除基于浏览器的应用程序



1. 单击开始指向程序、 管理工具，请指向，然后单击广告 FS 2.0 管理。
2. 控制台树中下广告 FS 2.0\Trust 关系，, 单击信赖方信任，Microsoft Office 365 身份平台信任中，右键单击，然后单击编辑索赔规则。 
3. 在编辑索赔规则对话框中，依次选择颁发授权规则选项卡，然后单击添加规则开始索赔规则向导。
4. 在选择规则模板页上下索赔规则模板中，, 选择发送索赔使用自定义规则，，然后单击下一步。
5. 在配置规则页面本名规则索赔，键入的显示名称为此规则。 自定义规则下中，键入或粘贴以下索赔规则语言语法： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 单击完成。 确认新规则立即显示以下允许访问颁发授权规则列表中的所有用户规则。
7. 若要将规则，保存在编辑索赔规则对话框中，单击确定。

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>方案 4： 阻止所有外部指定 Active Directory 组的 Office 365 访问

下面的示例中启用访问从内部基于 IP 地址的客户端。 它会阻止来自于驻留公司的网络之外，外部客户端 IP 地址的客户端的访问权限，除外中指定的 Active Directory Group.The 规则这些个人设置版本上默认颁发授权规则标题为允许所有用户访问。 使用以下步骤添加到依赖使用索赔规则向导方信任 Microsoft Office 365 身份平台颁发授权规则：

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>若要创建规则阻止所有外部指定 Active Directory 组的 Office 365 访问



1. 单击开始指向程序、 管理工具，请指向，然后单击广告 FS 2.0 管理。
2. 控制台树中下广告 FS 2.0\Trust 关系，, 单击信赖方信任，Microsoft Office 365 身份平台信任中，右键单击，然后单击编辑索赔规则。 
3. 在编辑索赔规则对话框中，依次选择颁发授权规则选项卡，然后单击添加规则开始索赔规则向导。
4. 在选择规则模板页上下索赔规则模板中，, 选择发送索赔使用自定义规则，，然后单击下一步。
5. 在配置规则页面本名规则索赔，键入的显示名称为此规则。 自定义规则下中，键入或粘贴以下索赔规则语言语法： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 单击完成。 确认新规则立即显示以下允许访问颁发授权规则列表中的所有用户规则。
7. 若要将规则，保存在编辑索赔规则对话框中，单击确定。


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>在上方的情况下使用索赔规则语言语法的说明

|描述|声称规则语言语法|
|-----|-----| 
|默认广告 FS 规则应用于允许所有用户访问。 此规则应该存在中依赖方信任颁发授权规则列表 Microsoft Office 365 身份平台。|= > 问题 (键入 ="https://schemas.microsoft.com/authorization/claims/permit"，值 ="true");| 
|添加到新的自定义规则本节指定请求已来自联合身份验证的服务器代理 （即，具有 x ms 代理标题）
所有规则都包含该建议。|存在 ([类型"https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"= =])| 
|用于建立请求取自 IP 定义接受的覆盖范围内的客户端。|不存在 ([类型"https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"，值 = = = ~"客户提供公共 ip 地址 regex"])| 
|本节用于指定是否正在访问的应用程序不 Microsoft.Exchange.ActiveSync 应该拒绝该请求。|不存在 ([类型"https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"= =，Value=="Microsoft.Exchange.ActiveSync"])| 
|此规则允许你以确定是否通话是通过 Web 浏览器，并不拒绝。|不存在 ([类型"https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path"，值 = = = ="月 adfs 月 1！/"])| 
|此规则规定，应拒绝仅组的用户中特定 Active Directory （基于 SID 值）。 将添加到此声明不表示将允许的一组用户无论位置。|存在 ([类型"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid"，值 = = = ~"{组允许广告组 SID 值}"])| 
|这是必需的条款处理拒绝满足前面的所有条件时。|= > 问题 (键入 ="https://schemas.microsoft.com/authorization/claims/deny"，值 ="true");|

### <a name="building-the-ip-address-range-expression"></a>生成 IP 地址范围 expression

当前仅通过 Exchange Online，它对广告 FS 传递验证请求时填充标题设置 HTTP 标题填充 x ms-转发的客户端-ip 索赔。 声明值可能是以下情况之一：

>[!Note] 
>Exchange Online 当前支持的只 IPV4 和不 IPV6 地址。

一个 IP 地址： 直接连接到联机更换费用的客户端路由器的 IP 地址

>[!Note] 
>公司的网络上的客户端的 IP 地址将显示为组织的站代理或网关的外部界面 IP 地址。

作为企业客户的内部或外部取决于 VPN 或 DA.配置的客户端，可能会通过 VPN，或通过 Microsoft DirectAccess (DA) 连接到公司的网络的客户端

一个或多个 IP 地址： 当 Exchange Online 无法确定连接的客户端的 IP 地址，它将设置基于 x-转发-有关标头值，可以包含在基于 HTTP 请求和支持的许多客户端、 负载平衡和代理服务器市场上的标准标头的值。

>[!Note]
>多个 IP 地址，表明客户 IP 地址和传递请求，每个代理服务器的地址会用逗号分隔。

与相关的 Exchange Online 基础结构的 IP 地址将不会显示在列表中。


#### <a name="regular-expressions"></a>常规表情

当你将需要匹配范围内的 IP 地址时，它有必要构建常规 expression 进行比较。 在下一系列中的步骤，我们将提供有关如何构建此类 expression 匹配下列地址范围 （请注意，你将需要更改这些示例匹配 IP 公共区域） 的示例：


- 192.168.1.1 – 192.168.1.25
- 10.0.0.1 – 10.0.0.14

首先，将匹配一个 IP 地址的基本模式如下所示： \b###\.###\.###\.###\b

将扩展这种情况，我们可以匹配或 expression 使用两种不同的 IP 地址，如下所示： \b###\.###\.###\.###\b|\b###\.###\.###\.###\b

因此，是一个示例要 （例如 192.168.1.1 或 10.0.0.1） 只是两个地址相匹配： \b192\.168\.1\.1\b|\b10\.0\.0\.1\b

这将为你提供的技术，你可以通过它输入任意数量的地址。 其中的各种地址需要允许，例如 192.168.1.1 – 192.168.1.25，匹配必须完成按字符字符： \b192\.168\.1\。([1 9] | 1 [0 到 9] | 2 [0 5]) \b

>[!Note] 
>IP 地址视为字符串并不是一个数字。


此规则分别，如下所示： \b192\.168\.1\。

此匹配 192.168.1 任何值的开始。

以下匹配最终小数点后地址部分所需区域：


- （[1 9] 匹配以地址 1 9
- | 1 [0 到 9] 匹配结束 10 19 中的地址
- |2[0-5]) 以 20 25 匹配地址

>[!Note]
>括号必须正确位于，以便在不启动匹配的 IP 地址的其他部分。

与好友 192 块，我们可以编写 10 块类似 expression: \b10\.0\.0\。([1 9] | 1 [0 4]) \b

搭配制定它们，以下 expression 应该"192.168.1.1~25"和"10.0.0.1~14"所有地址： \b192\.168\.1\。([1-9]|1[0-9]|2[0-5])\b|\b10\.0\.0\。([1 9] | 1 [0 4]) \b

#### <a name="testing-the-expression"></a>测试 Expression

Regex 表情会变得非常棘手，因此我们强烈建议使用 regex 验证工具。 如果您执行搜索"在线 regex expression builder"internet，你将找到将允许你试用示例数据针对你表情的多个好在线工具。

当测试 expression 时，很重要，了解如何希望必须匹配。 Exchange online 系统可能会发送许多 IP 地址，请用逗号分隔。 上面提供的表情将适用于此。 但是，请务必对此功能的看法测试 regex 表情时。 例如，用户可以使用下面的示例输入验证上方的示例： 

192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20 

10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1 











## <a name="validating-the-deployment"></a>验证部署

### <a name="security-audit-logs"></a>安全审核日志

若要确认新的请求上下文索赔将被发送和适用于广告 FS 声称处理管道的启用审核日志广告 FS 服务器上。 然后发送标准安全审核日志条目某些身份验证请求和检查索赔值。 

若要启用的安全事件日志广告 FS 服务器的审核日志记录，请按照配置审核广告 fs 2.0 在步骤。

### <a name="event-logging"></a>事件日志记录

默认情况下，应用程序事件日志位于应用程序和服务日志记录失败的请求 \ 广告 FS 2.0 \ Admin.For 广告 fs 事件日志记录的详细信息看到[设置广告 FS 2.0 事件日志记录](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx)。

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>配置详述广告 FS 跟踪日志

到广告 FS 2.0 调试日志记录广告 FS 跟踪事件。 若要启用跟踪，请参阅[配置为广告 FS 2.0 调试跟踪](https://technet.microsoft.com/en-us/library/adfs2-troubleshooting-configuring-computers.aspx)。

启用跟踪后，使用以下命令行语法启用的详细日志记录级别： wevtutil.exe sl"广告 FS 2.0 跟踪/调试"/l:5  

## <a name="related"></a>相关
新的声明类型的详细信息，请参阅[广告 FS 索赔类型](AD-FS-Claims-Types.md)。
 

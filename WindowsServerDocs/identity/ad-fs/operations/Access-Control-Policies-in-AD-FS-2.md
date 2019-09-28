---
ms.assetid: ''
title: Active Directory 联合身份验证服务2.0 中的客户端访问控制策略
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b0d6133a6fb43b8624dc1329db632fb5dd4aa070
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358451"
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>AD FS 2.0 中的客户端访问控制策略
Active Directory 联合身份验证服务2.0 中的客户端访问策略允许你限制或授予用户对资源的访问权限。  本文档介绍如何在 AD FS 2.0 中启用客户端访问策略以及如何配置最常见的方案。

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>在 AD FS 2.0 中启用客户端访问策略

若要启用客户端访问策略，请执行以下步骤。

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>第 1 步：在 AD FS 服务器上安装 AD FS 2.0 包的更新汇总2

下载[Active Directory 联合身份验证服务（AD FS）2.0 包的更新汇总 2](https://support.microsoft.com/en-us/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0) ，并将其安装在所有联合服务器和联合服务器代理上。

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>步骤 2：将五个声明规则添加到 Active Directory 声明提供方信任

在所有 AD FS 服务器和代理上都安装了更新汇总2后，请使用以下过程添加一组声明规则，这些规则将使新的声明类型对策略引擎可用。

为此，你将使用以下过程为每个新的请求上下文声明类型添加五个接受转换规则。

在 Active Directory 声明提供方信任上，创建一个新的接受转换规则以传递每个新的请求上下文声明类型。

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>若要为五个上下文声明类型中的每一种，将声明规则添加到 Active Directory 声明提供方信任：


1. 单击 "开始"，依次指向 "程序"、"管理工具"，然后单击 "AD FS 2.0 管理"。
2. 在控制台树中的 "AD FS 2.0 \ 信任关系" 下，单击 "声明提供方信任"，右键单击 Active Directory，然后单击 "编辑声明规则"。
3. 在 "编辑声明规则" 对话框中，选择 "接受转换规则" 选项卡，然后单击 "添加规则" 以启动规则向导。
4. 在 "选择规则模板" 页上的 "声明规则模板" 下，选择 "通过" 或 "筛选传入声明"，然后单击 "下一步"。
5. 在 "配置规则" 页上的 "声明规则名称" 下，键入此规则的显示名称;在 "传入声明类型" 中，键入以下声明类型 URL，然后选择 "传递所有声明值"。</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. 若要验证规则，请在列表中选择它，然后单击 "编辑规则"，然后单击 "查看规则语言"。 声明规则语言应如下所示：`c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. 单击 Finish。
8. 在 "编辑声明规则" 对话框中，单击 "确定" 保存规则。
9. 重复步骤2到步骤6，为下面所示的其余四种声明类型创建其他声明规则，直到创建了所有五个规则。

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


~~~
`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`
~~~

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>步骤 3:更新 Microsoft Office 365 标识平台信赖方信任

选择以下示例方案之一，以配置最符合组织需求的 Microsoft Office 365 标识平台信赖方信任上的声明规则。

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>AD FS 2.0 的客户端访问策略方案
以下部分将介绍 AD FS 2.0 的情况

### <a name="scenario-1-block-all-external-access-to-office-365"></a>方案 1：阻止对 Office 365 的所有外部访问

此客户端访问策略方案允许从所有内部客户端进行访问，并基于外部客户端的 IP 地址阻止所有外部客户端。 规则集基于默认的颁发授权规则进行生成，以允许访问所有用户。 你可以使用以下过程向 Office 365 信赖方信任添加颁发授权规则。

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>创建规则以阻止对 Office 365 的所有外部访问



1. 单击 "开始"，依次指向 "程序"、"管理工具"，然后单击 "AD FS 2.0 管理"。
2. 在控制台树中的 "AD FS 2.0 \ 信任关系" 下，单击 "信赖方信任"，右键单击 Microsoft Office 365 "标识平台信任"，然后单击 "编辑声明规则"。 
3. 在 "编辑声明规则" 对话框中，选择 "颁发授权规则" 选项卡，然后单击 "添加规则" 以启动声明规则向导。
4. 在 "选择规则模板" 页上的 "声明规则模板" 下，选择 "使用自定义规则发送声明"，然后单击 "下一步"。
5. 在 "配置规则" 页上的 "声明规则名称" 下，键入此规则的显示名称。 在 "自定义规则" 下，键入或粘贴以下声明规则语言语法：`exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");` 
6. 单击 Finish。 验证 "颁发授权规则" 列表中的 "允许访问所有用户的规则" 下面是否出现了新规则。
7. 若要保存规则，请在 "编辑声明规则" 对话框中，单击 "确定"。

>[!NOTE]
>必须将上述值替换为有效的 IP 表达式的 "公共 ip 地址正则表达式";有关详细信息，请参阅生成 IP 地址范围表达式。


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>方案 2：阻止对 Office 365 的所有外部访问（Exchange ActiveSync 除外）

下面的示例允许从包括 Outlook 在内的内部客户端访问所有的 Office 365 应用程序，包括 Exchange Online。 它阻止从位于企业网络外部的客户端进行访问，如客户端 IP 地址所示，智能手机等 Exchange ActiveSync 客户端除外。 规则集建立在名为 "允许所有用户访问" 的默认颁发授权规则之上。 使用以下步骤，通过声明规则向导将颁发授权规则添加到 Office 365 信赖方信任：

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>创建规则以阻止对 Office 365 的所有外部访问



1. 单击 "开始"，依次指向 "程序"、"管理工具"，然后单击 "AD FS 2.0 管理"。
2. 在控制台树中的 "AD FS 2.0 \ 信任关系" 下，单击 "信赖方信任"，右键单击 Microsoft Office 365 "标识平台信任"，然后单击 "编辑声明规则"。 
3. 在 "编辑声明规则" 对话框中，选择 "颁发授权规则" 选项卡，然后单击 "添加规则" 以启动声明规则向导。
4. 在 "选择规则模板" 页上的 "声明规则模板" 下，选择 "使用自定义规则发送声明"，然后单击 "下一步"。
5. 在 "配置规则" 页上的 "声明规则名称" 下，键入此规则的显示名称。 在 "自定义规则" 下，键入或粘贴以下声明规则语言语法：`exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.Autodiscover"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.ActiveSync"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 单击 Finish。 验证 "颁发授权规则" 列表中的 "允许访问所有用户的规则" 下面是否出现了新规则。
7. 若要保存规则，请在 "编辑声明规则" 对话框中，单击 "确定"。

>[!NOTE]
>必须将上述值替换为有效的 IP 表达式的 "公共 ip 地址正则表达式";有关详细信息，请参阅生成 IP 地址范围表达式。

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>方案 3：阻止对 Office 365 的所有外部访问（基于浏览器的应用程序除外）

规则集建立在名为 "允许所有用户访问" 的默认颁发授权规则之上。 使用以下步骤，通过声明规则向导将颁发授权规则添加到 Microsoft Office 365 标识平台信赖方信任：

>[!NOTE]
>此方案不受第三方代理的支持，因为对客户端访问策略标头具有被动（基于 Web）请求的限制。

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>创建规则以阻止对 Office 365 的所有外部访问（基于浏览器的应用程序除外）



1. 单击 "开始"，依次指向 "程序"、"管理工具"，然后单击 "AD FS 2.0 管理"。
2. 在控制台树中的 "AD FS 2.0 \ 信任关系" 下，单击 "信赖方信任"，右键单击 Microsoft Office 365 "标识平台信任"，然后单击 "编辑声明规则"。 
3. 在 "编辑声明规则" 对话框中，选择 "颁发授权规则" 选项卡，然后单击 "添加规则" 以启动声明规则向导。
4. 在 "选择规则模板" 页上的 "声明规则模板" 下，选择 "使用自定义规则发送声明"，然后单击 "下一步"。
5. 在 "配置规则" 页上的 "声明规则名称" 下，键入此规则的显示名称。 在 "自定义规则" 下，键入或粘贴以下声明规则语言语法：`exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 单击 Finish。 验证 "颁发授权规则" 列表中的 "允许访问所有用户的规则" 下面是否出现了新规则。
7. 若要保存规则，请在 "编辑声明规则" 对话框中，单击 "确定"。

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>方案4：阻止指定 Active Directory 组对 Office 365 的所有外部访问

以下示例启用了基于 IP 地址的内部客户端的访问。 它阻止从公司网络外部的客户端进行访问，这些客户端具有外部客户端 IP 地址，但指定 Active Directory 组中的个人除外。规则集建立在名为 "允许访问" 的默认颁发授权规则之上所有用户。 使用以下步骤，通过声明规则向导将颁发授权规则添加到 Microsoft Office 365 标识平台信赖方信任：

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>创建规则以阻止指定 Active Directory 组对 Office 365 的所有外部访问



1. 单击 "开始"，依次指向 "程序"、"管理工具"，然后单击 "AD FS 2.0 管理"。
2. 在控制台树中的 "AD FS 2.0 \ 信任关系" 下，单击 "信赖方信任"，右键单击 Microsoft Office 365 "标识平台信任"，然后单击 "编辑声明规则"。 
3. 在 "编辑声明规则" 对话框中，选择 "颁发授权规则" 选项卡，然后单击 "添加规则" 以启动声明规则向导。
4. 在 "选择规则模板" 页上的 "声明规则模板" 下，选择 "使用自定义规则发送声明"，然后单击 "下一步"。
5. 在 "配置规则" 页上的 "声明规则名称" 下，键入此规则的显示名称。 在 "自定义规则" 下，键入或粘贴以下声明规则语言语法：`exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 单击 Finish。 验证 "颁发授权规则" 列表中的 "允许访问所有用户的规则" 下面是否出现了新规则。
7. 若要保存规则，请在 "编辑声明规则" 对话框中，单击 "确定"。


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>上述方案中使用的声明规则语言语法的说明

|                                                                                                   描述                                                                                                   |                                                                     声明规则语言语法                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              默认 AD FS 规则，以允许访问所有用户。 此规则应已存在于 Microsoft Office 365 标识平台信赖方信任颁发授权规则列表中。              |                                  = > 问题（类型 = "<https://schemas.microsoft.com/authorization/claims/permit>"，值 = "true"）;                                   |
|                               如果将此子句添加到新的自定义规则，则指定请求来自联合服务器代理（即，它具有 x ms proxy 标头）                                |                                                                                                                                                                    |
|                                                                                 建议所有规则都包含此。                                                                                  |                                    exists （[Type = = "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy>"]）                                    |
|                                                         用于确定请求来自具有定义的可接受范围内的 IP 的客户端。                                                         | 不存在（[Type = = "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip>"，值 = ~ "客户提供的公共 ip 地址正则表达式"]） |
|                                    此子句用于指定如果访问的应用程序不是 Microsoft，则应拒绝该请求。                                     |       不存在（[Type = = "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application>"，Value = = "        |
|                                                      此规则允许你确定调用是否是通过 Web 浏览器进行的，并且不会被拒绝。                                                      |              不存在（[Type = = "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path>"，Value = = "/adfs/ls/"]）               |
| 此规则指出特定 Active Directory 组中的唯一用户（基于 SID 值）应被拒绝。 如果将 NOT 添加到此语句，则表示将允许一组用户，而不考虑位置。 |             exists （[Type = = "<https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid>"，值 = ~ "{组 SID 允许的 AD 组的值}"]）              |
|                                                                这是在满足前面所有条件时发出拒绝的必需子句。                                                                 |                                   = > 问题（类型 = "<https://schemas.microsoft.com/authorization/claims/deny>"，值 = "true"）;                                    |

### <a name="building-the-ip-address-range-expression"></a>构建 IP 地址范围表达式

从当前由 Exchange Online 设置的 HTTP 标头中填充了 x ms 转发的客户端 ip 声明，这会在将身份验证请求传递到 AD FS 时填充标头。 声明的值可以是下列值之一：

>[!Note] 
>Exchange Online 当前只支持 IPV4，而不支持 IPV6 地址。

单个 IP 地址：直接连接到 Exchange Online 的客户端的 IP 地址

>[!Note] 
>企业网络上的客户端的 IP 地址将显示为组织的出站代理或网关的外部接口 IP 地址。

通过 VPN 或 Microsoft DirectAccess （DA）连接到公司网络的客户端可以显示为内部企业客户端，也可以作为外部客户端出现，具体取决于 VPN 或 DA 的配置。

一个或多个 IP 地址：当 Exchange Online 无法确定正在连接的客户端的 IP 地址时，它将基于 x 转发的标头的值设置值，这是基于 HTTP 的请求中可包含的一个非标准标头，许多客户端和负载均衡器都支持该标头。以及市场上的代理。

>[!Note]
>多个 IP 地址（表示通过请求的每个代理的客户端 IP 地址和地址）将用逗号分隔。

与 Exchange Online 基础结构相关的 IP 地址将不会出现在列表中。


#### <a name="regular-expressions"></a>正则表达式

当必须与某个范围的 IP 地址匹配时，需要构造一个正则表达式来执行比较。 在接下来的一系列步骤中，我们将为如何构造此类表达式来匹配以下地址范围提供示例（请注意，必须更改这些示例以匹配公共 IP 范围）：


- 192.168.1.1 –192.168.1.25
- 10.0.0.1 –10.0.0.14

首先，将匹配单个 IP 地址的基本模式如下： \b #\.# ####\.###\.# # # \b

扩展此内容，我们可以使用或表达式匹配两个不同的 IP 地址，如下所示：\.\b # # #\. \.\. ######\.# # # \b | \b # ##### ######\b \.

因此，只匹配两个地址（如192.168.1.1 或10.0.0.1）的示例为： \b192\.168\.1\.1 \ b | \b10\.0\.0\.1 \ b

这为你提供了可用于输入任意数量的地址的方法。 如果需要允许的地址范围（例如192.168.1.1 –192.168.1.25），则匹配必须是 by 字符： \b192 @ no__t-0168 @ no__t-11 @ no__t-2 （[1-9] | 1 [0-9] | 2 [0-5]） \b

>[!Note] 
>IP 地址被视为字符串而不是数字。


规则按如下方式分解： \b192\.168 1\.\.

这与任何以192.168.1 开头的值匹配。

以下内容与最后一个小数点后面的地址部分所需的范围匹配：


- （[1-9] 匹配以1-9 结尾的地址
- | 1 [0-9] 匹配以10-19 结尾的地址
- | 2 [0-5]）匹配以20-25 结尾的地址

>[!Note]
>必须正确定位括号，以便不会开始匹配 IP 地址的其他部分。

使用匹配的192块，我们可以为10块编写类似的表达式： \b10 @ no__t-00 @ no__t-10 @ no__t （[1-9] | 1 [0-4]） \b

将其放在一起，以下表达式应匹配 "192.168.1.1 ~ 25" 和 "10.0.0.1 ~ 14" 的所有地址： \b192 @ no__t-0168 @ no__t-11 @ no__t-2 （[1-9] | 1 [0-9] | 2 [0-5]） \b | \b10 @ no__t-30 @ no__t-40 @ no__t-5 （[1-9] | 1 [0-4]） \b

#### <a name="testing-the-expression"></a>测试表达式

正则表达式表达式可能会变得相当复杂，因此强烈建议使用 regex 验证工具。 如果您在 internet 上搜索 "联机 regex 表达式生成器"，您将看到多个有效的联机实用程序，您可以使用这些实用程序来尝试使用示例数据。

测试表达式时，必须了解需要匹配的内容，这一点非常重要。 Exchange online 系统可能会发送多个 IP 地址，用逗号分隔。 上面提供的表达式适用于这种情况。 但是，在测试正则表达式时要考虑这一点。 例如，可以使用以下示例输入来验证上述示例： 

192.168.1.1、192.168.1.2、192.169.1.1。 192.168.12.1、192.168.1.10、192.168.1.25、192.168.1.26、192.168.1.30、1192.168.1.20 

10.0.0.1、10.0.0.5、10.0.0.10、10.0.1.0、10.0.1.1、110.0.0.1、10.0.0.14、10.0.0.15、10.0.0.10、10、0.0。1 











## <a name="validating-the-deployment"></a>验证部署

### <a name="security-audit-logs"></a>安全审核日志

若要验证是否正在发送新请求上下文声明并将其提供给 AD FS 声明处理管道，请在 AD FS 服务器上启用审核日志记录。 然后发送一些身份验证请求，并检查标准安全审核日志条目中的声明值。 

若要启用对 AD FS 服务器上的安全日志的审核事件日志记录，请按照配置 AD FS 2.0 的审核中的步骤进行操作。

### <a name="event-logging"></a>事件日志记录

默认情况下，失败的请求将记录到位于 "应用程序和服务日志 \ AD FS 2.0 \ 管理员" 下的应用程序事件日志中。有关 AD FS 的事件日志记录的详细信息，请参阅[设置 AD FS 2.0 事件日志记录](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx)。

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>配置详细 AD FS 跟踪日志

AD FS 跟踪事件记录到 AD FS 2.0 调试日志中。 若要启用跟踪，请参阅[为 AD FS 2.0 配置调试跟踪](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx)。

启用跟踪后，请使用以下命令行语法启用详细日志记录级别： wevtutil sl "AD FS 2.0 跟踪/调试"/l：5  

## <a name="related"></a>相关内容
有关新声明类型的详细信息，请参阅[AD FS 声明类型](AD-FS-Claims-Types.md)。


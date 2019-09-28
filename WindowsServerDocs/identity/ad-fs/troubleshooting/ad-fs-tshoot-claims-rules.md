---
title: AD FS 故障排除-声明规则
description: 本文档介绍了如何排查声明规则语法与 AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d0146ba7cfc736f4d37ca66d58d624cc2f7f9a23
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366278"
---
# <a name="ad-fs-troubleshooting---claims-rules-syntax"></a>AD FS 疑难解答-声明规则语法
声明是一个使用者对其自身或另一个使用者做出的陈述。  声明由信赖方颁发，它们被赋予一个或多个值，然后打包到 AD FS 服务器颁发的安全令牌。  本文介绍声明语法和创建。  有关声明颁发的信息，请参阅[AD FS 故障排除-声明颁发](ad-fs-tshoot-claims-issuance.md)。

>[!NOTE]  
>你可以使用[ADFS 帮助](https://adfshelp.microsoft.com)站点上的[ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest)来帮助排查声明问题。   

## <a name="how-claim-rules-are-processed"></a>处理声明规则的方式
声明规则是通过声明[管道](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md)使用[声明引擎](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)处理的。 声明引擎是联合身份验证服务的一个逻辑组件，它检查用户提供的传入声明集，然后根据每个规则中的逻辑生成输出声明集。

## <a name="how-to-create-a-claim-rule"></a>如何创建声明规则
声明规则对于联合身份验证服务中的每个联合信任关系单独进行创建，不在多个信任间共享。 你可以从[声明规则模板](../../ad-fs/technical-reference/determine-the-type-of-claim-rule-template-to-use.md)创建规则、通过使用[声明规则语言](../../ad-fs/technical-reference/when-to-use-a-custom-claim-rule.md)创建规则来从头开始，或使用 Windows PowerShell 自定义规则。

## <a name="understanding-the-components-of-the-claim-rule-language"></a>了解声明规则语言的组件
声明规则语言包含以下组件，这些组件以 "= >" 运算符分隔：

- 一个条件-用于检查输入声明并确定是否应执行规则的发出语句。  它表示一个逻辑表达式，该表达式的计算结果必须为 true，才能执行规则正文部分。

- 发出语句

例如：

```c:[type == "Name", value == "domain user"] => issue(type = "Role", value = "employee");``` 

以下声明具有以下内容：
- 条件-`c:[type == "Name", value == "domain user"] `-对于 windows 帐户名是否为域用户，对输入声明进行评估
- 颁发-`issue(type = "Role", value = "employee")`-如果条件为 true，则向输入声明添加具有 employee 角色的新声明。

有关声明和语法的详细信息，请参阅[声明规则语言的角色](../../ad-fs/technical-reference/the-role-of-the-claim-rule-language.md)。

## <a name="claims-rule-editor"></a>声明规则编辑器
完成声明后，声明规则编辑器会执行语法检查，并单击 **"确定"** 。  如果你使用的语法不正确，则编辑器会通知你。

![claims](media/ad-fs-tshoot-claims/claims1.png)

## <a name="event-logs"></a>事件日志
尝试使用日志对声明进行故障排除时，最好的方法是查找声明输出。  可以在事件日志中查找1000和1001事件。

![claims](media/ad-fs-tshoot-claims/claims2.png)

## <a name="creating-a-sample-application"></a>创建示例应用程序
你还可以创建一个示例应用程序，用于回显你的声明。  例如，你可以使用示例应用程序，并创建具有你要尝试排除的相同声明的信赖方，并查看该应用是否存在与该声明有关的任何问题。

![claims](media/ad-fs-tshoot-claims/claim4.png)

此处提供了一个很好的示例 web 应用程序。  此应用程序是一个简单的 web 应用，用于回显从信赖方收到的声明。  若要使用此方法，需要通过以下方法编辑 web.config 应用：
- 将 https://app1.contoso.com/sampapp 更改为用于托管 sampapp 的 URL
- 更改 sts.contoso.com 的所有实例，使之指向 AD FS 联合服务器
- 将指纹替换为指纹

![claims](media/ad-fs-tshoot-claims/claims3.png)

以下[博客文章](https://blogs.technet.microsoft.com/tangent_thoughts/2015/02/20/install-and-configure-a-simple-net-4-5-sample-federated-application-samapp/)详细说明了如何进行设置。

## <a name="next-steps"></a>后续步骤

- [AD FS 疑难解答](ad-fs-tshoot-overview.md)
---
title: AD FS 故障排除-声明规则
description: 本文档介绍如何解决与 AD FS 声明规则语法
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 027b2afc9e580253ec820e7e5be14419387ddd44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837918"
---
# <a name="ad-fs-troubleshooting---claims-rules-syntax"></a>AD FS 故障排除-声明规则语法
声明是一个语句，一个使用者使其自身或另一个主题。  在信赖方颁发的声明，它们是给定的一个或多个值，然后由 AD FS 服务器颁发的安全令牌中打包。  本文介绍如何处理声明语法和创建。  有关声明颁发请参阅[AD FS 故障排除-声明颁发](ad-fs-tshoot-claims-issuance.md)。

>[!NOTE]  
>可以使用[ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest)上[ADFS 帮助](https://adfshelp.microsoft.com)站点以帮助解决声明的问题。   

## <a name="how-claim-rules-are-processed"></a>处理声明规则的方式
通过处理声明规则[声明管道](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md)使用[声明引擎](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)。 声明引擎是联合身份验证服务的一个逻辑组件，它检查用户提供的传入声明集，然后根据每个规则中的逻辑生成输出声明集。

## <a name="how-to-create-a-claim-rule"></a>如何创建声明规则
声明规则对于联合身份验证服务中的每个联合信任关系单独进行创建，不在多个信任间共享。 您可以创建的规则[声明规则模板](../../ad-fs/technical-reference/determine-the-type-of-claim-rule-template-to-use.md)，通过创作规则使用从头开始[声明规则语言](../../ad-fs/technical-reference/when-to-use-a-custom-claim-rule.md)或使用 Windows PowerShell 自定义规则。

## <a name="understanding-the-components-of-the-claim-rule-language"></a>了解声明规则语言的组件
声明规则语言包括以下组件分隔"= >"运算符：

- 条件-用于检查输入的声明并确定是否应执行规则的发出语句。  表示必须对计算的逻辑表达式为 true 才能执行规则的正文部分。

- 发出语句

例如：

```c:[type == "Name", value == "domain user"] => issue(type = "Role", value = "employee");``` 

以下声明具有以下：
- 条件-`c:[type == "Name", value == "domain user"] `的计算结果的 windows 帐户名称是否是域用户的输入的声明
- 颁发- `issue(type = "Role", value = "employee")` -如果条件为 true，将添加到输入声明与员工的角色的新声明。

声明和语法的详细信息请参阅[的声明规则语言角色](../../ad-fs/technical-reference/the-role-of-the-claim-rule-language.md)。

## <a name="claims-rule-editor"></a>声明规则编辑器
语法检查由声明规则编辑器后完成该声明，并单击**确定**。  因此如果具有不正确的语法然后编辑器将通知你。

![声明](media/ad-fs-tshoot-claims/claims1.png)

## <a name="event-logs"></a>事件日志
查找声明使用日志进行故障排除时的最佳方法是查找声明输出。  您可以查找事件日志中的 1000年和 1001年事件。

![声明](media/ad-fs-tshoot-claims/claims2.png)

## <a name="creating-a-sample-application"></a>创建示例应用程序
您还可以创建一个示例应用程序回显您的声明。  例如，您可以使用示例应用程序，创建信赖方具有相同的声明要进行故障排除和查看应用程序是否具有声明的任何问题。

![声明](media/ad-fs-tshoot-claims/claim4.png)

此处提供了一个很好的示例 web 应用。  此应用是回显收到信赖方的声明一个简单的 web 应用。  若要使用这需要编辑通过 web.config 应用：
- 更改 https://app1.contoso.com/sampapp到的 URL 将用于托管 sampapp
- 更改 sts.contoso.com 指向 AD FS 联合身份验证服务器的所有实例
- 指纹替换为你的指纹

![声明](media/ad-fs-tshoot-claims/claims3.png)

以下[博客文章](https://blogs.technet.microsoft.com/tangent_thoughts/2015/02/20/install-and-configure-a-simple-net-4-5-sample-federated-application-samapp/)具有极好、 更深入说明，此设置。

## <a name="next-steps"></a>后续步骤

- [AD FS 进行故障排除](ad-fs-tshoot-overview.md)
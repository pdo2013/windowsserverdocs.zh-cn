---
ms.assetid: 102eeeb1-6c55-42a2-b321-71a7dab46146
title: AD FS 中的访问控制策略
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c690f81620f97622a2f068b07c36e0a6c59e90d4
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190338"
---
# <a name="access-control-policies-in-windows-server-2016-ad-fs"></a>Windows Server 2016 AD FS 中的访问控制策略

  
## <a name="access-control-policy-templates-in-ad-fs"></a>在 AD FS 访问控制策略模板  
Active Directory 联合身份验证服务现在支持访问控制策略模板的使用。  通过使用访问控制策略模板，管理员可以通过分配给信赖方 (Rp) 的组策略模板实施策略设置。 管理员还可以对策略模板进行更新和所做的更改将应用于信赖方自动如果无需用户交互。  
  
## <a name="what-are-access-control-policy-templates"></a>访问控制策略模板是什么？  
策略处理 AD FS core 管道具有以下三个阶段： 身份验证、 授权和声明颁发。 当前，AD FS 管理员必须单独为每个阶段中配置策略。  这还涉及了解这些策略的影响，并且这些策略可以间的依赖关系。 此外，管理员必须了解声明规则语言和作者自定义规则，以便启用 （例如一些简单的 / 常用策略。 阻止外部访问）。  
  
管理员需要配置颁发授权规则使用此旧模型模板执行何种访问控制策略是替换声明语言。  颁发授权规则的旧 PowerShell cmdlet 仍适用，但它是互斥的新模型。 管理员可以选择任何一个以使用新模型或旧模型。  新模型，管理员可以控制何时授予访问权限，包括强制实施多重身份验证。  
  
访问控制策略模板使用允许模型。  这意味着默认情况下，没有人具有访问权限以及必须显式授予访问权限。  但是，这不是只是全有或允许执行任何操作。  管理员可以将异常添加到允许规则。  例如，管理员可能想要授予访问权限基于特定网络上通过选择此选项并指定 IP 地址范围。  但管理员可以添加和异常，例如，管理员可能会添加一个例外，从特定网络并指定该 IP 地址范围。  
  
![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP2.PNG)  
  
## <a name="built-in-access-control-policy-templates-vs-custom-access-control-policy-templates"></a>内置的访问控制策略模板与自定义访问控制策略模板  
AD FS 包含几个内置的访问控制策略模板。  以下目标具有一组相同的策略要求，例如适用于 Office 365 的客户端访问策略的一些常见方案。  不能修改这些模板。  
  
![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP3.PNG)  
  
若要提供更高的灵活性来满足业务需求，管理员可以创建自己的访问权限策略模板。  这些可以在创建之后修改和自定义策略模板所作更改将应用于所有受这些策略模板的 RPs。  若要添加的自定义策略模板将只需在 AD FS 管理中单击从添加访问控制策略。  
  
若要创建一个策略模板，管理员需要首先指定哪些条件下将令牌颁发和/或委派授权请求。 下表中显示条件和操作的选项。   使用不同的或新值管理员可以进一步配置以粗体显示的条件。 如果有，管理员还可以指定的异常。 当满足条件时，如果没有指定的异常不会触发允许操作和传入的请求与异常中指定的条件相匹配。  
  
|允许用户|除外| 
| --- | --- | 
 |从**特定**网络|从**特定**网络<br /><br />从**特定**组<br /><br />从具有的设备**特定**信任级别<br /><br />与**特定**请求中的声明|  
|从**特定**组|从**特定**网络<br /><br />从**特定**组<br /><br />从具有的设备**特定**信任级别<br /><br />与**特定**请求中的声明|  
|从具有的设备**特定**信任级别|从**特定**网络<br /><br />从**特定**组<br /><br />从具有的设备**特定**信任级别<br /><br />与**特定**请求中的声明|  
|与**特定**请求中的声明|从**特定**网络<br /><br />从**特定**组<br /><br />从具有的设备**特定**信任级别<br /><br />与**特定**请求中的声明|  
|需要多重身份验证和|从**特定**网络<br /><br />从**特定**组<br /><br />从具有的设备**特定**信任级别<br /><br />与**特定**请求中的声明|  
  
如果管理员选择多个条件，属于**AND**关系。 操作是互相排斥，和一个策略规则，你仅可选择一个操作。 如果管理员选择多个异常，它们属于**或**关系。 几个策略规则示例如下所示：  
  
|**策略**|**策略规则**|
| --- | --- |  
|Extranet 访问需要进行 MFA<br /><br />允许所有用户|**规则 #1**<br /><br />从**extranet**<br /><br />和使用 MFA<br /><br />许可<br /><br />**Rule#2**<br /><br />从**intranet**<br /><br />许可|  
|外部访问权限不允许使用除非 FTE<br /><br />允许在已加入工作区的设备上 FTE 的 intranet 访问|**规则 #1**<br /><br />从**extranet**<br /><br />来回**非 FTE**组<br /><br />许可<br /><br />**规则 #2**<br /><br />从**intranet**<br /><br />来回**加入工作区**设备<br /><br />来回**FTE**组<br /><br />许可|  
|除"服务管理员"extranet 访问需要进行 MFA<br /><br />允许所有用户访问|**规则 #1**<br /><br />从**extranet**<br /><br />和使用 MFA<br /><br />许可<br /><br />除**服务管理员组**<br /><br />**规则 #2**<br /><br />始终<br /><br />许可|  
|非工作区加入的设备从 extranet 访问需要进行 MFA<br /><br />允许 intranet 和 extranet 访问 AD 结构|**规则 #1**<br /><br />从**intranet**<br /><br />来回**AD Fabric**组<br /><br />许可<br /><br />**规则 #2**<br /><br />从**extranet**<br /><br />来回**非工作区加入**设备<br /><br />来回**AD Fabric**组<br /><br />和使用 MFA<br /><br />许可<br /><br />**规则 3**<br /><br />从**extranet**<br /><br />来回**加入工作区**设备<br /><br />来回**AD Fabric**组<br /><br />许可|  
  
## <a name="parameterized-policy-template-vs-non-parameterized-policy-template"></a>参数化的策略模板与非参数化策略模板  
访问控制策略可以是  
  
参数化的策略模板是具有参数的策略模板。 管理员需要将此模板分配给 RPs.An 管理员时这些参数的值不能对模板进行更改参数化的策略创建它后输入。  参数化策略的一个示例是内置策略，允许特定组。  此策略应用于 RP，每当需要指定此参数。  
  
![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP5.PNG)  
  
非参数化策略模板是不带参数的策略模板。 管理员可以将此模板 rps 分配不需要任何输入的情况下，可以创建它后对非参数化策略模板进行更改。  此示例是内置的策略，授权所有人并要求进行 MFA。  
  
![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP4.PNG)  
  
## <a name="how-to-create-a-non-parameterized-access-control-policy"></a>如何创建非参数化的访问控制策略  
若要创建非参数化的访问控制策略，请使用以下过程  
  
#### <a name="to-create-a-non-parameterized-access-control-policy"></a>若要创建非参数化的访问控制策略  
  
1.  从 AD FS 管理，在左侧选择访问控制策略，在右侧单击添加访问控制策略。  
  
2.  输入一个名称和描述。  例如：允许用户与经过身份验证的设备。  
  
3.  下**允许访问，如果满足以下规则之一**，单击**添加**。  
  
4.  下允许，检查在框旁边放置**从特定的信任级别的设备**  
  
5.  在底部，选择在有下划线**特定**  
  
6.  从该弹出窗口中，选择**进行身份验证**从下拉列表。  单击“确定”  。  
  
    ![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP6.PNG)  
  
7.  单击“确定”  。 单击“确定”  。  
  
    ![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP7.PNG)  
  
## <a name="how-to-create-a-parameterized-access-control-policy"></a>如何创建参数化的访问控制策略  
若要创建参数化的访问控制策略，请使用以下过程  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>若要创建参数化的访问控制策略  
  
1.  从 AD FS 管理，在左侧选择访问控制策略，在右侧单击添加访问控制策略。  
  
2.  输入一个名称和描述。  例如：允许用户使用特定的声明。  
  
3.  下**允许访问，如果满足以下规则之一**，单击**添加**。  
  
4.  下允许，检查在框旁边放置**与请求中的特定声明**  
  
5.  在底部，选择在有下划线**特定**  
  
6.  从该弹出窗口中，选择**参数指定的访问控制策略分配时**。  单击“确定”  。  
  
    ![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP8.PNG)  
  
7.  单击“确定”  。 单击“确定”  。  
  
    ![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP9.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-an-exception"></a>如何创建自定义访问控制策略，并出现异常  
若要创建访问控制策略，并出现异常，请使用以下过程。  
  
#### <a name="to-create-a-custom-access-control-policy-with-an-exception"></a>若要创建自定义访问控制策略，并出现异常  
  
1.  从 AD FS 管理，在左侧选择访问控制策略，在右侧单击添加访问控制策略。  
  
2.  输入一个名称和描述。  例如：允许用户与身份验证的设备，但不是管理。  
  
3.  下**允许访问，如果满足以下规则之一**，单击**添加**。  
  
4.  下允许，检查在框旁边放置**从特定的信任级别的设备**  
  
5.  在底部，选择在有下划线**特定**  
  
6.  从该弹出窗口中，选择**进行身份验证**从下拉列表。  单击“确定”  。  
  
7.  下除外，在框旁边放置一个检查**从特定的信任级别的设备**  
  
8.  在底部下除外，选择在有下划线**特定**  
  
9. 从该弹出窗口中，选择**托管**从下拉列表。  单击“确定”  。  
  
10. 单击“确定”  。 单击“确定”  。  
  
    ![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP10.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-multiple-permit-conditions"></a>如何创建自定义访问控制策略包含多个允许条件  
若要创建具有多个允许的访问控制策略条件使用以下过程  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>若要创建参数化的访问控制策略  
  
1.  从 AD FS 管理，在左侧选择访问控制策略，在右侧单击添加访问控制策略。  
  
2.  输入一个名称和描述。  例如：允许用户与特定声明，并从特定组。  
  
3.  下**允许访问，如果满足以下规则之一**，单击**添加**。  
  
4.  下允许，检查在框旁边放置**从特定的组**和**与请求中的特定声明**  
  
5.  在底部，选择在有下划线**特定**组旁边的第一个条件  
  
6.  从该弹出窗口中，选择**参数指定当分配了策略**。  单击“确定”  。  
  
7.  在底部，选择在有下划线**特定**对于第二个条件，旁边声明  
  
8.  从该弹出窗口中，选择**参数指定的访问控制策略分配时**。  单击“确定”  。  
  
9. 单击“确定”  。 单击“确定”  。  
  
![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP12.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-a-new-application"></a>如何将访问控制策略分配到新的应用程序  
将访问控制策略分配到新的应用程序是非常简单，现在已集成到该向导添加 RP。  从信赖方信任向导可以选择您想要分配的访问控制策略。  创建新的信赖方信任时，这是一项要求。  
  
![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP13.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-an-existing-application"></a>如何将访问控制策略分配到现有应用程序  
分配的访问控制策略到现有应用程序只需选择该应用程序从信赖方信任和上右键单击**编辑访问控制策略**。  
  
![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP14.PNG)  
  
从此处你可以选择的访问控制策略，并将其应用于应用程序。  
  
![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP15.PNG)  
  
## <a name="see-also"></a>请参阅  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 


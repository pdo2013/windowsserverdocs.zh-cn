---
ms.assetid: c0d64566-5530-482e-a332-af029a5fb575
title: 将设计要求映射到林设计模型
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d65b03dc255de5523c48c2bb9359530b8e7c3167
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408763"
---
# <a name="mapping-design-requirements-to-forest-design-models"></a>将设计要求映射到林设计模型

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

你的组织中的大多数组可以共享由单一信息技术（IT）组管理的单个组织林，其中包含共享该林的所有组的用户帐户和资源。 此共享林称为初始组织林，是组织的林设计模型的基础。  

由于初始组织林可以在组织中托管多个组，因此，林所有者必须与每个组建立服务级别协议，以便所有参与方都能理解它们的预期。 这将通过建立议定的服务期望来保护各个组和林所有者。  

如果组织中的所有组都不能共享一个组织林，则必须扩展林的设计，以满足不同组的需求。 这涉及到基于其独立性和隔离需要应用于组的设计要求，以及它们是否具有受限连接网络，然后确定可用于满足这些需求的林模型要求. 下表列出了基于独立性、隔离和连接因素的林设计模型方案。 确定最符合要求的林设计方案后，确定是否需要作出任何其他决策来满足您的设计规范。  

> [!NOTE]  
> 如果某个因素列为 "N/A"，则不会考虑这一点，因为其他要求也适用于该因素。  

|应用场景|受限连接|数据隔离|数据独立性|服务隔离|服务自治|  
|------------|------------------------|------------------|-----------------|---------------------|--------------------|  
|[方案 1:加入现有林以进行数据独立性 @ no__t-0|否|否|是|否|否|  
|[方案 2:使用组织林或域进行服务独立性 @ no__t-0|否|否|不可用|否|是|  
|[Scenario 3：使用组织林或资源林进行服务隔离 @ no__t-0|否|否|不可用|是|不可用|  
|[Scenario 4：使用组织林或受限访问林进行数据隔离 @ no__t-0|不可用|是|不可用|不可用|不可用|  
|[Scenario 5：使用组织林，或重新配置防火墙以进行受限连接 @ no__t-0|是|否|不可用|否|否|  
|[Scenario 6：使用组织林或域，并使用有限连接性为服务自治重新配置防火墙 no__t-0|是|否|不可用|否|是|  
|[Scenario 7：使用资源林，并使用有限连接性为服务隔离重新配置防火墙 no__t-0|是|否|不可用|是|不可用|  

## <a name="BKMK_1"></a>场景 1：加入现有林以进行数据自治  

只需在现有组织林中的组织单位（Ou）中托管组即可满足数据自治的要求。 将 Ou 控制委派给该组中的数据管理员，以实现数据独立性。 有关使用 Ou 委托控件的详细信息，请参阅[创建组织单位设计](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)。  
  
## <a name="BKMK_2"></a>场景 2：使用组织林或域进行服务自治  

如果组织中的某个组将服务自治标识为要求，我们建议您首先重新考虑此要求。 实现服务自治会为组织带来更多的管理开销和额外成本。 确保对服务独立性的要求并不只是为了方便起见，您可以调整满足此要求所涉及的成本。  
  
可以通过执行下列操作之一满足服务自治的要求：  

- 创建组织林。 将需要服务自治的组的用户、组和计算机放到单独的组织林中。 将该组中的个人分配为林所有者。 如果组需要与组织中的其他林访问或共享资源，则他们可以在组织林和其他林之间建立信任关系。  

- 使用组织域。 将用户、组和计算机放在现有组织林中的一个单独的域中。 此模型仅提供域级服务自治，不适用于完全服务独立性、服务隔离或数据隔离。  

有关使用组织域的详细信息，请参阅[使用组织域林模型](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md)。  

## <a name="BKMK_3"></a>场景 3：使用组织林或资源林进行服务隔离  

通过执行以下操作之一，可以满足服务隔离的要求：  

- 使用组织林。 将需要服务隔离的组的用户、组和计算机放到单独的组织林中。 将该组中的个人分配为林所有者。 如果组需要与组织中的其他林访问或共享资源，则他们可以在组织林和其他林之间建立信任关系。 但是，我们不建议使用此方法，因为在林信任方案中，通过通用组进行的资源访问会受到严重限制。  

- 使用资源林。 将资源和服务帐户置于单独的资源林中，并将用户帐户保留在现有的组织林中。 必要时，可以在资源林中创建备用帐户以访问资源林中的资源（如果组织林不可用）。 备用帐户必须具有登录到资源林所需的权限，并保持对资源的控制，直到组织林恢复联机。  

   在资源与组织林之间建立信任关系，以便用户可以在使用其常规用户帐户时访问林中的资源。 此配置启用用户帐户的集中管理，同时允许用户在组织林不可用时回退到资源林中的备用帐户。  

服务隔离的注意事项包括：

- 为服务隔离创建的林可以信任来自其他林的域，但不得将其他林中的用户包括在其服务管理员组中。 如果其他林中的用户包括在隔离林中的管理组中，则可能会危及独立林的安全，因为林中的服务管理员没有独占控制。  

- 只要域控制器在网络上是可访问的，它们就会受到来自该网络的恶意软件的攻击（例如拒绝服务攻击）。 你可以执行以下操作来防范攻击的可能性：  

   - 仅在被视为安全的网络上托管域控制器。  

   - 限制对承载域控制器的网络或网络的访问。  

- 服务隔离需要创建其他林。 评估维护基础结构以支持其他林的成本是否超过了由于组织林不可用而导致资源访问丢失相关的成本。  

## <a name="BKMK_4"></a>方案4：使用组织林或受限访问林进行数据隔离  

可以通过执行下列操作之一来实现数据隔离：  

- 使用组织林。 将需要数据隔离的组的用户、组和计算机放入单独的组织林中。 将该组中的个人分配为林所有者。 如果组需要与组织中的其他林访问或共享资源，请在组织林与其他林之间建立信任关系。 只有需要访问保密信息的用户才会存在于新的组织林中。 用户使用一个帐户来访问其自己的林中的两个分类数据，并通过信任关系方式访问其他林中的未分类数据。  

- 使用受限访问林。 这是一个单独的林，其中包含受限制的数据以及用于访问该数据的用户帐户。 在用于访问网络上无限制资源的现有组织林中保留单独的用户帐户。 不会在受限访问林与企业中的其他林之间创建信任关系。 您可以通过在单独的物理网络上部署林来进一步限制林，使其无法连接到其他林。 如果在单独的网络上部署林，则用户必须具有两个工作站：一个用于访问受限制的林，另一个用于访问网络的 nonrestricted 区域。  

为数据隔离创建林的注意事项包括：  

- 为数据隔离创建的组织林可以信任来自其他林的域，但其他林的用户不得包含在以下任何内容中：  

  - 负责管理服务管理员组成员身份的服务管理或组的组  

  - 对存储受保护数据的计算机具有管理控制的组  

  - 有权访问受保护数据或组的组，这些数据或组负责管理有权访问受保护数据的用户对象或组对象  

    如果来自其他林的用户包括在这些组中的任何一个组中，则其他林的危害可能会导致隔离林泄露，并泄露受保护的数据。  

- 其他林可以配置为信任为数据隔离创建的组织林，以便隔离林中的用户可以访问其他林中的资源。 但是，隔离林中的用户决不能以交互方式登录到信任林中的工作站。 信任林中的计算机可能会被恶意软件破坏，并可用于捕获用户的登录凭据。  

   > [!NOTE]
   > 若要防止信任林中的服务器模拟独立林的用户，然后访问独立林中的资源，林所有者可以禁用委托身份验证或使用约束委托功能。 有关委派的身份验证和约束委派的详细信息，请参阅[委派身份验证](https://go.microsoft.com/fwlink/?LinkId=106614)。  

- 你可能需要在组织林和组织中的其他林之间建立防火墙，以限制用户对其林以外的信息的访问。  

- 尽管单独创建林会启用数据隔离，但只要隔离林中的域控制器和托管受保护信息的计算机在网络上都是可访问的，它们就会受到从该网络上的计算机发起的攻击。 确定攻击风险太高或者攻击或安全违规后果太大的组织需要限制对网络或托管域控制器和托管受保护数据的计算机的网络的访问. 可以通过使用防火墙和 Internet 协议安全（IPsec）等技术来限制访问。 在极端情况下，组织可能会选择在独立网络上维护受保护的数据，该网络不与组织中的任何其他网络建立物理连接。  

   > [!NOTE]  
   > 如果受限制的访问林与其他网络之间存在任何网络连接，则在将受限区域中的数据传输到其他网络时，可能会出现这种情况。  

## <a name="BKMK_5"></a>方案5：使用组织林或为受限连接重新配置防火墙  

若要满足有限的连接要求，可以执行以下操作之一：  

- 将用户放入现有组织林中，然后打开防火墙，以允许 Active Directory 流量通过。  

- 使用组织林。 将连接受限的组的用户、组和计算机放入单独的组织林中。 将该组中的个人分配为林所有者。 组织林在防火墙的另一端提供单独的环境。 林包括在林中管理的用户帐户和资源，以便用户无需通过防火墙即可完成日常任务。 特定的用户或应用程序可能有需要通过防火墙来联系其他林的特殊需求。 您可以通过在防火墙中打开相应的接口（包括任何信任功能所必需的接口），单独满足这些需求。  

有关配置用于 Active Directory 域服务（AD DS）的防火墙的详细信息，请参阅[按防火墙分段的网络中的 Active Directory](https://go.microsoft.com/fwlink/?LinkId=37928)。  

## <a name="BKMK_6"></a>方案6：使用组织林或域，并使用有限连接性为服务自治重新配置防火墙  

如果组织中的某个组将服务自治标识为要求，我们建议您首先重新考虑此要求。 实现服务自治会为组织带来更多的管理开销和额外成本。 确保对服务独立性的要求并不只是为了方便起见，您可以调整满足此要求所涉及的成本。  

如果连接受到限制，并且你需要服务自治，则可以执行以下操作之一：  

- 使用组织林。 将需要服务自治的组的用户、组和计算机放到单独的组织林中。 将该组中的个人分配为林所有者。 组织林在防火墙的另一端提供单独的环境。 林包括在林中管理的用户帐户和资源，以便用户无需通过防火墙即可完成日常任务。 特定的用户或应用程序可能有需要通过防火墙来联系其他林的特殊需求。 您可以通过在防火墙中打开相应的接口（包括任何信任功能所必需的接口），单独满足这些需求。  

- 将用户、组和计算机放在现有组织林中的一个单独的域中。 此模型仅提供域级服务自治，不适用于完全服务独立性、服务隔离或数据隔离。 林中的其他组必须信任新域的服务管理员，使其与林所有者信任的程度相同。 出于此原因，我们不建议采用这种方法。 有关使用组织域的详细信息，请参阅[使用组织域林模型](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md)。  

还需要打开防火墙，以允许 Active Directory 流量通过。 有关配置防火墙以与 AD DS 一起使用的详细信息，请参阅[由防火墙分段的网络中的 Active Directory](https://go.microsoft.com/fwlink/?LinkId=37928)。  

## <a name="BKMK_7"></a>方案7：使用资源林，并使用有限连接性为服务隔离重新配置防火墙  

如果连接受到限制并且需要服务隔离，则可以执行以下操作之一：  

- 使用组织林。 将需要服务隔离的组的用户、组和计算机放到单独的组织林中。 将该组中的个人分配为林所有者。 组织林在防火墙的另一端提供单独的环境。 林包括在林中管理的用户帐户和资源，以便用户无需通过防火墙即可完成日常任务。 特定的用户或应用程序可能有需要通过防火墙来联系其他林的特殊需求。 您可以通过在防火墙中打开相应的接口（包括任何信任功能所必需的接口），单独满足这些需求。  

- 使用资源林。 将资源和服务帐户置于单独的资源林中，并将用户帐户保留在现有的组织林中。 如果组织林变得不可用，则可能需要在资源林中创建一些替代用户帐户来维护对资源林的访问。 备用帐户必须具有登录到资源林所需的权限，并保持对资源的控制，直到组织林恢复联机。  

   在资源与组织林之间建立信任关系，以便用户可以在使用其常规用户帐户时访问林中的资源。 此配置启用用户帐户的集中管理，同时允许用户在组织林不可用时回退到资源林中的备用帐户。  

服务隔离的注意事项包括：  

- 为服务隔离创建的林可以信任来自其他林的域，但不得将其他林中的用户包括在其服务管理员组中。 如果其他林中的用户包括在隔离林中的管理组中，则可能会危及独立林的安全，因为林中的服务管理员没有独占控制。  

- 只要域控制器在网络上是可访问的，它们就会受到来自该网络上的计算机的攻击（例如拒绝服务攻击）。 你可以执行以下操作来防范攻击的可能性：  

   - 仅在被视为安全的网络上托管域控制器。  

   - 限制对承载域控制器的网络或网络的访问。  

- 服务隔离需要创建其他林。 评估维护基础结构以支持其他林的成本是否超过了由于组织林不可用而导致资源访问丢失相关的成本。  

   特定的用户或应用程序可能有需要通过防火墙来联系其他林的特殊需求。 您可以通过在防火墙中打开相应的接口（包括任何信任功能所必需的接口），单独满足这些需求。  

有关配置防火墙以与 AD DS 一起使用的详细信息，请参阅[由防火墙分段的网络中的 Active Directory](https://go.microsoft.com/fwlink/?LinkId=37928)。  

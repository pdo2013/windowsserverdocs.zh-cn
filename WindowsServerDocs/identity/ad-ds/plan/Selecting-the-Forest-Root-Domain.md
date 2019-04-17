---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: "选择森林根域"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d87273df1e42e1fa88df09e0b86d4c86ea75ee3f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="selecting-the-forest-root-domain"></a>选择森林根域

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在 Active Directory 森林部署的第一个域称为森林根域。 此域广告 DS 部署的生命周期保持森林根域。  
  
森林根域包含企业管理员和方案管理员的组。 这些服务管理员组用于管理森林级别操作，如添加和删除域模式将变为实现。  
  
选择森林根域包括确定可用作森林根域的 Active Directory 域中域设计之一是否需要部署专用的森林根域。  
  
有关部署森林根域的信息，请参阅[部署 Windows Server 2008 森林根域](https://technet.microsoft.com/library/cc731174.aspx)。  
  
## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>选择地区或专用森林根域  
如果应用单个域型号时，单个域可以充当森林根域。 如果应用多域模式时，你可以选择部署专用的森林根域，或者选择区域域用作森林根域。  
  
### <a name="dedicated-forest-root-domain"></a>专用的森林根域  
专用的森林根域是创建域专为森林根的功能。 它不包含任何用户帐户之外的森林根域服务管理员帐户。 此外，它不代表任何地理区域中域结构。 森林中的所有其他域是专用的森林根域的孩子。  
  
使用专门的森林根提供以下优势：  
  
-   从域的服务管理员的森林的服务管理员的分隔运营。 在单个域环境中的域管理员和内置管理员组成员可以使用标准工具和过程可让自己的企业管理员和方案管理员的组成员。 在树林中使用专用的森林根域的域管理员和内置管理员区域的域中的组成员无法本身的森林级别的服务管理员组成员使用进行标准工具和过程。  
  
-   从其他域中运营更改的保护。 专用的森林根域不代表特定地理区域中域结构。 出于此原因，它不会影响重新组织或其他导致重命名或重构域的更改。  
  
-   作为中性根，以便没有所在的国家或地区似乎已属于另一个区域。 某些组织可能想要避免该个国家/地区的外观或地区属于另一个国家或地区命名空间中。 当你使用专用的森林根域时，所有区域域可以域层次中的等。  
  
在多区域域环境中使用专用的森林根，森林根域复制上网络基础结构包含影响降至最低。 这是因为森林根仅承载的服务管理员帐户。 林和领域特有的其他数据的用户帐户的大多数存储在区域的域。  
  
一个缺点使用专用的森林根域是，它创建其他管理开销，以便支持其他域。  
  
### <a name="regional-domain-as-a-forest-root-domain"></a>作为森林根域区域域  
如果您不选择部署专用的森林根域，必须选择区域域用作森林根域。 此域所有其他区域的域的家长域，将你部署的第一个域。 森林根域包含用户帐户，以及管理其他区域的域进行管理的方式相同。 主要区别在于它还包含企业管理员和方案管理员的组。  
  
创建选择函数为某个区域域，因为森林根域，它不会创建更多管理开销维护额外的域的优势。 选择相应的区域域森林根，例如，表示你总部的域或已最快的网络连接的区域。 如果为你的组织来选择区域域是森林根域困难，可以选择要改用专用的森林根模型。  
  
## <a name="assigning-the-forest-root-domain-name"></a>指定森林根域名  
森林根域名也是森林的名称。 森林根名称是 prefix.suffix 的包含前缀形式职务域名系统 (DNS) 名称。 例如，组织可能设有森林根名称 corp.contoso.com。在此示例中，corp 前缀并且 contoso.com 职务。  
  
从你的网络上的名称，现有的列表中选择职务。 前缀，请选择不先前使用过你的网络的新名称。 通过将新前缀附加到现有职务，你创建一个唯一的命名空间。 创建新的命名空间适用于 Active Directory 域服务 (广告 DS) 确保了任何现有 DNS 基础结构不需要进行修改以适应广告 DS。  
  
### <a name="selecting-a-suffix"></a>选择职务  
若要选择森林根域职务：  
  
1.  有关列表组织的已注册的网络，可以将主机广告 DS 正在使用的 DNS 后缀联系 DNS 所有者。 请注意，在内部网络上使用的后缀可能不同于后缀外部使用。 例如，组织可能使用 contosopharma.com Internet 和 contoso.com 内部公司的网络。  
  
2.  请咨询选择用于广告 DS 职务 DNS 所有者。 如果没有合适后缀存在，请注册命名颁发机构 Internet 新名称。  
  
我们建议你使用注册的 Active Directory 命名空间中 Internet 颁发机构 DNS 名称。 保证注册的名称将是全球唯一。 如果稍后另一个组织注册 DNS 域同名 （或者，如果你的组织与合并、 获取，或由其他公司获取使用 DNS 同名），不能与另一交互两个基础结构。  
  
> [!CAUTION]  
> 不要使用单个标签 DNS 名称。 详细信息，请参阅有关配置为使用单个标签 DNS 名称域 Windows 的信息 ([https://go.microsoft.com/fwlink/?LinkId=106631](https://go.microsoft.com/fwlink/?LinkId=106631))。 此外，我们不建议使用注销的后缀，如.local。  
  
### <a name="selecting-a-prefix"></a>选择前缀  
选择是不是已在使用该网络上的已注册的职务下表中使用前缀规则选择前缀森林根域名。 添加前缀当前未使用，以创建新的附属名称。 例如，如果你 DNS 根名称 contoso.com，可以创建 Active Directory 森林根域名称 concorp.contoso.com，如果命名空间 concorp.contoso.com 尚未网络上使用。 此新分支命名空间的将致力于广告 DS，并且可以使用现有 DNS 实现轻松集成。  
  
如果你选择区域域用作森林根域，你可能需要选择一个新前缀域。 由于森林根域名影响所有森林中的其他域名，可能不适用地区组织根据的名称。 如果你使用的当前未使用该网络上的新职务，你可以使用它作为森林根域名而不选择其他前缀。  
  
下表中选择的已注册 DNS 名前缀规则。  
  
|规则|说明|  
|--------|---------------|  
|选择一个前缀，且不太可能时变为过期版本。|避免使用名称，如产品系列或可能在以后更改的操作系统。 我们建议使用通用名称，如 corp 或 ds。|  
|选择包含标准字符互联网前缀。|一个 Z、 一 z 0 到 9、 和 （-），但不是会完全数字。|  
|包含 15 字符一些或少一些前缀中。|如果你选择前缀长度 15 个字符的或更少，NetBIOS 名称是前缀相同。|  
  
务必 Active Directory DNS 所有者处理组织 DNS 所有者，以获得拥有的将用于 Active Directory 命名空间的名称。 有关设计 DNS 基础结构，以支持 DS 广告的详细信息，请参阅[创建 DNS 基础结构设计](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)。  
  
## <a name="documenting-the-forest-root-domain-name"></a>记录森林根域名  
DNS 前缀和森林根域对于你选择的职务文档。 此时，找出哪些域将森林根。 你可以向你创建可以记录你的套餐为新的和升级后域和你的域名"域计划"表中添加森林根域名信息。 若要打开它，请下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip 从工作辅助为 Windows Server 2003 部署工具包 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，然后打开"计划域"(DSSLOGI_5.doc)。  
  



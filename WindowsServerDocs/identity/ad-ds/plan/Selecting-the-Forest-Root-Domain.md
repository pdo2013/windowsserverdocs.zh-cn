---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: 选择林根域
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c669a5c8103fba52140147957823db978f8b6955
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871348"
---
# <a name="selecting-the-forest-root-domain"></a>选择林根域

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在 Active Directory 林中部署第一个域称为林根域。 此域在保持生命周期的 AD DS 部署目录林根域。  
  
目录林根域包含的 Enterprise Admins 和 Schema Admins 组。 这些服务管理员组用于管理林级操作，例如添加和删除的域以及对架构的更改的实现。  
  
选择目录林根域，涉及到决定如果一个域设计中的 Active Directory 域可以作为目录林根域或需要部署专用的林根级域。  
  
有关部署目录林根域的信息，请参阅[部署 Windows Server 2008 目录林根域](https://technet.microsoft.com/library/cc731174.aspx)。  
  
## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>选择区域或专用林根级域

如果要将应用一个域模型，目录林根域的单一域的作用。 如果要将应用多个域模型，可以选择要部署的专用的林根域或选择要用作目录林根域的区域的域。  
  
### <a name="dedicated-forest-root-domain"></a>专用的林根级域

专用的林根级域是域专门创建充当目录林根。 它不包含目录林根域的服务管理员帐户之外的任何用户帐户。 此外，它不表示您的域结构的任何地理区域。 在林中的所有其他域是专用的林根域的子域。  

使用专用的林根提供以下优势：  

- 从域服务管理员的林服务管理员的操作分离。 在单个域环境中，Domain Admins 和内置 Administrators 组的成员可以使用标准工具和过程可让自己 Enterprise Admins 和 Schema Admins 组的成员。 在使用专用的林根级域的林中，域管理员和地区域中的内置 Administrators 组的成员不能使本身林级别的服务管理员组的成员使用标准工具和过程。  
- 从其他域中操作方面的更改的保护。 专用的林根级域不表示您的域结构中的特定地理区域。 出于此原因，它不是受重组或导致的重命名或林内的域的其他更改。  
- 作为非特定根，以便任何国家/地区或区域显示为从属于另一个区域。 某些组织可能更愿意避免外观的一个国家/地区或区域是从属于另一个国家/地区或区域的命名空间中。 当使用专用的林根级域时，所有区域的域可以是域层次结构中的对等方。  

在多区域域环境中使用的专用的林根，林根级域的复制网络基础结构上出现影响降到最低。 这是因为目录林根仅托管的服务管理员帐户。 林和其他特定于域的数据中的用户帐户的大部分存储在地区域中。  
  
使用专用的林根级域的一个缺点在于，它创建额外的管理开销，以支持其他域。  
  
### <a name="regional-domain-as-a-forest-root-domain"></a>为目录林根级域区域域

如果您选择不部署专用的林根级域，必须选择要用作目录林根域的区域域。 此域是父域的所有其他区域的域，并且将部署的第一个域。 目录林根域包含的用户帐户，并与管理的其他区域的域相同的方式进行管理。 主要区别在于它还包括 Enterprise Admins 和 Schema Admins 组。  
  
创建利用选择区域对函数域作为目录林根域，它不会创建额外的管理开销维护其他域。 选择适当的区域域是目录林根目录，例如表示的总部的域或具有最快的网络连接的区域。 如果您选择要为目录林根域的区域域的组织很难，您可以选择以改为使用专用的林根模型。  
  
## <a name="assigning-the-forest-root-domain-name"></a>分配林根域的名称

林根域的名称也是林的名称。 林根名称为前缀和后缀形式的 prefix.suffix 组成的域名系统 (DNS) 名称。 例如，一个组织可能具有在目录林根名称 corp.contoso.com。 在此示例中，公司是前缀，contoso.com 是后缀。  
  
从网络上的现有名称的列表中选择的后缀。 对于前缀，选择未使用在网络以前的新名称。 通过将一个新的前缀附加到现有的后缀，则创建唯一的命名空间。 为 Active Directory 域服务 (AD DS) 中创建新的命名空间可确保任何现有的 DNS 基础结构不需要进行修改以适应 AD DS。  
  
### <a name="selecting-a-suffix"></a>选择一个后缀

若要选择为目录林根域后缀：  
  
1. 在将承载 AD DS 在网络上使用的已注册 DNS 后缀的列表的组织，请与 DNS 所有者联系。 请注意，内部网络上使用的后缀可能不同于在外部使用的后缀。 例如，组织可能使用 contosopharma.com 上的 Internet 和内部企业网络上的 contoso.com。  
  
2. 请查阅 DNS 所有者以选择与 AD DS 一起使用的后缀。 如果不存在任何合适的后缀，注册 Internet 命名颁发机构的新名称。  
  
我们建议使用注册到 Active Directory 命名空间中的 Internet 颁发机构的 DNS 名称。 要全局唯一，保证具有已注册的名称。 如果另一个组织更高版本注册相同的 DNS 域名 （或如果你的组织将与合并、 获取时，或获取的另一家公司使用相同的 DNS 名称），两个基础结构不能与另一个进行交互。  
  
> [!CAUTION]  
> 不要使用单标签 DNS 名称。 有关详细信息，请参阅有关如何为具有单标签 DNS 名称的域配置 Windows 信息 ([https://go.microsoft.com/fwlink/?LinkId=106631](https://go.microsoft.com/fwlink/?LinkId=106631))。 此外，我们不建议使用未注册的后缀，例如.local。  
  
### <a name="selecting-a-prefix"></a>选择一个前缀

如果你选择一个已注册的后缀，已在网络上使用，通过使用下表中的前缀规则选择林根域的名称的前缀。 添加当前未使用，以创建新的从属名称的前缀。 例如，如果你的 DNS 根名称为 contoso.com，可以创建 Active Directory 林根域名称 concorp.contoso.com，如果命名空间 concorp.contoso.com 已不在网络上使用。 命名空间的该新分支将专用于 AD DS，并且可以与现有的 DNS 实现轻松集成。  
  
如果您选择的区域的域，才能为目录林根级域，您可能需要选择新的域的前缀。 林根域的名称会影响所有林中其他域的名称，因为基于区域的名称可能不适用。 如果使用的新文件当前不在网络上使用的后缀，您可以使用它作为林根域的名称而不选择其他前缀。  
  
下表列出了用于选择已注册的 DNS 名称的前缀的规则。  
  
|规则|说明|  
|--------|---------------|  
|选择的前缀，则不可能变得过时。|避免使用如某个产品系列或将来可能会更改的操作系统的名称。 我们建议使用通用名称如 corp 或 ds。|  
|选择包括仅限 Internet 标准字符的前缀。|A-Z、 a-z、 0-9、 和 （-），但不是完全数字。|  
|包含 15 个字符或更短的前缀中。|如果您选择的前缀长度小于或等于的 15 个字符，NetBIOS 名称是与该前缀相同。|  
  
非常重要的 Active Directory DNS 所有者才能使用组织的 DNS 所有者才能获取将用于 Active Directory 命名空间的名称的所有权。 有关设计 DNS 基础结构来支持 AD DS 的详细信息，请参阅[创建 DNS 基础结构设计](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)。  
  
## <a name="documenting-the-forest-root-domain-name"></a>记录林根域的名称

记录的 DNS 前缀和后缀为目录林根域选择。 此时，标识哪些域将为目录林根。 可以将林根域名称信息添加到您创建的用于记录你计划为新的和升级域和你的域名的"域规划"工作表。 若要打开它，请下载从 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip[作业辅助工具为 Windows Server 2003 部署工具包](https://go.microsoft.com/fwlink/?LinkID=102558)并打开"域规划"(DSSLOGI_5.doc)。

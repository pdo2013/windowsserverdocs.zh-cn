---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: 选择林根域
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 80d39a5910d06559b98211eaf55a4cd0c82442a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402502"
---
# <a name="selecting-the-forest-root-domain"></a>选择林根域

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Active Directory 林中部署的第一个域称为林根域。 在 AD DS 部署的生命周期中，此域仍是目录林根级域。  
  
目录林根级域包含 Enterprise Admins 和 Schema Admins 组。 这些服务管理员组用于管理林级操作，例如添加和删除域以及实现架构更改。  
  
选择目录林根级域涉及确定域设计中 Active Directory 域之一是否可以充当目录林根级域，或者是否需要部署专用林根域。  
  
有关部署目录林根级域的信息，请参阅[部署 Windows Server 2008 林根域](https://technet.microsoft.com/library/cc731174.aspx)。  
  
## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>选择区域或专用林根域

如果要应用单个域模型，则单一域将充当目录林根级域。 如果你正在应用多个域模型，则可以选择部署专用林根域，或选择一个区域域作为目录林根级域。  
  
### <a name="dedicated-forest-root-domain"></a>专用林根级域

专用目录林根级域是专门创建的域，用作林根。 它不包含目录林根级域的服务管理员帐户以外的任何用户帐户。 此外，它不表示域结构中的任何地理区域。 林中的所有其他域都是专用林根域的子域。  

使用专用林根具有以下优势：  

- 域服务管理员对林服务管理员的操作分离。 在单一域环境中，Domain Admins 组和内置管理员组的成员可以使用标准工具和过程自行成为企业管理员和架构管理员组的成员。 在使用专用目录林根级域的林中，Domain Admins 和 Domain Admins 组的成员不能通过使用标准工具和过程，使其成为林级别服务管理员组的成员。  
- 防止其他域中的操作更改。 专用林根域不表示域结构中的特定地理区域。 出于此原因，它不受重组或其他导致重命名或重新构建域的更改的影响。  
- 用作非特定根，因此不会显示任何国家或地区。 某些组织可能希望避免一个国家或地区隶属于命名空间中的另一个国家或地区的外观。 使用专用目录林根级域时，所有区域域都可以是域层次结构中的对等。  

在使用专用林根的多区域域环境中，林根域的复制对网络基础结构的影响最小。 这是因为林根只承载服务管理员帐户。 林中的大部分用户帐户以及其他特定于域的数据存储在区域中。  
  
使用专用林根域的一个缺点是它会产生额外的管理开销，以支持其他域。  
  
### <a name="regional-domain-as-a-forest-root-domain"></a>作为林根域的区域域

如果选择不部署专用林根域，则必须选择一个区域域作为林根域。 此域是所有其他地区性域的父域，将是你部署的第一个域。 目录林根级域包含用户帐户，其管理方式与管理其他区域域的方式相同。 主要区别在于它还包括 Enterprise Admins 和 Schema Admins 组。  
  
选择区域域作为目录林根级域的优点在于，它不会创建维护附加域的额外管理开销。 选择适当的区域域作为林根节点，例如代表您的总部的域或网络连接速度最快的区域。 如果你的组织很难选择一个区域域作为目录林根级域，则可以改为选择使用专用林根模型。  
  
## <a name="assigning-the-forest-root-domain-name"></a>分配目录林根域名

林根域名也是林的名称。 林根名称是一个域名系统（DNS）名称，该名称由前缀和前缀后缀组成。 例如，组织可能将林根名称设置为 corp.contoso.com。 在此示例中，corp 是前缀，contoso.com 是后缀。  
  
从网络上的现有名称列表中选择 "后缀"。 对于前缀，请选择以前未在网络上使用的新名称。 通过将新前缀附加到现有后缀，可以创建唯一的命名空间。 为 Active Directory 域服务（AD DS）创建新的命名空间可确保无需修改任何现有的 DNS 基础结构即可容纳 AD DS。  
  
### <a name="selecting-a-suffix"></a>选择后缀

选择目录林根级域的后缀：  
  
1. 请联系组织的 DNS 所有者，以获取将承载 AD DS 的网络中使用的已注册 DNS 后缀列表。 请注意，在内部网络上使用的后缀可能不同于在外部使用的后缀。 例如，组织可能会在 Internet 上使用 contosopharma.com，并在内部公司网络中使用 contoso.com。  
  
2. 请参阅 DNS 所有者，以选择与 AD DS 一起使用的后缀。 如果不存在适当的后缀，请使用 Internet 命名机构注册一个新名称。  
  
建议你使用在 Active Directory 命名空间中向 Internet 颁发机构注册的 DNS 名称。 只有注册的名称才能保证是全局唯一的。 如果以后的其他组织注册了相同的 DNS 域名（或者，如果你的组织与使用相同 DNS 名称的另一家公司进行合并、获取或获取此域名），这两个基础结构将无法相互交互。  
  
> [!CAUTION]  
> 不要使用单标签 DNS 名称。 有关详细信息，请参阅有关为具有单标签 DNS 名称的域配置 Windows 的信息（[https://go.microsoft.com/fwlink/?LinkId=106631](https://go.microsoft.com/fwlink/?LinkId=106631)）。 此外，我们不建议使用未注册的后缀，如 local。  
  
### <a name="selecting-a-prefix"></a>选择前缀

如果选择了已在网络上使用的已注册后缀，请使用下表中的前缀规则为林根域名选择前缀。 添加当前未用于创建新的从属名称的前缀。 例如，如果 DNS 根名称是 contoso.com，则可以在网络上未使用命名空间 concorp.contoso.com 时，创建 Active Directory 林根域名 concorp.contoso.com。 命名空间的新分支将专门用于 AD DS，并可与现有的 DNS 实现轻松集成。  
  
如果选择了一个区域域作为目录林根级域，则可能需要为域选择新的前缀。 因为林根域名会影响林中的所有其他域名，所以，基于突破的名称可能不合适。 如果你使用的是当前未在网络中使用的新后缀，则可以将其用作林根域名，而无需选择其他前缀。  
  
下表列出了为已注册的 DNS 名称选择前缀的规则。  
  
|规则|说明|  
|--------|---------------|  
|选择不可能过时的前缀。|避免将来可能会更改的名称，如产品线或操作系统。 建议使用一般名称，如 corp 或 ds。|  
|选择仅包含 Internet 标准字符的前缀。|A-z、a-z、0-9 和（-），但并不完全是数字。|  
|前缀中包含15个或更少的字符。|如果选择的前缀长度不超过15个字符，则 NetBIOS 名称与前缀相同。|  
  
Active Directory DNS 所有者与组织的 DNS 所有者合作，以获取将用于 Active Directory 命名空间的名称的所有权，这一点非常重要。 有关设计 DNS 基础结构以支持 AD DS 的详细信息，请参阅[创建 Dns 基础结构设计](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)。  
  
## <a name="documenting-the-forest-root-domain-name"></a>记录目录林根域名

记录为林根域选择的 DNS 前缀和后缀。 此时，确定哪个域将成为林根。 您可以将目录林根域名信息添加到您创建的 "域计划" 工作表中，以便记录新域和升级域的计划以及域名。 若要打开它，请从[Windows Server 2003 部署工具包的作业帮助](https://go.microsoft.com/fwlink/?LinkID=102558)下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services，并打开 "域计划" （DSSLOGI_5）。

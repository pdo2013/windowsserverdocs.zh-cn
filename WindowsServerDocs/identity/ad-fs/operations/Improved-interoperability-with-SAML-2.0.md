---
ms.assetid: 80b5335b-fa02-4944-900c-5fe4f5c6111d
title: SAML 2.0 改进的互操作性
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4f55eaacec8ee0eb41e1980f1aa15c6256f8b979
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818728"
---
# <a name="improved-interoperability-with-saml-20"></a>SAML 2.0 改进的互操作性

>适用于：Windows Server 2016

  
Windows Server 2016 中的 AD FS 包含其他 SAML 协议的支持，包括支持导入基于包含多个实体的元数据的信任关系。  这使你可以配置 AD FS 参与 confederations 如 InCommon 联合身份验证并符合 eGov 2.0 标准其他实现。   
  
新功能基于组的信赖方或声明提供方信任。 每个组是 EntitiesDescriptor (< md:EntitiesDescriptor >) 元素中 eGov 指定 2.0 配置文件，其中包含一个或多个 EntityDescriptor 元素。  组将具有常见的授权规则，并可以修改所有其他属性类似于单个信任对象。  
  
一旦信任组导入到 AD FS，AD FS 作为基于元数据文档的组会自动更新信任关系。  
  
启用这些方案是使用新的 PowerShell commandlet 的添加和删除 AdfsClaimsProviderTrustsGroup 和 AdfsRelyingPartyTrustsGroup 对象一样简单。 这可以使用元数据 URL 或文件，如下面的示例中所示。  
  
此外，AD FS 2016 具有作用域参数对支持 SAML 核心规范，部分 3.4.1.2 中所述。 此元素允许信赖方指定一个或多个标识提供者进行身份验证请求。  
  
## <a name="examples"></a>示例  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataUrl "https://www.contosoconsortium.com/metadata/metadata.xml"   
```  
  
  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataFile "C:\metadata.xml"   
```  
  
## <a name="references"></a>参考  
  
可以找到 eGov 2.0 配置文件[此处。](https://kantarainitiative.org/confluence/download/attachments/60817482/kantara-report-egov-saml2-profile-2.0.pdf?version=1&modificationDate=1345580916000&api=v2)  
  
在 SAML 核心规范中可找到[此处。](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)   



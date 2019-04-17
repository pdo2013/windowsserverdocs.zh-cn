---
ms.assetid: 80b5335b-fa02-4944-900c-5fe4f5c6111d
title: "改进了与 SAML 2.0 的互操作性"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4f55eaacec8ee0eb41e1980f1aa15c6256f8b979
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="improved-interoperability-with-saml-20"></a>改进了与 SAML 2.0 的互操作性

>适用于：Windows Server 2016

  
在 Windows Server 2016 的广告 FS 包含其他 SAML 协议支持，包括导入信任根据所包含的多个实体元数据的支持。  这将使您可以配置广告 FS 参与 confederations 如 InCommon 联盟和符合 eGov 2.0 标准其他实现。   
  
新功能取决于信赖方或索赔提供商信任的组。 每个组是 EntitiesDescriptor (< md:EntitiesDescriptor >) 元素因为中 eGov 指定 2.0 配置文件，包含一个或多个 EntityDescriptor 元素。  组具有常见授权规则，并且所有其他属性可以进行修改类似个别信任对象。  
  
一旦信任组导入到广告 FS，广告 FS 自动更新信任基于元数据文档一组。  
  
启用这些方案是像使用新的 PowerShell commandlets 该添加和删除 AdfsClaimsProviderTrustsGroup AdfsRelyingPartyTrustsGroup 对象一样简单。 这可以使用 URL 元数据或文件，如以下示例所示。  
  
此外，广告 FS 2016 具有支持范围参数 SAML 核心指标，3.4.1.2 部分中所述。 此元素允许依赖方若要指定的一项或身份验证的更多的身份提供商请求。  
  
## <a name="examples"></a>示例  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataUrl "https://www.contosoconsortium.com/metadata/metadata.xml"   
```  
  
  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataFile "C:\metadata.xml"   
```  
  
## <a name="references"></a>引用  
  
找不到 eGov 2.0 个人资料[此处。](https://kantarainitiative.org/confluence/download/attachments/60817482/kantara-report-egov-saml2-profile-2.0.pdf?version=1&modificationDate=1345580916000&api=v2)  
  
找不到 SAML 核心规范[此处。](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)   



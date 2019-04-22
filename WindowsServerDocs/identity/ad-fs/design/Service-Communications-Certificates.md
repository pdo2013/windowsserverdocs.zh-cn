---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: 服务通信证书
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 32e60f2c2d9e4fced04061ace44882b792e1a3bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825648"
---
# <a name="service-communications-certificates"></a>服务通信证书

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

联合身份验证服务器在其中使用 WCF 消息安全性的情况下需要使用的服务通信证书。  
  
## <a name="service-communication-certificate-requirements"></a>服务通信证书要求  
服务通信证书必须满足以下要求才能与 AD FS 配合使用：  
  
-   服务通信证书必须包含服务器身份验证增强型密钥用法\(EKU\)扩展。  
  
-   证书吊销列表\(Crl\)必须是从服务通信证书的根 CA 证书链中的所有证书的访问。 此外必须由任何联合身份验证服务器代理和 Web 服务器信任此联合身份验证服务器的受信任的根 CA。  
  
-   可在服务通信证书的使用者名称必须与联合身份验证服务属性中的联合身份验证服务名称匹配。  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>有关服务通信证书的部署注意事项  
配置服务通信证书，以便所有联合服务器都使用相同的证书。 如果要部署联合 Web 单一\-符号\-上\(SSO\)设计中，我们建议你的服务通信证书由公共 CA 颁发。 可以请求和安装这些证书通过 IIS 管理器管理单元\-中。  
  
可以使用自助\-已签名，服务通信证书已成功在测试实验室环境中的联合身份验证服务器上。 但是，对于生产环境中，我们建议从公共 CA 获取服务通信证书。 以下是为何不应使用自身的原因\-已签名，服务通信证书用于实际部署：  
  
-   自\-已签名，SSL 证书必须添加到每个资源伙伴组织中的联合身份验证服务器上的受信任的根存储。 此单独不会启用攻击者泄露资源联合身份验证服务器，而信任自\-签名的证书也会增加计算机的攻击面，它可能会导致安全漏洞如果证书签名人不可高信度。  
  
-   它会创建不良用户体验。 在尝试访问显示以下消息的联合的资源时，客户端将接收安全警报的提示："安全证书已颁发由您不愿信任的公司。" 这是预期的行为，因为自\-签名的证书不受信任。  
  
    > [!NOTE]  
    > 如果有必要，您可以解决此条件通过使用组策略来手动推送自\-签名证书对访问 AD FS 网站将尝试每个客户端计算机上的受信任的根存储。  
  
-   Ca 提供的附加证书\-基于未提供的自助功能，例如私钥存档、 续订和吊销，\-签名证书。  
  


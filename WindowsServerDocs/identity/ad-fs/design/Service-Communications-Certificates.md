---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: 服务通信证书
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 624d2e26bc0277129e44eee3fdce7c7396b735a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407908"
---
# <a name="service-communications-certificates"></a>服务通信证书

对于使用 WCF 消息安全的方案，联合服务器需要使用服务通信证书。  
  
## <a name="service-communication-certificate-requirements"></a>服务通信证书要求  
服务通信证书必须满足以下要求才能处理 AD FS：  
  
-   服务通信证书必须包括服务器身份验证增强型密钥用法 \(EKU @ no__t 扩展。  
  
-   证书吊销列表必须可从服务通信证书到根 CA 证书的证书链中的所有证书访问 \(CRLs @ no__t-1。 根 CA 还必须受信任此联合服务器的任何联合服务器代理和 Web 服务器信任。  
  
-   服务通信证书中使用的使用者名称必须与联合身份验证服务的属性中的联合身份验证服务名称匹配。  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>服务通信证书的部署注意事项  
配置服务通信证书，以便所有联合服务器使用同一证书。 如果你正在部署联合 Web Single @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3 设计，则建议你的服务通信证书由公共 CA 颁发。 可以通过 IIS 管理器 snap @ no__t-0in 请求并安装这些证书。  
  
在测试实验室环境中，可以在联合服务器上成功使用 no__t-0signed 和 service communication 证书。 但是，对于生产环境，建议从公共 CA 获取服务通信证书。 以下是你不应使用 "no__t-0signed"、用于实时部署的服务通信证书的原因：  
  
-   必须在资源伙伴组织中的每台联合服务器上将 no__t-0signed、SSL 证书添加到受信任的根存储中。 虽然这种情况并不允许攻击者泄露资源联合服务器，但信任 no__t-0signed 证书会增加计算机的攻击面，如果证书签名者不是，则可能会导致安全漏洞。值得.  
  
-   它会导致用户体验不佳。 当客户端尝试访问显示以下消息的联合资源时，会收到安全警报提示："安全证书是由你未选择信任的公司颁发的。" 这是预期的行为，因为 no__t-0signed 证书不受信任。  
  
    > [!NOTE]  
    > 如有必要，可以使用组策略将 no__t-0signed 证书手动向下推送到将尝试访问 AD FS 站点的每台客户端计算机上的受信任根存储中，从而解决此问题。  
  
-   Ca 提供了其他证书 @ no__t-0based 功能，如私钥存档、续订和吊销，这些功能不是由自 @ no__t-1signed 证书提供的。  
  


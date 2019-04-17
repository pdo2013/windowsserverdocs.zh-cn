---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: "Service 通信证书"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 32e60f2c2d9e4fced04061ace44882b792e1a3bc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="service-communications-certificates"></a>Service 通信证书

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

联合服务器 WCF 邮件安全使用在其中的情况下需要服务通信证书的使用。  
  
## <a name="service-communication-certificate-requirements"></a>服务通信证书要求  
服务通信证书必须满足以下要求处理广告 FS:  
  
-   服务通信证书必须包括服务器身份验证增强密钥用法 \(EKU\) 扩展。  
  
-   必须访问为所有的证书链中从该服务通信证书根证书吊销列表 \(CRLs\)。 根还必须受任何联合身份验证的服务器代理和 Web 服务器信任此联合身份验证的服务器。  
  
-   在服务通信证书中使用的对象名称必须与联合身份验证服务属性联合身份验证服务的名称匹配。  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>部署服务通信证书注意事项  
配置服务通信证书，以便使用同一个证书，所有联合身份验证的服务器。 如果你正在部署联合 Web Single\-Sign\-On \(SSO\) 设计，我们建议你服务通信证书，由公共认证颁发。 你可以请求和安装这些通过 snap\ IIS 管理器中的证书。  
  
你可以使用 self\ 登录、 服务通信证书成功联盟服务器的测试实验环境中。 但是，对于生产环境中，我们建议你从公用 CA 获取服务通信证书。 以下是你应该不使用 self\ 登录，服务通信证书的动态部署为什么的原因：  
  
-   一个 self\ 登录，SSL 证书必须将添加到每个资源合作伙伴组织中联合身份验证的服务器上的受信任的根应用商店。 时，这只需如此便不会未启用攻击者泄露资源联盟服务器，信任 self\ 签名证书也会增加攻击的一台电脑，并且它不可靠签名证书者是否可能导致的安全漏洞。  
  
-   它创建错误的用户体验。 在尝试访问显示以下消息的联合的资源时，客户将收到安全警告提示:"的安全证书颁发公司你不愿信任。" 这是预期的行为，因为不受信任的 self\ 签名证书。  
  
    > [!NOTE]  
    > 如有必要，你可以通过使用组策略手动推 self\ 签名证书于受信任的根官方商城将尝试访问广告 FS 站点每个客户端计算机上解决这种情况。  
  
-   Ca 提供其他 certificate\ 基于的功能，如专用密钥存档、 续订和吊销、 未提供 self\ 签名证书的。  
  


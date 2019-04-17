---
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: "广告 fs 配置额外的身份验证方法"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 65baa6f94e1efa1fa337eab477b18f947cf0bb2d
ms.sourcegitcommit: 36d7b1dfd7da8e9f303d007a628e76149de000f2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2018
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>广告 fs 配置额外的身份验证方法

>适用于：Windows Server 2016，Windows Server 2012 R2

以启用多重身份验证 (MFA)，必须选择至少一个额外的身份验证方法。 默认情况下，在 Windows Server 2012 R2、Active Directory 联合身份验证服务 (AD FS) 你可以选择身份验证证书（即，智能卡基于身份验证）作为额外的身份验证方法。

> [!NOTE]
> 如果你选择的身份验证证书，请确保智能卡证书已牢固设置和已满足 pin 要求。

你知道 Microsoft Azure 提供类似的功能在云中吗？ 了解有关[Microsoft Azure 身份解决方案](http://aka.ms/m2w274)。<br /><br />在 Microsoft Azure 创建混合身份解决方案：<br /> - [了解有关 Azure 多重身份验证信息。](http://aka.ms/ey6o9r)<br /> - [管理单林混合使用云的身份验证的环境的身份。](http://aka.ms/g1jat8)<br /> - [管理敏感应用程序的其他多重身份验证的风险。](http://aka.ms/kt1bbm)|

## <a name="microsoft-and-third-party-additional-authentication-methods"></a>Microsoft 和第三方身份验证的其他方法
你还可以配置，并启用 Microsoft 和在 Windows Server 2012 R2 的广告 FS 中的第三方身份验证方法。 后，安装和使用广告 FS 注册，则可以执行 MFA 作为策略全球或每信赖-方身份验证的一部分。

下面是 Microsoft 和第三方提供商的 MFA 产品/服务的按字母顺序排列的列表中当前可在 Windows Server 2012 R2 的广告 fs。

||||
|-|-|-|
|**提供者**|**提供**|**若要了解详细信息的链接**|
|Gemalto|Gemalto 身份与安全服务|[http://www.gemalto.com/identity](http://www.gemalto.com/identity)|
|inWebo 技术|inWebo 企业级身份验证服务|[inWebo 企业级身份验证](http://www.inwebo.com)|
|登录人脉|登录人脉 MFA API 连接头有广告 FS 2012 R2（公共 beta 版）|[https://www.loginpeople.com](https://www.loginpeople.com)|
|MicrosoftCorp.|MicrosoftAzure MFA|[演练指南：管理敏感应用程序使用其他多重身份验证的风险](https://technet.microsoft.com/library/dn280946.aspx)（参见步骤 3）|
|RSA 的 EMC 安全除|Microsoft Active Directory 联合身份验证服务 RSA SecurID 验证代理|[Microsoft Active Directory 联合身份验证服务 RSA SecurID 验证代理](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet，Inc.|广告 fs SafeNet 身份验证服务 (SAS) 代理|[SafeNet 身份验证服务：广告 FS 代理配置指南](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|Swisscom|移动 ID 身份验证服务和签名服务|[移动 ID 身份验证服务](http://swisscom.ch/mid)|
|Symantec|Symantec 验证和 ID 保护服务 (VIP)|[Symantec 验证和 ID 保护服务 (VIP)](http://www.symantec.com/vip-authentication-service)|

## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 的广告 fs 自定义身份验证方法
我们现在提供了用于在 Windows Server 2012 R2 的广告 fs 构建自己的自定义身份验证方法的说明进行操作。 有关详细信息，请参阅[版本在 Windows Server 2012 R2 的广告 fs 自定义身份验证方法](https://go.microsoft.com/fwlink/?LinkID=511980)。

## <a name="see-also"></a>请参阅
[管理敏感应用程序的其他多重身份验证的风险](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)



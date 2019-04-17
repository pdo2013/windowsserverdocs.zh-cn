---
title: 组托管的服务帐户概述
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cef0693c-f861-48a7-a1c0-8d1bc06143ce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 4912ae273e603b4a3362c4984da710f780c3e8b3
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2018
---
# <a name="group-managed-service-accounts-overview"></a>组托管的服务帐户概述

>适用于：Windows Server（半年通道），Windows Server 2016

本主题的 IT 专业人员通过描述实际应用，引入组托管服务帐户更改在 Microsoft 的实现和硬件和软件要求。


## <a name="BKMK_OVER"></a>功能描述
独立托管服务帐户、 在 Windows Server 2008 R2 和 Windows 7 引入了，这是提供自动密码管理和简化的 SPN 管理，包括委派给其他管理员管理托管的域帐户。

组托管服务帐户提供相同的功能在域中，但也可以在多台服务器扩展该功能。 连接到服务器电场的日落，如网络加载余额，在托管服务时需要支持相互身份验证身份验证协议，所有实例服务的都使用的相同的原则。 当组托管服务帐户用作主体服务时，Windows 操作系统管理而不是依赖管理密码管理员帐户的密码。

Microsoft 键分发服务 \(kdssvc.dll\) 提供安全地获取最新密钥或 Active Directory 帐户与主要标识符特定密钥机制。 键分发服务共享幽静用于创建的帐户的键。 这些键会定期更改。 一组托管服务帐户域控制器计算上提供的项密钥 Distribution 服务，此外到其他属性组托管服务帐户的密码。  成员主机可以通过联系域控制器获取前面的当前密码值。

## <a name="BKMK_APP"></a>实际应用
组托管服务帐户服务器电场的日落，或网络加载余额后面的系统上运行的服务提供身份一个解决方案。 通过提供一组 MSA 解决方案，服务可以配置为新组主体的 MSA，并由 Windows 处理密码管理。

使用一组托管服务帐户、 服务或服务管理员不必管理密码同步之间服务实例。 组托管服务帐户支持多长的时间段内以及管理用于服务的所有实例成员主机保持离线状态下的主机。 这意味着你可以将支持现有客户端计算机可验证不知道的实例连接到该服务的单个身份一个 server 场部署。

故障转移群集 gMSAs 不支持。 但是，在群集服务顶部运行的服务可以使用 gMSA 或 sMSA 它们是 Windows 服务、 应用池、 计划的任务中，还是本地支持 gMSA 或 sMSA。

## <a name="BKMK_SOFT"></a>软件要求

64\ 位体系结构需运行的 Windows PowerShell 命令，用于进行管理组托管服务帐户。

托管的服务帐户方式取决于受支持的 Kerberos 加密类型。当使用的 Kerberos DC 创建向服务器身份验证的客户端计算机时 Kerberos 服务票证受保护加密 DC 和服务器的支持。 DC 使用该帐户 msDS\ SupportedEncryptionTypes 特性决定加密服务器支持，如果没有属性，假设客户端计算机不支持更强大的加密类型。 如果主机配置为不支持 RC4，将始终失败身份验证。 出于此原因，AES 应始终显式配置为 Msa。

> [!NOTE]
> 开始与 Windows Server 2008 R2、DES 默认处于禁用状态。 有关支持的加密类型的详细信息，请参阅[中 Kerberos 身份验证更改](https://technet.microsoft.com/library/dd560670(WS.10).aspx)。

组托管服务帐户并非适用于 Windows Server 2012 之前的 Windows 操作系统。

## <a name="server-manager-information"></a>服务器管理器信息
没有配置步骤必要实现的 MSA，并使用服务器管理器或 Install\ WindowsFeature cmdlet 的 MSA 进行分组。

## <a name="BKMK_LINKS"></a>请参阅
下表提供了与托管服务帐户和组托管服务帐户相关的其他资源的链接。

|键入的内容|引用|
|--------|-------|
|**产品评估**|[托管的服务帐户中的新增功能](what-s-new-for-managed-service-accounts.md)<br /><br />[托管的服务帐户 Windows 7 和 Windows Server 2008 R2 的文档](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[服务帐户 Step\ by\ 步指南](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**计划**|仍不可|
|**部署**|仍不可|
|**操作**|[托管服务帐户 Active Directory 中](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**故障排除**|仍不可|
|**评估**|[入门组托管服务帐户](getting-started-with-group-managed-service-accounts.md)|
|**工具和设置**|[托管服务帐户 Active Directory 域服务中](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**社区资源**|[了解托管的服务帐户:、 实现，最佳做法，和疑难解答](http://blogs.technet.com/b/askds/archive/2009/09/10/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)|
|**相关的技术**|[Active Directory 域名服务概述](active-directory-domain-services-overview.md)|



---
title: Group Managed Service Accounts Overview
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
ms.openlocfilehash: 24e3e3c15544de2f3bed4a7ef177b659e8095385
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836968"
---
# <a name="group-managed-service-accounts-overview"></a>Group Managed Service Accounts Overview

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题面向 IT 专业人员通过描述实际应用程序、 引入组托管服务帐户更改 Microsoft 的实现中，以及硬件和软件要求。


## <a name="BKMK_OVER"></a>功能说明
独立托管服务帐户 (sMSA) 是提供自动密码管理、 简化的服务主体名称 (SPN) 管理和委派给其他管理员管理的功能的托管的域帐户。 在 Windows Server 2008 R2 和 Windows 7 中引入了此类型的托管的服务帐户 (MSA)。

组托管服务帐户 (gMSA) 提供在域中相同的功能，但也通过多个服务器进行了扩展该功能。 连接到服务器场，如网络负载平衡解决方案上承载的服务时支持相互身份验证的身份验证协议要求服务的所有实例都使用相同的主体。 当为服务主体使用的是 gMSA 时，Windows 操作系统管理而不是依靠管理员管理密码的帐户的密码。

Microsoft 密钥分发服务\(kdssvc.dll\)提供安全地获取最新的密钥或 Active Directory 帐户与密钥标识符的特定密钥的机制。 密钥发行服务共享用于创建帐户密钥的机密。 这些密钥会定期更改。 对于 gMSA 的域控制器会计算密钥发行服务，以及 gMSA 的其他属性提供的密钥的密码。  成员主机可通过联系域控制器获得当前和以前的密码值。

## <a name="BKMK_APP"></a>实际应用程序
Gmsa 的服务器场上或在网络负载均衡器后面的系统上运行的服务提供了一个身份解决方案。 通过提供 gMSA 解决方案，可以将服务配置为新的 gMSA 主体和密码管理由 Windows 处理。

使用 gMSA 时，服务或服务管理员不需要管理服务实例之间的密码同步。 GMSA 支持保持脱机的长的时间段内，并管理服务的所有实例的成员主机的主机。 这意味着，你可以部署支持现有客户端计算机可进行身份验证的单个身份的服务器场，而无需知道其连接到的服务的实例。

故障转移群集不支持 gMSA。 但是，如果在群集服务上运行的服务是 Windows 服务、应用池、计划任务或是本机支持的 gMSA 或 sMSA，则它们可以使用 gMSA 或 sMSA。

## <a name="BKMK_SOFT"></a>软件要求

64\-运行的 Windows PowerShell 命令用于管理 Gmsa 所需的位体系结构。

托管服务帐户依赖 Kerberos 支持的加密类型。在客户端计算机使用 Kerberos 在服务器中进行身份验证时，DC 将创建使用 DC 和服务器同时支持的加密保护的 Kerberos 服务票证。 DC 使用帐户的 msDS\-SupportedEncryptionTypes 属性来确定哪些服务器支持的加密，并且如果没有属性，它假定客户端计算机不支持更强的加密类型。 如果主机配置为不支持 RC4，身份验证将始终失败。 出于此原因，应该始终显式针对 MSA 配置 AES。

> [!NOTE]
> 从 Windows Server 2008 R2 开始，默认情况下禁用 DES。 有关支持的加密类型的详细信息，请参阅 [Kerberos 身份验证中的变化](https://technet.microsoft.com/library/dd560670(WS.10).aspx)。

Gmsa 不适用于 Windows Server 2012 之前的 Windows 操作系统。

## <a name="server-manager-information"></a>服务器管理器信息
没有实现 MSA 和 gMSA 使用服务器管理器或安装所需的配置步骤\-WindowsFeature cmdlet。

## <a name="BKMK_LINKS"></a>另请参阅
下表提供了指向与托管服务帐户和组托管服务帐户相关的更多资源的链接。

|内容类型|参考|
|--------|-------|
|**产品评估**|[什么是托管的服务帐户的新增功能](what-s-new-for-managed-service-accounts.md)<br /><br />[托管的服务帐户适用于 Windows 7 和 Windows Server 2008 R2 文档](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[服务帐户的步骤\-通过\-步骤指南](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**规划**|尚未提供|
|**部署**|尚未提供|
|**操作**|[托管服务帐户在 Active Directory 中](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**疑难解答**|尚未提供|
|**评估**|[开始使用组托管服务帐户](getting-started-with-group-managed-service-accounts.md)|
|**工具和设置**|[托管 Active Directory 域服务中的服务帐户](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**社区资源**|[托管服务帐户：了解、 实现、 最佳做法和故障排除](http://blogs.technet.com/b/askds/archive/2009/09/10/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)|
|**相关技术**|[Active Directory 域服务概述](active-directory-domain-services-overview.md)|



---
title: Windows 身份验证体系结构
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07c9d6bb-9b03-407d-89b6-97c7551b256b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: fd6e2cbc233a91b62e3c8c9caf1154f91c3863ac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855858"
---
# <a name="windows-authentication-architecture"></a>Windows 身份验证体系结构

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本概述主题面向 IT 专业人员介绍了 Windows 身份验证的基本体系结构方案。

身份验证是通过该系统会验证用户的登录或单一登录信息的过程。 用户的名称和密码进行比较的已授权的列表，以及如果系统检测到匹配项，该用户的权限列表中指定的范围内授予访问权限。

可扩展的体系结构的一部分，Windows Server 操作系统可实现一组默认的身份验证安全支持提供者，包括协商、 Kerberos 协议、 NTLM、 Schannel （安全通道） 和摘要。 使用这些提供程序的协议启用的用户、 计算机和服务的身份验证和身份验证过程使授权的用户和服务以安全地访问资源。

在 Windows Server 中，应用程序进行身份验证的用户使用 SSPI 抽象进行身份验证的调用。 因此，开发人员不需要了解特定的身份验证协议的复杂性或构建到其应用程序的身份验证协议。

Windows Server 操作系统包括一组构成的 Windows 安全模型的安全组件。 这些组件确保应用程序无法获得访问权限而无需身份验证和授权的资源。 以下各节介绍身份验证体系结构的元素。

### <a name="local-security-authority"></a>本地安全机构
本地安全机构 (LSA) 是受保护的子系统，进行身份验证并登录到本地计算机的用户。 此外，LSA 还维护所有方面的信息本地安全的计算机上 （这些方面统称为本地安全策略）。 它还提供各种服务的名称和安全标识符 (Sid) 之间的转换。

跟踪安全策略和计算机系统上的帐户的安全子系统。 对于域控制器，这些策略和帐户是指那些是有效的域控制器所在的域。 这些策略和帐户存储在 Active Directory 中。 LSA 子系统提供服务，用于验证对对象的访问，检查用户权限和生成的审核消息。

### <a name="security-support-provider-interface"></a>安全支持提供程序接口
安全支持提供程序接口 (SSPI) 是获取身份验证、 消息完整性、 消息隐私性和安全的服务质量任何分布式应用程序协议的集成的安全服务的 API。

SSPI 是实现的通用安全服务 API (GSSAPI)。 SSPI 提供所依据的分布式应用程序可调用其中一个的多个安全提供程序以获取经过身份验证的连接而无需知道的安全协议的详细信息的机制。

## <a name="see-also"></a>请参阅

-   [安全支持提供程序接口体系结构](security-support-provider-interface-architecture.md)

-   [在 Windows 身份验证的凭据进程](credentials-processes-in-windows-authentication.md)

-   [Windows 身份验证技术概述](https://technet.microsoft.com/library/dn169029.aspx)



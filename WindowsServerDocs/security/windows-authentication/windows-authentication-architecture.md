---
title: Windows 身份验证体系结构
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4a9deef6481c1f7dacb56e8166584de1c59d613c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403277"
---
# <a name="windows-authentication-architecture"></a>Windows 身份验证体系结构

>适用于：Windows Server（半年频道）、Windows Server 2016

本概述主题面向 IT 专业人员，介绍了 Windows 身份验证的基本体系结构方案。

身份验证是指系统验证用户的登录或登录信息时所使用的过程。 用户的名称和密码与授权列表进行比较，如果系统检测到匹配项，则将访问权限授予该用户的权限列表中指定的范围。

作为可扩展体系结构的一部分，Windows Server 操作系统将实现一组默认身份验证安全支持提供程序，其中包括 Negotiate、Kerberos 协议、NTLM、Schannel （安全通道）和摘要。 这些提供程序使用的协议允许用户、计算机和服务进行身份验证，并且身份验证过程使授权的用户和服务能够安全地访问资源。

在 Windows Server 中，应用程序通过使用 SSPI 对身份验证调用进行身份验证来对用户进行身份验证。 因此，开发人员不需要了解特定身份验证协议的复杂性，也不需要在其应用程序中构建身份验证协议。

Windows Server 操作系统包括组成 Windows 安全模型的一组安全组件。 这些组件确保应用程序不需要进行身份验证和授权即可访问资源。 以下各节介绍身份验证体系结构的元素。

### <a name="local-security-authority"></a>本地安全机构
本地安全机构（LSA）是一个受保护的子系统，用于对用户进行身份验证并登录到本地计算机。 此外，LSA 还维护有关计算机上的本地安全所有方面的信息（这些方面统称为本地安全策略）。 它还提供了用于在名称和安全标识符（Sid）之间进行转换的各种服务。

安全子系统将跟踪计算机系统上的安全策略和帐户。 如果是域控制器，则这些策略和帐户是对域控制器所在的域生效的策略和帐户。 这些策略和帐户存储在 Active Directory 中。 LSA 子系统提供的服务可用于验证对对象的访问权限、检查用户权限以及生成审核消息。

### <a name="security-support-provider-interface"></a>安全支持提供程序接口
安全支持提供程序接口（SSPI）是为任何分布式应用程序协议获得用于身份验证、消息完整性、消息隐私和安全服务质量的集成安全服务的 API。

SSPI 是通用安全服务 API （GSSAPI）的实现。 SSPI 提供一种机制，通过该机制，分布式应用程序可以调用多个安全提供程序之一来获取经过身份验证的连接，而无需了解安全协议的详细信息。

## <a name="see-also"></a>请参阅

-   [安全支持提供程序接口体系结构](security-support-provider-interface-architecture.md)

-   [Windows 身份验证中的凭据进程](credentials-processes-in-windows-authentication.md)

-   [Windows 身份验证技术概述](https://technet.microsoft.com/library/dn169029.aspx)



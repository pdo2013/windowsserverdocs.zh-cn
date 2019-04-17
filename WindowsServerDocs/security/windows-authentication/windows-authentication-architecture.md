---
title: "Windows 身份验证建筑"
description: "Windows Server 安全"
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
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="windows-authentication-architecture"></a>Windows 身份验证建筑

>适用于：Windows Server（半年通道），Windows Server 2016

IT 专业人员的此概述主题介绍 Windows 身份验证的基本结构方案。

身份验证是一个用户登录或登录信息系统验证过程。 用户的名称和密码比较授权的列表中，并且如果系统检测匹配项，授予访问权限该用户的列表中指定的范围内。

作为可扩展的体系结构的一部分，Windows Server 操作系统实现身份验证安全支持提供，其中包括协商、 Kerberos 协议、 NTLM、 Schannel （安全通道），以及摘要默认的集。 使用这些提供商的协议启用的用户、 计算机和服务，身份验证，并身份验证过程使授权的用户和服务安全的方式访问资源。

在 Windows Server、 应用程序进行身份验证的用户使用 SSPI 抽象身份验证的呼叫。 因此，不需要的开发人员理解复杂性的特定的身份验证的协议或向他们的应用程序版本身份验证的协议。

Windows Server 操作系统包含一组安全组件构成 Windows 安全模式。 这些组件确保应用程序无法获得对资源，而不身份验证和授权的访问权限。 以下部分介绍了身份验证体系结构元素。

### <a name="local-security-authority"></a>本地安全颁发机构
本地安全颁发机构 (LSA) 是受保护的子系统验证身份，并且在本地计算机的用户进行签名。 此外，LSA 维护所有方面的信息本地安全的计算机上 （这些方面统称为本地安全策略）。 它还提供了各种名称和安全标识符 (Sid) 之间的翻译服务。

安全子系统跟踪的安全策略和对计算机系统上的帐户。 对于域控制器，这些策略和帐户是指实际上域控制器在所处的域。 Active Directory 中存储这些策略和帐户。 LSA 子系统提供的验证访问对象的服务、查看用户权限和生成审核消息。

### <a name="security-support-provider-interface"></a>安全支持的提供商界面
安全支持提供程序接口 (SSPI) 是获取集成的安全服务，身份验证、消息完整性，消息隐私和安全-服务质量分布式应用程序中的任何协议的 API。

SSPI 是实现的通用安全服务 API (GSSAPI)。 SSPI 提供的机制依据分布式应用程序可以进行呼叫几个的安全供应商以获取安全协议的详细信息的验证的不知情的情况下连接之一。

## <a name="see-also"></a>请参阅

-   [安全支持的提供商界面建筑](security-support-provider-interface-architecture.md)

-   [在 Windows 身份验证的凭据进程](credentials-processes-in-windows-authentication.md)

-   [Windows 身份验证 Technical 概述](https://technet.microsoft.com/library/dn169029.aspx)



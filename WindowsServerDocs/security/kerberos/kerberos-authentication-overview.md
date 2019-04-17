---
title: "Kerberos 身份验证概述"
description: "Windows Server 安全"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 646c6309-e865-4be2-b415-44dd125af5c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 9f8878fb959c34663b1e1f3858fa7e0b6e7b6105
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="kerberos-authentication-overview"></a>Kerberos 身份验证概述

>适用于：Windows Server（半年通道），Windows Server 2016

Kerberos 是用于验证用户或主机的身份验证协议。 本主题包含有关在 Windows Server 2012 和 Windows 8 Kerberos 身份验证信息。

## <a name="BKMK_OVER"></a>功能描述
Windows Server 操作系统均实施 Kerberos 版 5 身份验证协议和公开身份验证、传输授权的扩展数据和委派。 实现 Kerberos 身份验证客户端可以通过安全支持的提供商接口 \(SSPI\) 作为安全性支持供应商访问 \(SSP\)，并且它。 与 Winlogon 单个 sign\ 上建筑集成初始用户身份验证。

与其他域控制器运行的 Windows Server 安全服务，Kerberos 密钥 Distribution 中心 \(KDC\) 是已集成。 KDC 使用它的帐户安全数据库作为的域的 Active Directory 域服务数据库。 Active Directory 域服务，才能进行域或森林中的默认 Kerberos 实现。

## <a name="kerb_tr_Kerb_Benefits"></a>实际应用
使用基于 domain\ 身份验证 Kerberos 获得好处却是：

-   **委派身份验证。**

    在访问客户端的代表资源时，Windows 操作系统运行的服务可模拟客户端计算机。 在许多情况下，一项服务可以完成其工作客户端访问在本地计算机上的资源。 当客户端计算机验证到该服务时，NTLM 和 Kerberos 协议提供服务需要模拟客户端计算机本地身份验证信息。 但是，某些分布式的应用被设计，以便 front\ 结束服务必须使用客户端计算机的身份时连接到其他计算机上 back\ 结束服务。 Kerberos 身份验证支持启用时连接到其他服务代表其客户服务委派机制。

-   **上的单一登录。**

    身份验证在某个域，或者树林中使用 Kerberos 允许用户或服务访问允许不情况下输入凭据的多个请求管理员的资源。 上通过 Winlogon 初始域号之后, Kerberos 管理整个森林凭据，无论在试图资源访问。

-   **互操作性。**

    通过 Microsoft Kerberos 第 V5 协议实现取决于 Internet 工程任务强制 \(IETF\) 到建议的 standards\ 跟踪规范。 因此，在 Windows 操作系统、Kerberos 协议奠定使用其他网络 Kerberos 协议用于身份验证的互操作性。 此外，Microsoft 会发布 Windows 协议实现 Kerberos 协议的文档。 文档包含技术需求、限制、相关性和 Microsoft 实施的 Kerberos 协议 Windows\ 特定协议行为。

-   **更高效到服务器的身份验证。**

    之前 Kerberos，NTLM 身份验证无法用于，这需要连接到域控制器应用服务器身份验证的每个客户端计算机或服务。 与 Kerberos 协议，续订会话门票替换 pass\ 通过身份验证。 无需转到某个域控制器服务器 \（除非需要先验证特权特性证书 \(PAC\)\)。 相反，服务器可以进行身份验证的客户端计算机检查客户端呈现的凭据。 客户端计算机可以一次获取凭据特定服务器，然后重用这些网络登录会话整个的凭据。

-   **相互身份验证。**

    通过使用 Kerberos 协议，两端网络连接的第一方可验证另一端方是的实体它自称。 不支持客户端验证的服务器的身份或支持验证身份的另一个服务器。 NTLM 身份验证专为网络环境服务器被认为是正版。 Kerberos 协议做任何此类假定。

## <a name="see-also"></a>请参阅
[Windows 身份验证概述](../windows-authentication/windows-authentication-overview.md)



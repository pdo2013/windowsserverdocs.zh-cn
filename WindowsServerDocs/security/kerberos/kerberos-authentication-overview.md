---
title: Kerberos Authentication Overview
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 33712dc8502035bd9e47e1d2bdd4583eb8347dec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386315"
---
# <a name="kerberos-authentication-overview"></a>Kerberos Authentication Overview

>适用于：Windows Server（半年频道）、Windows Server 2016

Kerberos 是一个用于验证用户或主机身份的身份验证协议。 本主题包含有关 Windows Server 2012 和 Windows 8 中的 Kerberos 身份验证的信息。

## <a name="BKMK_OVER"></a>功能说明
Windows Server 操作系统可实现 Kerberos 版本 5 身份验证协议和对公钥身份验证的扩展，用于传输授权数据和委派。 Kerberos 身份验证客户端作为安全支持提供程序实现 \(SSP @ no__t，可通过安全支持提供程序接口 \(SSPI @ no__t 进行访问。 初始用户身份验证与 Winlogon 单一 sign @ no__t-0on 体系结构集成。

与在域控制器上运行的其他 Windows Server 安全服务相集成，Kerberos 密钥发行中心 @no__t 0KDC @ no__t。 KDC 使用域的 Active Directory 域服务数据库作为其安全帐户数据库。 Active Directory 域服务是域或林中的默认 Kerberos 实现所必需的。

## <a name="kerb_tr_Kerb_Benefits"></a>实用应用程序
使用 Kerberos 进行域 @ no__t-0based authentication 获得的好处是：

-   **委托身份验证。**

    在代表客户端访问资源时，在 Windows 操作系统上运行的服务可以模拟客户端计算机。 通常，服务通过访问本地计算机上的资源为客户端完成工作。 当客户端计算机向服务进行身份验证时，NTLM 和 Kerberos 协议都可以提供服务在本地模拟客户端计算机所需的授权信息。 但是，某些分布式应用程序的设计使前端 @ no__t 0end 服务在连接到其他计算机上的 back @ no__t-1end 服务时必须使用客户端计算机的标识。 Kerberos 身份验证支持一种委派机制，使服务在连接到其他服务时可以代表其客户端进行操作。

-   **单一登录。**

    在域或林中使用 Kerberos 身份验证将允许用户或服务访问管理员允许访问的资源，而无需多次请求凭据。 在通过 Winlogon 第一次登录域之后，Kerberos 将在每次尝试访问资源时管理整个林中的凭据。

-   **交互.**

    Microsoft 对 Kerberos V5 协议的实现基于标准 @ no__t-0track 规范，建议使用 Internet 工程任务组 \(IETF @ no__t。 因此，在 Windows 操作系统中，Kerberos 协议有助于实现与使用 Kerberos 协议进行身份验证的其他网络之间的互操作性。 此外，Microsoft 发布了有关实现 Kerberos 协议的 Windows 协议文档。 该文档包含 Microsoft 实现 Kerberos 协议的技术要求、限制、依赖项和 Windows @ no__t-0specific 协议行为。

-   **对服务器进行更高效的身份验证。**

    在 Kerberos 出现之前，可以使用 NTLM 身份验证，它要求应用程序服务器必须连接到域控制器，以便验证每个客户端计算机或服务的身份。 通过 Kerberos 协议，可续订 session ticket 替代 pass @ no__t-0through authentication。 服务器不需要 @no__t 到域控制器，它需要验证特权属性证书 \(PAC @ no__t-2 @ no__t。 服务器可以通过检查客户端出示的凭据来验证客户端计算机的身份。 客户端计算机可以获得一次特定服务器的凭据，然后在整个网络登录会话中重复使用这些凭据。

-   **相互身份验证。**

    通过使用 Kerberos 协议，网络连接两端的每一方可验证另一方所宣称的身份。 NTLM 不允许客户端验证服务器的身份，也不允许一个服务器验证另一个服务器的身份。 NTLM 身份验证旨在用于服务器假定为真的网络环境。 Kerberos 协议不进行此假设。

## <a name="see-also"></a>请参阅
[Windows 身份验证概述](../windows-authentication/windows-authentication-overview.md)



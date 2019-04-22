---
title: Kerberos Authentication Overview
description: Windows Server 安全
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824108"
---
# <a name="kerberos-authentication-overview"></a>Kerberos Authentication Overview

>适用于：Windows 服务器 （半年频道），Windows Server 2016

Kerberos 是一个用于验证用户或主机身份的身份验证协议。 本主题包含有关 Windows Server 2012 和 Windows 8 中 Kerberos 身份验证信息。

## <a name="BKMK_OVER"></a>功能说明
Windows Server 操作系统可实现 Kerberos 版本 5 身份验证协议和对公钥身份验证的扩展，用于传输授权数据和委派。 作为安全支持提供程序实现 Kerberos 身份验证客户端\(SSP\)，并可以通过安全支持提供程序接口来访问\(SSPI\)。 初始用户身份验证与 Winlogon 单个登录集成\-体系结构。

Kerberos 密钥分发中心\(KDC\)与其他域控制器运行的 Windows Server 安全服务集成。 KDC 使用域的 Active Directory 域服务数据库作为其安全帐户数据库。 Active Directory 域服务是域或林中的默认 Kerberos 实现所必需的。

## <a name="kerb_tr_Kerb_Benefits"></a>实际应用程序
通过使用 Kerberos 进行域获得好处\-所基于的身份验证：

-   **委派的身份验证。**

    代表客户端访问资源时，在 Windows 操作系统运行的服务可以模拟客户端计算机。 通常，服务通过访问本地计算机上的资源为客户端完成工作。 当客户端计算机向服务进行身份验证时，NTLM 和 Kerberos 协议都可以提供服务在本地模拟客户端计算机所需的授权信息。 但是，设计一些分布式应用程序，以便 front\-结束服务必须使用客户端计算机的标识，当它连接到后\-结束的其他计算机上的服务。 Kerberos 身份验证支持一种委派机制，使服务在连接到其他服务时可以代表其客户端进行操作。

-   **单一登录。**

    在域或林中使用 Kerberos 身份验证将允许用户或服务访问管理员允许访问的资源，而无需多次请求凭据。 在通过 Winlogon 第一次登录域之后，Kerberos 将在每次尝试访问资源时管理整个林中的凭据。

-   **互操作性。**

    由 Microsoft 对 Kerberos V5 协议的实现基于标准\-跟踪，建议使用到 Internet 工程任务组的规范\(IETF\)。 因此，在 Windows 操作系统中，Kerberos 协议有助于实现与使用 Kerberos 协议进行身份验证的其他网络之间的互操作性。 此外，Microsoft 发布了有关实现 Kerberos 协议的 Windows 协议文档。 文档包含技术要求、 限制、 依赖项，以及 Windows\-Kerberos 协议的 Microsoft 实现的特定协议行为。

-   **服务器身份验证更高效。**

    在 Kerberos 出现之前，可以使用 NTLM 身份验证，它要求应用程序服务器必须连接到域控制器，以便验证每个客户端计算机或服务的身份。 使用 Kerberos 协议，可续订的会话票证就取代了传递\-通过身份验证。 服务器不必定位到域控制器\(除非它需要验证特权属性证书\(PAC\)\)。 服务器可以通过检查客户端出示的凭据来验证客户端计算机的身份。 客户端计算机可以获得特定服务器的凭据后，即可重复使用这些凭据在整个网络登录会话。

-   **相互身份验证。**

    通过使用 Kerberos 协议，网络连接两端的每一方可验证另一方所宣称的身份。 NTLM 无法使客户端验证服务器的身份或使服务器之间相互验证身份的另一个。 NTLM 身份验证旨在用于服务器假定为真的网络环境。 Kerberos 协议不进行此假设。

## <a name="see-also"></a>请参阅
[Windows 身份验证概述](../windows-authentication/windows-authentication-overview.md)



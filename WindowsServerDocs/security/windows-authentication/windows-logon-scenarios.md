---
title: Windows 登录方案
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 66b7c568-67b7-4ac9-a479-a5a3b8a66142
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 07bfb538e1b43fc0c734b3c59b906c027ef985c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403294"
---
# <a name="windows-logon-scenarios"></a>Windows 登录方案

>适用于：Windows Server（半年频道）、Windows Server 2016

本参考主题面向 IT 专业人员，概述常见的 Windows 登录和登录方案。

Windows 操作系统要求所有用户使用有效的帐户登录到计算机以访问本地和网络资源。 基于 Windows 的计算机通过实现登录进程（在其中对用户进行身份验证）来保护资源。 在对用户进行身份验证后，授权和访问控制技术实现了保护资源的第二阶段：确定经过身份验证的用户是否有权访问某个资源。

本主题的内容适用于本主题开头 "**适用**于" 列表中指定的 Windows 版本。

此外，应用程序和服务可能要求用户登录以访问应用程序或服务提供的资源。 登录过程类似于登录过程，因为需要有效的帐户和正确的凭据，但登录信息存储在本地计算机的安全帐户管理器（SAM）数据库中，并且在适用的 Active Directory 中。 登录帐户和凭据信息由应用程序或服务进行管理，并且可以选择本地存储在凭据保险箱中。

若要了解身份验证的工作原理，请参阅[Windows 身份验证概念](windows-authentication-concepts.md)。

本主题介绍以下方案：

-   [交互式登录](#BKMK_InteractiveLogon)

-   [网络登录](#BKMK_NetworkLogon)

-   [智能卡登录](#BKMK_SmartCardLogon)

-   [生物识别登录](#BKMK_BioLogon)

## <a name="BKMK_InteractiveLogon"></a>交互式登录
当用户在 "凭据输入" 对话框中输入凭据时，或当用户将智能卡插入智能卡读卡器或用户与生物识别设备交互时，登录过程开始。 用户可以通过使用本地用户帐户或域帐户登录到计算机，来执行交互式登录。

下图显示了交互式登录元素和登录过程。

![显示交互式登录元素和登录过程的关系图](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Windows 客户端身份验证体系结构**

### <a name="BKMK_LocaDomainLogon"></a>本地和域登录
用户为域登录提供的凭据包含本地登录所需的所有元素，如帐户名和密码或证书，以及 Active Directory 域信息。 此过程将向用户的本地计算机上的安全数据库或 Active Directory 域确认用户的身份。 不能为域中的用户关闭此必需的登录过程。

用户可以通过以下两种方式之一对计算机执行交互式登录：

-   当用户直接对计算机进行物理访问，或计算机是计算机网络的一部分时，本地。

    本地登录授予用户访问本地计算机上的 Windows 资源的权限。 本地登录要求用户在本地计算机上的安全帐户管理器（SAM）中拥有用户帐户。 SAM 以本地计算机注册表中存储的安全帐户的形式来保护和管理用户和组信息。 计算机可以有网络访问权限，但这不是必需的。 本地用户帐户和组成员身份信息用于管理对本地资源的访问。

    网络登录授予用户访问本地计算机上的 Windows 资源的权限，以及凭据的访问令牌定义的联网计算机上的任何资源。 本地登录和网络登录都要求用户在本地计算机上的安全帐户管理器（SAM）中拥有用户帐户。 本地用户帐户和组成员身份信息用于管理对本地资源的访问，用户的访问令牌定义了哪些资源可在联网计算机上访问。

    本地登录和网络登录不足以授予用户和计算机访问和使用域资源的权限。

-   通过终端服务或远程桌面服务（RDS）远程执行，在这种情况下，登录将进一步限定为远程交互。

交互式登录后，Windows 代表用户运行应用程序，用户可以与这些应用程序交互。

本地登录授予用户访问本地计算机上的资源或联网计算机上的资源的权限。 如果计算机已加入域，则 Winlogon 功能尝试登录到该域。

域登录授予用户访问本地和域资源的权限。 域登录要求用户在 Active Directory 中具有用户帐户。 计算机必须具有 Active Directory 域中的帐户，并且必须以物理方式连接到网络。 用户还必须具有登录到本地计算机或域的用户权限。 域用户帐户信息和组成员身份信息用于管理对域和本地资源的访问权限。

### <a name="BKMK_RemoteLogon"></a>远程登录
在 Windows 中，通过 "远程登录" 访问其他计算机依赖于远程桌面协议（RDP）。 由于用户必须已成功登录到客户端计算机，然后才能尝试远程连接，因此交互式登录过程已成功完成。

RDP 管理用户使用远程桌面客户端输入的凭据。 这些凭据适用于目标计算机，并且用户必须具有该目标计算机上的帐户。 此外，目标计算机必须配置为接受远程连接。 发送目标计算机凭据以尝试执行身份验证过程。 如果身份验证成功，用户将连接到可使用提供的凭据访问的本地和网络资源。

## <a name="BKMK_NetworkLogon"></a>网络登录
只有用户、服务或计算机身份验证发生后，才能使用网络登录。 在网络登录期间，此过程不使用凭据条目对话框来收集数据。 而是使用以前建立的凭据或使用其他方法收集凭据。 此过程将向用户尝试访问的任何网络服务确认用户的身份。 除非必须提供备用凭据，否则用户通常不会看到此过程。

为了提供这种类型的身份验证，安全系统包括以下身份验证机制：

-   Kerberos 版本5协议

-   公钥证书

-   安全套接字层/传输层安全（SSL/TLS）

-   Digest

-   NTLM，与基于 Microsoft Windows NT 4.0 的系统兼容

有关元素和过程的信息，请参阅上面的交互式登录关系图。

## <a name="BKMK_SmartCardLogon"></a>智能卡登录
智能卡只能用于登录到域帐户，而不能用于本地帐户。 智能卡身份验证要求使用 Kerberos 身份验证协议。 Windows 2000 Server 中引入的，在基于 Windows 的操作系统中，将实现 Kerberos 协议初始身份验证请求的公钥扩展。 与共享密钥加密相比，公钥加密是不对称的，也就是说，需要两个不同的密钥：一个用于加密，另一个用于解密。 同时，执行这两个操作所需的密钥构成了私钥/公钥对。

若要启动典型的登录会话，用户必须通过提供仅供用户和基础 Kerberos 协议基础结构知道的信息来证明其身份。 机密信息是从用户的密码派生的加密共享密钥。 共享密钥是对称的，这意味着加密和解密使用同一密钥。

下图显示了智能卡登录所需的元素和过程。

![显示智能卡登录所需的元素和过程的图示](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**智能卡凭据提供程序体系结构**

当使用智能卡而不是密码时，存储在用户智能卡上的私钥/公钥对将替换为从用户的密码派生的共享密钥。 私钥仅存储在智能卡上。 公钥可供所有者要交换机密信息的任何人使用。

有关 Windows 中智能卡登录过程的详细信息，请参阅[windows 中的智能卡登录的工作原理](https://technet.microsoft.com/library/ff404285.aspx)。

## <a name="BKMK_BioLogon"></a>生物识别登录
设备用于捕获和生成项目的数字特征，如指纹。 然后，将此数字表示形式与相同项目的示例进行比较，并在两个成功比较时进行身份验证。 运行本主题开头的 "**适用**于" 列表中指定的任意操作系统的计算机都可以配置为接受这种形式的登录。 但是，如果生物识别登录仅配置为本地登录，则用户在访问 Active Directory 域时需要提供域凭据。

## <a name="additional-resources"></a>其他资源
有关 Windows 如何管理在登录过程中提交的凭据的信息，请参阅[Windows 身份验证中的凭据管理](https://technet.microsoft.com/library/dn169014.aspx)。

[Windows 登录和身份验证技术概述](https://technet.microsoft.com/library/dn169029.aspx)



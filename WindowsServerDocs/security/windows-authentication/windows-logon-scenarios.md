---
title: Windows 登录方案
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d6a1d52bee3c2d14ea83ccb794ecea1872868df5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828178"
---
# <a name="windows-logon-scenarios"></a>Windows 登录方案

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本参考主题面向 IT 专业人员总结了常见的 Windows 登录和单一登录方案。

Windows 操作系统要求所有用户登录到使用有效的帐户访问本地计算机和网络资源。 基于 Windows 的计算机通过实现登录过程中，用户进行身份验证保护的资源。 用户进行身份验证后，授权和访问控制技术实现保护资源的第二个阶段： 确定经过身份验证的用户有权访问的资源。

本主题的内容适用于版本的 Windows 中指定**适用于**本主题开头的列表。

此外，应用程序和服务可以要求用户登录以访问应用程序或服务提供这些资源。 有效的帐户和正确的凭据是必需的但登录信息存储在本地计算机上的安全帐户管理器 (SAM) 数据库以及在 Active Directory 中 （如果适用），是类似于登录过程中，登录过程。 登录帐户和凭据信息由应用程序或服务，并根据需要可以本地存储在凭据保险箱中。

若要了解身份验证的工作原理，请参阅[Windows 身份验证概念](windows-authentication-concepts.md)。

本主题介绍以下方案：

-   [交互式登录](#BKMK_InteractiveLogon)

-   [网络登录](#BKMK_NetworkLogon)

-   [智能卡登录](#BKMK_SmartCardLogon)

-   [生物识别登录](#BKMK_BioLogon)

## <a name="BKMK_InteractiveLogon"></a>交互式登录
当用户在凭据条目对话框中，输入凭据或用户在智能卡插入智能卡读卡器，或时使用生物识别设备的用户交互时，将开始登录过程。 用户可以通过使用本地用户帐户或域帐户来登录到计算机上执行交互式登录。

下图显示了交互式登录元素和登录过程。

![显示的交互式登录元素和登录过程的关系图](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Windows 客户端身份验证体系结构**

### <a name="BKMK_LocaDomainLogon"></a>本地和域登录
提供的域登录名的用户凭据包含所需的本地登录，如帐户名称和密码或证书和 Active Directory 域信息的所有元素。 该过程会确认到用户的本地计算机上的安全数据库或对 Active Directory 域的用户的标识。 不能为域中的用户禁用此必需登录过程。

用户可以执行交互式登录到计算机中通过两种方式：

-   本地，当用户具有直接物理访问计算机，或在计算机的计算机的网络的一部分时。

    本地登录授予用户访问本地计算机上的 Windows 资源的权限。 本地登录要求用户具有用户帐户在本地计算机上中安全帐户管理器 (SAM)。 SAM 保护并管理用户和组信息存储在本地计算机注册表中的安全帐户的形式。 计算机可以具有网络访问权限，但不需要。 本地用户帐户和组成员身份信息用于管理本地资源的访问权限。

    网络登录授予用户定义的凭据的访问令牌访问除了联网的计算机上的任何资源在本地计算机上的 Windows 资源的权限。 本地登录和网络登录要求用户具有用户帐户在本地计算机上中安全帐户管理器 (SAM)。 本地用户帐户和组成员身份信息用于管理本地资源访问权限和用户的访问令牌定义哪些资源可以访问联网计算机上。

    本地登录和网络登录不是只需授予用户和计算机的权限以访问并使用域资源。

-   远程，通过终端服务或远程桌面服务 (RDS)，在其中登录用例是进一步限定为远程交互。

交互式登录之后，Windows 运行应用程序代表用户和用户可以与这些应用程序交互。

本地登录授予对本地计算机上的访问资源或联网的计算机上的资源的用户权限。 如果将计算机加入到域中，Winlogon 功能尝试登录到该域。

域登录名授予用户权限以访问本地和域资源。 域登录名要求用户具有用户帐户在 Active Directory 中。 计算机必须具有 Active Directory 域中的帐户，并会以物理方式连接到网络。 用户还必须登录到本地计算机或域的用户权限。 使用域用户帐户信息和组成员身份信息来管理域和本地资源访问权限。

### <a name="BKMK_RemoteLogon"></a>远程登录
在 Windows 中，通过远程登录访问另一台计算机依赖于远程桌面协议 (RDP)。 由于用户必须已具有已成功登录到客户端计算机尝试远程连接之前，已成功完成交互式登录过程。

RDP 管理用户输入通过使用远程桌面客户端的凭据。 这些凭据适用于目标计算机，并且用户必须具有该目标计算机上的帐户。 此外，目标计算机必须配置为接受远程连接。 目标计算机凭据发送以尝试执行身份验证过程。 如果身份验证成功，用户连接到本地，网络资源的访问通过使用提供的凭据。

## <a name="BKMK_NetworkLogon"></a>网络登录
用户、 服务或计算机身份验证发生之后，可以仅使用网络登录。 网络登录期间进程不使用凭据条目对话框收集数据。 相反，以前建立的凭据或使用另一种方法来收集凭据。 此过程将确认为该用户尝试访问任何网络服务的用户的标识。 此过程是通常对用户不可见，除非必须提供备用凭据。

若要提供此类型的身份验证，安全系统包括这些身份验证机制：

-   Kerberos 版本 5 协议

-   公钥证书

-   安全套接字层/传输层安全性 (SSL/TLS)

-   Digest

-   NTLM，与基于 Microsoft Windows NT 4.0 的系统的兼容性

有关元素和进程的信息，请参阅上面的交互式登录关系图。

## <a name="BKMK_SmartCardLogon"></a>智能卡登录
可以使用智能卡仅对域帐户，而非本地帐户登录。 智能卡身份验证要求使用 Kerberos 身份验证协议。 引入了在 Windows 2000 Server 中，在基于 Windows 的操作系统中公共密钥扩展到 Kerberos 实现协议的初始身份验证请求。 与共享机密加密，公钥加密是非对称，也就是说，需要使用两个不同密钥： 一个用于加密，另一个用来解密。 在一起，执行这两种操作所需的键构成了私钥/公钥对。

若要启动一个典型的登录会话，用户必须知道只对用户和底层的 Kerberos 协议基础结构的信息，从而证明其身份。 机密信息是从用户的密码派生的加密共享的密钥。 共享的密钥是对称的这意味着同一个密钥用于加密和解密。

下图显示的元素和所需的智能卡登录的过程。

![显示的元素和所需的智能卡登录过程的关系图](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**智能卡凭据提供程序体系结构**

而不是密码使用智能卡时，将从用户的密码派生的共享机密密钥替换存储在用户的智能卡上的私钥/公钥对。 私钥存储仅在智能卡上。 公钥可以可向任何人所有者希望与之交换机密信息。

有关 Windows 中的智能卡登录过程的详细信息，请参阅[智能卡登录的工作原理在 Windows 中](https://technet.microsoft.com/library/ff404285.aspx)。

## <a name="BKMK_BioLogon"></a>生物识别登录
设备用于捕获和生成的某个项目，如指纹数字特征。 此数字表示形式进行比较的示例相同的项目，并时成功进行了比较两个，可以进行身份验证。 运行任何中指定的操作系统的计算机**适用于**本主题开头的列表可以配置为接受这种形式的登录。 但是，如果生物识别登录仅配置为本地登录，用户需要访问 Active Directory 域时提供域凭据。

## <a name="additional-resources"></a>其他资源
有关 Windows 如何管理凭据登录过程中提交的信息，请参阅[在 Windows 身份验证的凭据管理](https://technet.microsoft.com/library/dn169014.aspx)。

[Windows 登录和身份验证技术概述](https://technet.microsoft.com/library/dn169029.aspx)



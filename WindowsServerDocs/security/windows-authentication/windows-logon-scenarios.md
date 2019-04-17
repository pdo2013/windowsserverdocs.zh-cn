---
title: "Windows 登录方案"
description: "Windows Server 安全"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="windows-logon-scenarios"></a>Windows 登录方案

>适用于：Windows Server（半年通道），Windows Server 2016

IT 专业人员的此参考主题总结了 Windows 的通用登录和登录方案。

Windows 操作系统的系统要求登录到拥有有效访问本地帐户的计算机和网络资源的所有用户。 基于 Windows 的计算机安全通过实现登录过程中，用户进行身份验证的资源。 授权和访问控制技术用户验证后，实现保护资源的第二个阶段：如何确定身份验证的用户是否有权访问资源。

适用于版本的 Windows 中指定的本主题中的内容**适用于**开头的本主题列表。

此外，应用程序和服务可以需要用户在登录来访问的应用程序或服务提供这些资源。 登录过程也是相似登录过程中，在于一个有效的帐户，并正确的凭据要求，但登录信息存储在本地计算机上的安全帐户管理器 （三千） 数据库和 Active Directory （如果适用）。 登录帐户并凭据信息由该应用或服务，并且 （可选） 可以可以本地存储在凭据保险箱内。

若要了解身份验证的工作方式，请参阅[Windows 身份验证的概念](windows-authentication-concepts.md)。

本主题介绍以下情形：

-   [交互式登录](#BKMK_InteractiveLogon)

-   [网络登录](#BKMK_NetworkLogon)

-   [智能卡登录](#BKMK_SmartCardLogon)

-   [生物登录](#BKMK_BioLogon)

## <a name="BKMK_InteractiveLogon"></a>交互式登录
登录过程开始用户在凭据条目对话框中，输入凭据时或用户智能读卡器中插入一张智能卡或时用户的生物设备进行交互。 用户可以通过使用的本地用户帐户或域帐户，登录到一台计算机上执行交互式登录。

下图显示的交互式登录元素和登录过程。

![显示交互式登录元素和直观的图示](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Windows 客户端身份验证建筑**

### <a name="BKMK_LocaDomainLogon"></a>本地和域登录
对于域登录的用户提供的凭据包含所有必需的本地登录，如的帐户名称和密码或证书，以及 Active Directory 域信息元素。 该过程确认给用户的本地计算机上的安全数据库或 Active Directory 域的用户的身份。 此强制登录过程某个域中的用户无法关闭。

用户可以在两种方式之一执行交互登录到一台计算机：

-   本地，当用户可以直接物理访问到计算机，或计算机处于网络计算机的一部分。

    本地登录授予用户 Windows 资源本地计算机上的访问权限。 本地登录需要用户拥有一个用户帐户在本地计算机上的安全帐户管理器 （三千）。 三千保护和管理用户和组中的形式存储在本地计算机注册表了安全帐户的信息。 计算机产生网络访问权限，但不需要。 本地用户帐户和组成员的信息用于管理访问本地资源。

    网络登录授予访问除了联网计算机上的任何资源本地计算机上的 Windows 资源，由凭据访问令牌用户权限。 本地登录和网络登录需要用户拥有一个用户帐户在本地计算机上的安全帐户管理器 （三千）。 本地用户帐户和组成员的信息用于管理访问本地资源，并访问令牌用户定义哪些资源可访问联网计算机上。

    本地登录和网络登录没有足够授予用户和计算机的权限来访问和使用域资源。

-   远程，通过终端服务或远程桌面服务 (RDS)，在这种情况下登录是进一步限定为远程交互。

交互式的登录后 Windows 运行代表用户，应用程序，并且用户可以使用这些应用程序进行交互。

本地登录授予用户到本地计算机上的访问权限资源或联网计算机上的资源的权限。 如果计算机已加入域，然后尝试登录到该域 Winlogon 功能。

域登录授予权访问本地用户和域资源。 域登录需要用户 Active Directory 中有一个用户帐户。 计算机必须具有 Active Directory 域中的帐户，并物理连接到该网络。 用户必须还有用户在登录到本地计算机或的域的权限。 域用户的帐户信息和组成员信息用于管理访问域和当地的资源。

### <a name="BKMK_RemoteLogon"></a>远程登录
在 Windows 中，通过远程登录访问其他计算机依赖于远程桌面协议 (RDP)。 用户必须已成功登录到客户端计算机尝试远程连接之前，因为已成功完成交互式登录进程。

RDP 管理用户输入使用远程桌面客户端的凭据。 这些凭据适用于目标计算机，并且用户必须具有该目标计算机上的帐户。 此外，必须配置目标计算机以接受远程连接。 目标计算机凭据发送尝试执行身份验证过程。 身份验证是否成功，用户已连接到本地和网络资源，都可以访问使用提供的凭据。

## <a name="BKMK_NetworkLogon"></a>网络登录
后发生用户、 服务或计算机的身份验证，只能使用网络登录。 在网络登录进程不会使用凭据条目对话框中收集数据。 相反，以前建立凭据或使用另一种方法来收集的凭据。 此过程确认用户尝试访问的任何网络服务中的用户的身份。 提供所需的凭据备用才通常用户看不到此过程。

为了提供这种类型的身份验证，安全系统包含这些身份验证机制：

-   Kerberos 版本 5 协议

-   公钥证书

-   安全套接字层月传输层安全性 (TLS 月 SSL)

-   摘要

-   NTLM，与 Microsoft Windows NT 4.0 基于系统的兼容性

有关元素和进程的信息，请参阅上面的交互式登录的关系图。

## <a name="BKMK_SmartCardLogon"></a>智能卡登录
智能卡可用于仅次于域帐户，而非本地帐户登录。 智能卡身份验证要求使用 Kerberos 身份验证协议。 引入 Windows 2000 Server、 基于 Windows 的操作系统中实现 Kerberos 协议的初始身份验证请求的公用主要扩展。 共享密钥加密，与公钥对称，即，需要两种不同的键：一个用于另一个解密加密。 在一起，所需执行两项操作的键构成专用/公众关键配对。

若要启动典型登录会话，用户必须通过提供只有用户和基础 Kerberos 协议基础结构知道的信息证明他或她的身份。 机密信息是共享加密密钥得出用户的密码。 共享的密钥为对称，这意味着同一个键用于加密和解密。

下图显示的元素和智能卡登录所需的进程。

![显示元素和进程智能卡登录所需的图示](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**智能卡凭据提供商建筑**

使用智能卡时的用途而不是一个密码，存储在用户的智能卡专用/公钥对替代共享密钥，其中得出用户的密码。 仅在智能卡上存储专用键。 公钥可以可向任何与之所有者要交换机密信息。

有关 Windows 中的智能卡登录过程的详细信息，请参阅[智能卡登录的工作方式在 Windows 中](https://technet.microsoft.com/library/ff404285.aspx)。

## <a name="BKMK_BioLogon"></a>生物登录
设备用于捕获并版本数字项目，如指纹的特征。 此数字表示进行比较的同一项目，示例和两个成功比较时，可以进行身份验证。 计算机运行的任一中指定的操作系统**适用于**可配置列表开头的本主题以接受这种登录。 但是，如果生物登录仅配置为本地登录时，用户需要访问 Active Directory 域时提供域的凭据。

## <a name="additional-resources"></a>更多资源
有关 Windows 如何管理凭据登录过程提交的信息，请参阅[在 Windows 身份验证的凭据管理](https://technet.microsoft.com/library/dn169014.aspx)。

[Windows 登录并验证 Technical 概述](https://technet.microsoft.com/library/dn169029.aspx)



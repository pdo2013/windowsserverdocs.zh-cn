---
title: 配置证书模板 PEAP 和 EAP 要求
description: 本主题提供有关使用证书网络策略 Server 和 Windows Server 2016 中远程访问的信息。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e597a65982aeead907c9a41f17f1785a0bf81016
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>配置证书模板 PEAP 和 EAP 要求

>适用于：Windows Server（半年通道），Windows Server 2016

所有用于与可扩展身份验证 Protocol\ 传输层安全性 \(EAP\-TLS\)、保护可扩展身份验证 Protocol\ 传输层安全性 \(PEAP\-TLS\) 和 PEAP\ Microsoft 质询握手身份验证协议版本 2 网络的访问权限身份验证的证书 \ X.509 证书的要求，因此使用安全套接字层月传输级别安全 (SSL 月 TLS) 的连接的工作，必须满足 (MS\ CHAP v2\)。 客户端和服务器证书有其他要求。

>[!IMPORTANT]
>本主题提供配置证书模板说明进行的操作。 若要使用以下说明进行操作，则需要部署了你自己公钥基础结构 \(PKI\) 使用 Active Directory 证书服务 \(AD CS\)。

## <a name="minimum-server-certificate-requirements"></a>最低服务器证书要求

在 PEAP\ MS\ CHAP v2、PEAP\ TLS 或 EAP\-TLS 作为身份验证方法、NPS 服务器必须使用证书满足最低服务器服务器证书。 

可以配置客户端计算机以通过验证的服务器证书**验证服务器证书**选项的客户端计算机上或在组策略中。 

客户端计算机接受服务器的身份验证尝试当服务器证书满足以下要求：

- 主题名称中包含的值。 如果您运行的网络策略 Server (NPS) 具有空白的对象名称服务器到发出证书，不是可供你 NPS 服务器身份验证证书。 配置使用者名称证书模板：

    1. 打开证书模板。
    2. 在详细信息窗格中，右键单击你想要更改，然后单击证书模板**属性**。
    3. 单击**主题名称**选项卡，然后单击**版本从此 Active Directory 信息**。
    4. 在**主题文件名格式**，选择一个值的之外**无**。

- 计算机上的受信任的根证书颁发机构（加拿大）而不会不任何由 CryptoAPI 和检查指定远程访问策略或网络策略的失败为服务器链证书。

- 在外延密钥使用量 (EKU) 扩展的服务器身份验证目的配置 NPS 服务器或 VPN 服务器的计算机证书。 （为服务器身份验证的对象标识符是 1.3.6.1.5.5.7.3.1。）

- 所需的算法值为配置服务器证书**RSA**。 若要配置需的加密设置：

    1. 打开证书模板。
    2. 在详细信息窗格中，右键单击你想要更改，然后单击证书模板**属性**。
    3. 单击**加密**选项卡。在**算法名称**，单击**RSA**。 确保**至少密钥大小**设置为**2048 年**。

- 如果使用，主题替代 (SubjectAltName) 扩展名，必须包含 DNS 服务器的名称。 若要配置注册服务器了域名系统 (DNS) 同名证书模板： 

    1. 打开证书模板。
    2. 在详细信息窗格中，右键单击你想要更改，然后单击证书模板**属性**。
    3. 单击**主题名称**选项卡，然后单击**版本从此 Active Directory 信息**。
    4. 在**替换主题名称中包含以下信息**、选择**DNS 名称**。

使用 PEAP 和 EAP-TLS 时，NPS 服务器在计算机证书商店中，有以下例外显示已安装的所有证书的列表：

- 未包含在 EKU 扩展服务器身份验证目的证书不会显示。

- 未包含者名称证书不会显示。

- 基于注册表和不显示智能卡登录的证书。

有关详细信息，请参阅[802.1 X 有线和无线部署部署服务器证书](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)。

## <a name="minimum-client-certificate-requirements"></a>最小的客户端证书要求

使用 EAP-TLS 或 PEAP-TLS，服务器接受客户端身份验证尝试当证书满足以下要求：

- 客户端证书颁发的企业 CA 或映射到用户或计算机中 Active Directory 域服务 \(AD DS\) 帐户。

- 用户或计算机上的证书，对受信任的根 CA 的客户端链包含 EKU 扩展的客户端身份验证目的 \（客户端身份验证的对象标识符是 1.3.6.1.5.5.7.3.2\），并检查执行 CryptoAPI 和中远程访问策略或网络策略指定也 NPS 网络策略中指定的证书对象标识符检查不会失败。

- 不使用 802.1 X 的客户端，注册表基于证书智能卡登录的或受密码保护的证书。

- 对于用户证书证书中的主题备用名称 \(SubjectAltName\) 扩展包含用户主要命名 \(UPN\)。 若要配置证书模板 UPN:

    1. 打开证书模板。
    2. 在详细信息窗格中，右键单击你想要更改，然后单击证书模板**属性**。
    3. 单击**主题名称**选项卡，然后单击**版本从此 Active Directory 信息**。
    4. 在**替换主题名称中包含以下信息**、选择**用户主要命名 \(UPN\)**。

- 对于计算机证书证书中的主题备用名称 \(SubjectAltName\) 扩展必须包含完整的域命名的客户端，也称为 \(FQDN\) *DNS 名称*。 若要配置证书模板该名称：

    1. 打开证书模板。
    2. 在详细信息窗格中，右键单击你想要更改，然后单击证书模板**属性**。
    3. 单击**主题名称**选项卡，然后单击**版本从此 Active Directory 信息**。
    4. 在**替换主题名称中包含以下信息**、选择**DNS 名称**。

PEAP\ TLS 和 EAP\ TLS 的客户端中的证书贴靠中，有以下例外显示所有已安装的证书列表：

- 无线客户端不会显示注册表基于和智能卡登录的证书。 

- 无线客户端和 VPN 客户端不显示受密码保护的证书。 

- 未包含在 EKU 扩展的客户端身份验证目的证书不会显示。


有关 NPS 的详细信息，请参阅[网络策略 Server (NPS)](nps-top.md)。

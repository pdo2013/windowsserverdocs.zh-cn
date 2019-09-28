---
title: 配置用于 PEAP 和 EAP 要求的证书模板
description: 本主题提供有关在 Windows Server 2016 中的网络策略服务器和远程访问中使用证书的信息。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f60e5a1da1388a6dd2432a3603f83d6ca2990a29
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405407"
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>配置用于 PEAP 和 EAP 要求的证书模板

>适用于：Windows Server（半年频道）、Windows Server 2016

用于网络访问身份验证的所有证书，具有可扩展的身份验证协议 @ no__t-0Transport 层安全 \(EAP @ no__t-2TLS @ no__t，受保护的可扩展身份验证协议 @ no__t-4Transport 层安全性\(PEAP @ no__t-6TLS @ no__t-7，并且 PEAP @ no__t-8Microsoft 质询握手身份验证协议版本 2 \(MS @ no__t-10CHAP v2 @ no__t-11 必须满足 x.509 证书的要求，以及使用安全套接字的连接层/传输层安全性（SSL/TLS）。 客户端和服务器证书都具有其他要求。

>[!IMPORTANT]
>本主题提供有关配置证书模板的说明。 若要使用这些说明，需要使用 Active Directory 证书服务 \(AD CS @ no__t 部署自己的公钥基础结构 \(PKI @ no__t-1。

## <a name="minimum-server-certificate-requirements"></a>最小服务器证书要求

使用 PEAP @ no__t-0MS @ no__t-1CHAP v2、PEAP @ no__t-2TLS 或 EAP @ no__t-3TLS 作为身份验证方法时，NPS 必须使用满足最低服务器证书要求的服务器证书。 

通过在客户端计算机上或组策略中使用 "**验证服务器证书**" 选项，可以将客户端计算机配置为验证服务器证书。 

当服务器证书满足以下要求时，客户端计算机接受服务器的身份验证尝试：

- 使用者名称包含一个值。 如果将证书颁发给运行网络策略服务器（NPS）的服务器，该服务器的使用者名称为空，则该证书将无法用于对 NPS 进行身份验证。 使用使用者名称配置证书模板：

    1. 打开“证书模板”。
    2. 在详细信息窗格中，右键单击要更改的证书模板，然后单击 "**属性**"。
    3. 单击 "**使用者名称**" 选项卡，然后单击 "从此**Active Directory 信息生成**"。
    4. 在 "**使用者名称格式**" 中，选择 "**无**" 以外的值。

- 服务器上的计算机证书链接到受信任的根证书颁发机构（CA），并且不会失败由 CryptoAPI 执行并在远程访问策略或网络策略中指定的所有检查。

- NPS 或 VPN 服务器的计算机证书在扩展密钥用法（EKU）扩展中配置了服务器身份验证目的。 （服务器身份验证的对象标识符为1.3.6.1.5.5.7.3.1。）

- 用所需的加密设置配置服务器证书：

    1. 打开“证书模板”。
    2. 在详细信息窗格中，右键单击要更改的证书模板，然后单击 "**属性**"。
    3. 单击 "**加密**" 选项卡，并确保配置以下各项：
       - **提供程序类别：** 密钥存储提供程序
       - **算法名称：** RSA
       - **接口**Microsoft 平台加密提供程序
       - **最小密钥大小：** 2048
       - **哈希算法：** SHA2
    4. 单击“下一步”。

- 使用者备用名称（SubjectAltName）扩展（如果使用）必须包含服务器的 DNS 名称。 用注册服务器的域名系统（DNS）名称配置证书模板的步骤： 

    1. 打开“证书模板”。
    2. 在详细信息窗格中，右键单击要更改的证书模板，然后单击 "**属性**"。
    3. 单击 "**使用者名称**" 选项卡，然后单击 "从此**Active Directory 信息生成**"。
    4. 在 "将**此信息包含在备用使用者名称**" 中，选择 " **DNS 名称**"。

使用 PEAP 和 EAP-TLS 时，NPSs 将在计算机证书存储中显示所有已安装证书的列表，但以下情况例外：

- 不会显示不包含 EKU 扩展中的服务器身份验证目的的证书。

- 不显示不包含使用者名称的证书。

- 不显示基于注册表的登录证书和智能卡登录证书。

有关详细信息，请参阅为[802.1 x 有线和无线部署部署服务器证书](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)。

## <a name="minimum-client-certificate-requirements"></a>最低客户端证书要求

对于 EAP-TLS 或 PEAP-TLS，当证书符合以下要求时，服务器将接受客户端身份验证尝试：

- 客户端证书由企业 CA 颁发，或者映射到 Active Directory 域服务 \(AD DS @ no__t-1 中的用户或计算机帐户。

- 客户端上的用户或计算机证书链接到受信任的根 CA，包括 EKU 扩展中的客户端身份验证目的 @no__t-客户端身份验证的0the 对象标识符为 1.3.6.1.5.5.7.3.2 @ no__t-1，并且不是由 CryptoAPI 执行并在远程访问策略或网络策略中指定，或者在 NPS 网络策略中指定的证书对象标识符检查中进行。

- 802.1 X 客户端不使用智能卡登录或受密码保护的证书的基于注册表的证书。

- 对于用户证书，证书中的使用者可选名称 \(SubjectAltName @ no__t; 包含用户主体名称 \(UPN @ no__t。 在证书模板中配置 UPN：

    1. 打开“证书模板”。
    2. 在详细信息窗格中，右键单击要更改的证书模板，然后单击 "**属性**"。
    3. 单击 "**使用者名称**" 选项卡，然后单击 "从此**Active Directory 信息生成**"。
    4. 在 "将**此信息包含在备用使用者名称**中" 中，选择 "**用户主体名称 \(UPN @ no__t**"。

- 对于计算机证书，证书中的使用者可选名称 \(SubjectAltName @ no__t 扩展必须包含客户端的完全限定的域名 \(FQDN @ no__t-3 （也称为*DNS 名称*）。 在证书模板中配置此名称：

    1. 打开“证书模板”。
    2. 在详细信息窗格中，右键单击要更改的证书模板，然后单击 "**属性**"。
    3. 单击 "**使用者名称**" 选项卡，然后单击 "从此**Active Directory 信息生成**"。
    4. 在 "将**此信息包含在备用使用者名称**" 中，选择 " **DNS 名称**"。

使用 PEAP @ no__t-0TLS 和 EAP @ no__t-1TLS，客户端将在 "证书" 管理单元中显示所有已安装证书的列表，但以下情况例外：

- 无线客户端不显示基于注册表的证书和智能卡登录证书。 

- 无线客户端和 VPN 客户端不显示密码保护的证书。 

- 不会显示不包含 EKU 扩展中的客户端身份验证用途的证书。


有关 NPS 的详细信息，请参阅[网络策略服务器（NPS）](nps-top.md)。

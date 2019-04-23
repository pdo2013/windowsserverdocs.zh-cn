---
title: 配置用于 PEAP 和 EAP 要求的证书模板
description: 本主题提供有关使用网络策略服务器和远程访问 Windows Server 2016 中使用证书的信息。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 41f6f88705fa3d58be695fd825d960e7df21cd24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885878"
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>配置用于 PEAP 和 EAP 要求的证书模板

>适用于：Windows 服务器 （半年频道），Windows Server 2016

用于使用可扩展身份验证协议的网络访问身份验证的所有证书\-传输层安全性\(EAP\-TLS\)，受保护的可扩展身份验证协议\-传输层安全性\(PEAP\-TLS\)，和 PEAP\-Microsoft 质询握手身份验证协议版本 2 \(MS\-CHAP v2\)必须满足 X.509 证书的要求，适用于使用安全套接字层/传输级别安全性 (SSL/TLS) 的连接。 客户端和服务器证书具有附加要求。

>[!IMPORTANT]
>本主题提供有关配置证书模板的说明。 若要使用这些说明，则需要部署自己的公共密钥基础结构\(PKI\)与 Active Directory 证书服务\(AD CS\)。

## <a name="minimum-server-certificate-requirements"></a>最小服务器证书要求

与 PEAP\-MS\-CHAP v2，PEAP\-TLS 或 EAP\-作为身份验证方法使用 TLS，NPS 必须使用满足最小服务器证书要求的服务器证书。 

可以将客户端计算机配置为使用验证服务器证书**验证服务器证书**客户端计算机上或在组策略中的选项。 

当服务器证书满足以下要求时，客户端计算机接受服务器的身份验证尝试：

- 使用者名称包含的值。 如果将证书颁发到运行网络策略服务器 (NPS) 具有空白使用者名称的服务器，该证书不可用进行身份验证您的 NPS。 若要使用者名称配置证书模板：

    1. 打开“证书模板”。
    2. 在细节窗格中，右键单击你想要更改，然后单击该证书模板**属性**。
    3. 单击**使用者名称**选项卡，然后依次**中的 Active Directory 信息生成**。
    4. 在中**使用者名称格式**，选择一个值不是**None**。

- 在服务器链接到受信任的根证书颁发机构 (CA) 而不会任何的失败由 CryptoAPI，执行检查远程访问策略或网络策略中指定计算机证书。

- 扩展密钥用法 (EKU) 扩展中的服务器身份验证用途配置 NPS 或 VPN 服务器的计算机证书。 （服务器身份验证的对象标识符为 1.3.6.1.5.5.7.3.1。）

- 使用所需的加密设置配置服务器证书：

    1. 打开“证书模板”。
    2. 在细节窗格中，右键单击你想要更改，然后单击该证书模板**属性**。
    3. 单击**加密**选项卡上，请确保配置以下各项：
       - **提供程序类别：** 密钥存储提供程序
       - **算法名称：** RSA
       - **提供程序：** Microsoft 平台加密提供程序
       - **最小密钥大小：** 2048
       - **哈希算法：** SHA2
    4. 单击“下一步” 。

- 使用者备用名称 (SubjectAltName) 扩展中，如果使用，必须包含服务器的 DNS 名称。 若要注册的服务器的域名系统 (DNS) 名称配置证书模板： 

    1. 打开“证书模板”。
    2. 在细节窗格中，右键单击你想要更改，然后单击该证书模板**属性**。
    3. 单击**使用者名称**选项卡，然后依次**中的 Active Directory 信息生成**。
    4. 在中**另一个使用者名称中包含此信息**，选择**DNS 名称**。

使用 PEAP 和 EAP-TLS 时，NPSs 将显示在计算机证书存储中，有以下例外的所有已安装证书的列表：

- 不显示不包含 EKU 扩展中的服务器身份验证用途的证书。

- 不显示不包含使用者名称的证书。

- 基于注册表的和不显示智能卡登录证书。

有关详细信息，请参阅[802.1x 有线和无线部署部署服务器证书](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)。

## <a name="minimum-client-certificate-requirements"></a>最低客户端证书要求

使用 EAP-TLS 或 PEAP-TLS，则服务器将接受客户端身份验证尝试时的证书符合以下要求：

- 由企业 CA 颁发或映射到 Active Directory 域服务中的用户或计算机帐户的客户端证书\(AD DS\)。

- 用户或计算机上的证书客户端链接到受信任的根 CA，包括 EKU 扩展中的客户端身份验证用途\(客户端身份验证的对象标识符为 1.3.6.1.5.5.7.3.2\)，并将故障都不检查由 CryptoAPI 执行和远程访问策略或网络策略也不能在 NPS 网络策略中指定的证书对象标识符检查中指定。

- 802.1 X 客户端不使用基于注册表的证书的智能卡登录或密码保护证书。

- 对于用户证书，使用者可选名称\(SubjectAltName\)扩展的证书中包含的用户主体名称\(UPN\)。 若要配置证书模板中的 UPN:

    1. 打开“证书模板”。
    2. 在细节窗格中，右键单击你想要更改，然后单击该证书模板**属性**。
    3. 单击**使用者名称**选项卡，然后依次**中的 Active Directory 信息生成**。
    4. 在中**另一个使用者名称中包含此信息**，选择**用户主体名称\(UPN\)**。

- 对于计算机证书，使用者可选名称\(SubjectAltName\)证书中的扩展必须包含完全限定的域名\(FQDN\)的客户端，这也称为*DNS 名称*。 若要配置证书模板中的此名称：

    1. 打开“证书模板”。
    2. 在细节窗格中，右键单击你想要更改，然后单击该证书模板**属性**。
    3. 单击**使用者名称**选项卡，然后依次**中的 Active Directory 信息生成**。
    4. 在中**另一个使用者名称中包含此信息**，选择**DNS 名称**。

与 PEAP\-TLS 和 EAP\-TLS，客户端的所有已安装的证书列表中显示证书管理单元中，有以下例外：

- 无线客户端不会显示基于注册表的证书和智能卡登录证书。 

- 无线客户端和 VPN 客户端不显示受密码保护证书。 

- 不显示不包含 EKU 扩展中的客户端身份验证用途的证书。


有关 NPS 的详细信息，请参阅[网络策略服务器 (NPS)](nps-top.md)。

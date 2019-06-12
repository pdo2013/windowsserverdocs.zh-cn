---
title: 什么是 Kerberos 身份验证中的新增功能
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 90107bd49268f232fd6d532c304c2fdd050bcbf5
ms.sourcegitcommit: c6acac3622e5d34714ca5c569805931681f98779
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391497"
---
# <a name="whats-new-in-kerberos-authentication"></a>What's New in Kerberos Authentication

>适用于：Windows Server 2016 和 Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>KDC 支持基于公共密钥信任的客户端身份验证

从 Windows Server 2016 开始，Kdc 支持一种公共密钥映射方法。 如果帐户预配的公共密钥后，KDC 支持 Kerberos PKInit 显式使用该密钥。 由于没有任何证书验证，支持自签名的证书和不支持身份验证机制保证。

密钥信任是首选配置而不考虑 UseSubjectAltName 设置帐户。

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>Kerberos 客户端和 KDC 都支持用于 RFC 8070 PKInit 新鲜度扩展插件

从 Windows 10，版本 1607年和 Windows Server 2016 开始，Kerberos 客户端尝试[RFC 8070 PKInit 新鲜度扩展](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/)对于公钥基于登录。 

从 Windows Server 2016 开始，Kdc 可支持 PKInit 新鲜度扩展。 默认情况下，Kdc 不提供 PKInit 新鲜度扩展。 若要启用它，请使用 PKInit 新鲜度扩展 KDC 管理模板策略设置在域中的所有域控制器上的新的 KDC 支持。 配置时，Windows Server 2016 域功能级别 (DFL) 域时，支持以下选项：

- **已禁用**：KDC 永远不会提供 PKInit 新鲜度扩展，并接受而不进行新鲜度检查有效的身份验证请求。 用户将永远不会收到的全新公共密钥标识的 SID。
- **支持**:请求支持 PKInit 新鲜度扩展。 使用 PKInit 新鲜度扩展已成功进行身份验证的 Kerberos 客户端接收的全新公共密钥标识的 SID。
- **必需**：PKInit 新鲜度扩展是必需的身份验证成功。 使用公共密钥凭据时，不支持 PKInit 新鲜度扩展的 Kerberos 客户端将始终失败。

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>使用公钥身份验证的已加入域的设备支持

如果已加入域的设备能够注册其绑定的公钥和 Windows Server 2016 的域控制器 (DC) 从 Windows 10 版本 1507年和 Windows Server 2016，然后设备使用进行身份验证使用 Kerberos 身份验证的公钥Windows Server 2016 DC。 有关详细信息，请参阅[已加入域的设备公共密钥身份验证](Domain-joined-Device-Public-Key-Authentication.md)

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Kerberos 客户端允许在服务主体名称 (Spn) 中使用 IPv4 和 IPv6 地址的主机名

从 Windows 10 版本 1507年和 Windows Server 2016 开始，可以对 Kerberos 客户端被配置为在 Spn 中支持 IPv4 和 IPv6 主机名。 

注册表路径：

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

若要在 Spn 中配置 IP 地址的主机名的支持，创建 TryIPSPN 条目。 默认情况下，注册表中不存在此项。 创建该项后，请将 DWORD 值更改为 1。 如果未配置，不会尝试 IP 地址的主机名。

如果在 Active Directory 中注册 SPN，身份验证成功使用 Kerberos。 

有关详细信息，请查看文档[的 IP 地址配置 Kerberos](configuring-kerberos-over-ip.md)。

## <a name="kdc-support-for-key-trust-account-mapping"></a>KDC 支持密钥信任帐户映射

从 Windows Server 2016 开始，域控制器都支持密钥信任帐户映射，以及回退到现有 AltSecID 和用户主体名称 (UPN) 中的 SAN 行为。 当 UseSubjectAltName 设置为：

- 0:显式映射是必需的。 然后必须有任何一个：
    - 密钥信任 （new 与 Windows Server 2016)
    - ExplicitAltSecID
- 1：隐式映射允许 （默认值）：
    1. 如果密钥信任帐户配置，则它用于映射 （新 Windows Server 2016）。
    2. 如果在 SAN 中没有 UPN，然后 AltSecID 尝试为映射。
    3. 如果在 SAN 中没有 UPN，然后映射尝试使用 UPN。

## <a name="see-also"></a>请参阅

- [Kerberos 身份验证概述](kerberos-authentication-overview.md)

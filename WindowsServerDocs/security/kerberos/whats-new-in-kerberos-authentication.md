---
title: "什么是验证 Kerberos 中的新增功能"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 7b046490c606cdf9e1436f503bf46a9cd4280ea9
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2018
---
# <a name="whats-new-in-kerberos-authentication"></a>什么是验证 Kerberos 中的新增功能

>适用于：Windows Server 2016 和 Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>KDC 支持基于公用键信任的客户端身份验证

开始与 Windows Server 2016，Kdc 支持公共关键映射的一种方法。 如果公钥已预配帐户，则 KDC 支持 Kerberos PKInit 明确使用该密钥。 由于未证书验证，支持自签名的证书，身份验证机制保证不受支持。

信任键时首选配置无论 UseSubjectAltName 设置帐户。

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>RFC 8070 PKInit 新鲜扩展 Kerberos 客户端和 KDC 支持

从 Windows 10 版本 1607 年和 Windows Server 2016 开始，Kerberos 客户端尝试[RFC 8070 PKInit 新鲜扩展](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/)对于公钥基于登录加载项。 

Windows Server 2016 的开始 Kdc 可支持 PKInit 新鲜扩展。 默认情况下，Kdc 不会提供 PKInit 新鲜扩展。 若要启用它，可用于在域中的所有 Dc PKInit 新鲜扩展 KDC 管理模板策略设置的新 KDC 支持。 配置时，Windows Server 2016 域功能级别 (DFL) 域时支持以下选项：

- **禁用**: KDC 永远不会提供 PKInit 新鲜扩展，而不新鲜检查接受有效的身份验证的请求。 用户永远不会将收到最新公开键标识 SID。
- **受支持**: PKInit 新鲜扩展支持请求。 成功进行身份验证的 PKInit 新鲜扩展的 Kerberos 客户端接收最新公开键标识 SID。
- **需要**: PKInit 新鲜扩展，才能进行成功进行身份验证。 使用公钥凭据时，不支持 PKInit 新鲜扩展的 Kerberos 客户端将始终失败。

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>已加入域的设备支持使用公钥进行身份验证

开始使用 Windows 10 版本 1507 年和 Windows Server 2016，，已加入域的设备是否可以将其绑定的公钥注册 Windows Server 2016 域控制器 (DC)，然后设备可以使用 Windows Server 2016 DC Kerberos 身份验证的公钥进行身份验证。 有关详细信息，请参阅[加入域的设备公共键身份验证](Domain-joined-Device-Public-Key-Authentication.md)

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Kerberos 客户端服务主要名称 (Spn) 中允许 IPv4 和 IPv6 地址主机

从 Windows 10 版本 1507 年和 Windows Server 2016 开始，Kerberos 客户端可以配置为在 Spn 支持 IPv4 和 IPv6 主机。 

注册表路径：

HKLM\SOFTWARE\ Microsoft \Windows\CurrentVersion\Policies\System\ Kerberos \Parameters

若要 Spn 在配置 IP 地址主机的支持，创建 TryIPSPN 条目。 此项中不存在注册表默认情况。 创建项后，请为 1 更改 DWORD 值。 如果没有配置，不会尝试 IP 地址主机。

如果在 Active Directory 中注册 SPN，身份验证成功与 Kerberos。 

## <a name="kdc-support-for-key-trust-account-mapping"></a>KDC 支持键信任帐户地图

域控制器与 Windows Server 2016 开始，具有支持为键信任帐户映射回退到现有 AltSecID 和名称 UPN) 中的 SAN 行为。 当设置为 UseSubjectAltName:

- 0：显式映射是必需的。 然后必须有任何一个：
    - 键信任（新与 Windows Server 2016)
    - ExplicitAltSecID
- 1：隐式映射允许（默认）：
    1. 如果密钥信任配置的帐户，然后它用于映射（Windows Server 2016 的新增）。
    2. 如果在 SAN 没有 UPN，映射为尝试 AltSecID。
    3. 如果在 SAN UPN，映射为尝试 UPN。

## <a name="see-also"></a>请参阅

- [Kerberos 身份验证概述](kerberos-authentication-overview.md)

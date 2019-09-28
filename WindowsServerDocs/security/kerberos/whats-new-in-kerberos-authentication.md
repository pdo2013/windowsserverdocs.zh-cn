---
title: What's New in Kerberos Authentication
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: a0916abf1076b5f791a856f0c85f54ad17f6d64c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403479"
---
# <a name="whats-new-in-kerberos-authentication"></a>What's New in Kerberos Authentication

>适用于：Windows Server 2016 和 Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>KDC 支持基于公钥信任的客户端身份验证

从 Windows Server 2016 开始，Kdc 支持一种公钥映射方式。 如果为帐户设置了公钥，则 KDC 将使用该密钥显式支持 Kerberos PKInit。 由于不存在证书验证，因此支持自签名证书，并且不支持身份验证机制保障。

为帐户配置时，无论 UseSubjectAltName 设置如何，都首选密钥信任。

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>适用于 RFC 8070 PKInit 新鲜度 Extension 的 Kerberos 客户端和 KDC 支持

从 Windows 10 版本1607和 Windows Server 2016 开始，Kerberos 客户端尝试将[RFC 8070 PKInit freshness extension](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/)用于公钥的登录。 

从 Windows Server 2016 开始，Kdc 可以支持 PKInit 新鲜度扩展。 默认情况下，Kdc 不提供 PKInit 新鲜度扩展。 若要启用它，请对域中的所有 Dc 使用 "新 KDC 支持 PKInit 新鲜度 Extension KDC" 管理模板策略设置。 配置后，如果域为 Windows Server 2016 域功能级别（DFL），则支持以下选项：

- **已禁用**：KDC 绝不会提供 PKInit 新鲜度 Extension 并接受有效的身份验证请求，而不检查新鲜度。 用户永远不会收到新的公钥标识 SID。
- **支持**：请求支持 PKInit 新鲜度扩展。 Kerberos 客户端成功地通过 PKInit 新鲜度扩展进行身份验证时，会收到新的公钥标识 SID。
- **必需**：身份验证成功需要 PKInit 新鲜度扩展。 使用公钥凭据时，不支持 PKInit 新鲜度扩展的 Kerberos 客户端将始终失败。

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>已加入域的设备支持使用公钥进行身份验证

从 Windows 10 版本1507和 Windows Server 2016 开始，如果已加入域的设备能够向 Windows Server 2016 域控制器（DC）注册其绑定的公钥，则设备可以通过使用 Kerberos 身份验证的公钥进行身份验证Windows Server 2016 DC。 有关详细信息，请参阅已[加入域的设备公钥身份验证](Domain-joined-Device-Public-Key-Authentication.md)

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Kerberos 客户端允许 IPv4 和 IPv6 地址主机名位于服务主体名称（Spn）中

从 Windows 10 版本1507和 Windows Server 2016 开始，Kerberos 客户端可以配置为在 Spn 中支持 IPv4 和 IPv6 主机名。 

注册表路径：

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

若要配置对 Spn 中 IP 地址主机名的支持，请创建 TryIPSPN 条目。 默认情况下，注册表中不存在此项。 创建项后，将 DWORD 值更改为1。 如果未配置，则不尝试 IP 地址主机名。

如果在 Active Directory 中注册了 SPN，则身份验证将成功并带有 Kerberos。 

有关详细信息，请参阅[配置 IP 地址 Kerberos 的](configuring-kerberos-over-ip.md)文档。

## <a name="kdc-support-for-key-trust-account-mapping"></a>KDC 支持密钥信任帐户映射

从 Windows Server 2016 开始，域控制器支持密钥信任帐户映射，并回退到 SAN 行为中的现有 AltSecID 和用户主体名称（UPN）。 当 UseSubjectAltName 设置为时：

- 0需要显式映射。 然后必须存在以下两种情况之一：
    - 密钥信任（Windows Server 2016 中的新）
    - ExplicitAltSecID
- 1：允许隐式映射（默认值）：
    1. 如果为帐户配置了密钥信任，则将它用于映射（使用 Windows Server 2016 的新）。
    2. 如果 SAN 中没有 UPN，则尝试映射 AltSecID。
    3. 如果 SAN 中有 UPN，则会尝试 UPN 进行映射。

## <a name="see-also"></a>请参阅

- [Kerberos 身份验证概述](kerberos-authentication-overview.md)

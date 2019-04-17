---
title: "已加入域的设备公共身份验证"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: michikos
ms.technology: security-authentication
ms.date: 8/18/2017
ms.openlocfilehash: 65f39f4900fcd99ef90b160d3db7099edb73bc1c
ms.sourcegitcommit: e94838df702ff0135ebe7c179b228aa67b772d35
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2017
---
# <a name="domain-joined-device-public-key-authentication"></a>已加入域的设备公共身份验证

>适用于： Windows Server 2016，Windows 10

Kerberos 添加了支持加入域的设备，以与 Windows Server 2012 和 Windows 8 Windows 8. 此更改允许第三方提供商，自己创建配置并初始化加入域的设备，以用于域身份验证的证书的解决方案。 

## <a name="automatic-public-key-provisioning"></a>自动公共关键预配

已加入域的设备从 Windows 10 版本 1507 年和 Windows Server 2016 开始，自动配置绑定的公钥和 Windows Server 2016 域控制器 (DC)。 一旦键已预配到域，Windows 可以使用公钥身份验证。

### <a name="public-key-generation"></a>公开密钥生成
如果设备运行的 Credential Guard，然后创建公钥受 Credential Guard。 

如果 Credential Guard 不可用，并且 TPM，然后创建公钥由 TPM 受保护。 

两者皆不可用，如果某个键则不会生成，并设备仅验证使用密码。

### <a name="provisioning-computer-account-public-key"></a>配置计算机帐户公钥
Windows 启动时，它会检查公钥是否已为其计算机帐户预配。 如果没有，然后生成一个绑定的公钥并配置为其帐户中使用的 Windows Server 2016 或更高版本 DC 的广告。 如果所有 Dc 低级别，没有密钥将预配。

### <a name="configuring-device-to-only-use-public-key"></a>设备仅使用公钥配置
如果组策略设置**设备使用的身份验证证书的支持**设置为**强制**，然后设备需要运行 Windows Server 2016 的 dc 或更高版本进行身份验证。 该设置已在管理模板下 > 系统 > Kerberos。

### <a name="configuring-device-to-only-use-password"></a>配置设备以仅使用密码
如果组策略设置**设备使用的身份验证证书的支持**已禁用，然后始终使用密码。 该设置已在管理模板下 > 系统 > Kerberos。

## <a name="domain-joined-device-authentication-using-public-key"></a>已加入域的设备使用公钥的身份验证
当 Windows 已加入域的设备的证书时，Kerberos 首先验证使用该证书和上使用密码失败重试。 这将允许在设备向低级别 Dc 验证身份。

由于自动配置的公钥自签名的证书，则证书验证失败不支持键信任帐户映射的域控制器上。 默认情况下，Windows 会身份验证使用设备的域密码重试。



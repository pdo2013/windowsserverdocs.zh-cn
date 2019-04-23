---
title: 已加入域的设备公钥身份验证
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: michikos
ms.technology: security-authentication
ms.date: 08/18/2017
ms.openlocfilehash: 80906e7cfe3740200938704a4b4eaf0759af303a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885028"
---
# <a name="domain-joined-device-public-key-authentication"></a>已加入域的设备公钥身份验证

>适用于：Windows Server 2016 中，Windows 10

Kerberos 添加对已加入域的设备登录证书开始使用 Windows Server 2012 和 Windows 8 的支持。 此更改允许第三方供应商创建的解决方案来预配并初始化已加入域的设备使用进行域身份验证的证书。 

## <a name="automatic-public-key-provisioning"></a>自动公共密钥预配

从 Windows 10 版本 1507年和 Windows Server 2016 开始，已加入域的设备自动预配到 Windows Server 2016 域控制器 (DC) 的绑定公共密钥。 密钥预配，然后到域，Windows 可以使用公钥身份验证。

### <a name="public-key-generation"></a>公共密钥生成
如果设备正在运行 Credential Guard，然后创建一个公钥保护下 Credential Guard。 

如果 Credential Guard 不可用并且 TPM，然后创建一个公钥由 TPM 受保护。 

如果这都可用，然后不生成的密钥和设备可以仅使用进行身份验证密码。

### <a name="provisioning-computer-account-public-key"></a>预配计算机帐户公钥
当 Windows 启动时，它会检查其计算机帐户是否预配的公共密钥。 如果没有，然后生成绑定的公共密钥，并在 AD 中使用 Windows Server 2016 或更高版本的 DC 将其配置为其帐户。 如果所有域控制器是低级别，然后预不配任何密钥。

### <a name="configuring-device-to-only-use-public-key"></a>配置设备以只使用公钥
如果组策略设置**对使用证书进行设备身份验证的支持**设置为**强制**，则设备必须找到 DC 在运行 Windows Server 2016 或更高版本进行身份验证。 在管理模板下的设置是 > 系统 > Kerberos。

### <a name="configuring-device-to-only-use-password"></a>配置设备以只使用密码
如果组策略设置**对使用证书进行设备身份验证的支持**已禁用，则始终使用密码。 在管理模板下的设置是 > 系统 > Kerberos。

## <a name="domain-joined-device-authentication-using-public-key"></a>使用公钥的已加入域的设备身份验证
当 Windows 已加入域的设备的证书时，Kerberos 首先进行身份验证使用的证书和使用密码失败重试。 这允许设备向下级 Dc 进行身份验证。

由于自动预配的公共密钥具有自签名的证书，证书验证失败不支持密钥信任帐户映射的域控制器上。 默认情况下，Windows 会使用设备的域密码的身份验证重试。



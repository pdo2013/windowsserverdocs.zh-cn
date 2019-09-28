---
title: 已加入域的设备公钥身份验证
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: michikos
ms.technology: security-authentication
ms.date: 08/18/2017
ms.openlocfilehash: 616ebf1a8e01f84618d22d535609a0dc8414d718
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403500"
---
# <a name="domain-joined-device-public-key-authentication"></a>已加入域的设备公钥身份验证

>适用于：Windows Server 2016、Windows 10

Kerberos 添加了对加入域的设备的支持，以便使用从 Windows Server 2012 和 Windows 8 开始的证书登录。 此更改允许第三方供应商创建解决方案来为已加入域的设备设置和初始化证书，以用于域身份验证。 

## <a name="automatic-public-key-provisioning"></a>自动公钥预配

从 Windows 10 版本1507和 Windows Server 2016 开始，已加入域的设备会自动将绑定公钥预配到 Windows Server 2016 域控制器（DC）。 设置密钥后，Windows 可以使用域的公钥身份验证。

### <a name="public-key-generation"></a>公钥生成
如果设备正在运行 Credential Guard，则会通过 Credential Guard 来保护公钥。 

如果 Credential Guard 不可用，且 TPM 为，则会创建受 TPM 保护的公钥。 

如果两者都不可用，则不会生成密钥，并且设备只能使用密码进行身份验证。

### <a name="provisioning-computer-account-public-key"></a>预配计算机帐户公钥
当 Windows 启动时，它会检查是否为其计算机帐户设置了公钥。 如果不是，则它将生成绑定的公钥，并使用 Windows Server 2016 或更高版本的 DC 为 AD 中的帐户配置该公钥。 如果所有 Dc 都已关闭，则不会设置任何密钥。

### <a name="configuring-device-to-only-use-public-key"></a>将设备配置为仅使用公钥
如果将 "**使用证书进行设备身份验证**" 的 "组策略设置" 设置为 "**强制**"，则设备需要查找运行 Windows Server 2016 或更高版本的 DC 进行身份验证。 该设置位于 "管理模板 > System > Kerberos" 下。

### <a name="configuring-device-to-only-use-password"></a>将设备配置为仅使用密码
如果禁用**使用证书对设备身份验证**的组策略设置，则始终使用 password。 该设置位于 "管理模板 > System > Kerberos" 下。

## <a name="domain-joined-device-authentication-using-public-key"></a>使用公钥加入域的设备身份验证
当 Windows 具有已加入域的设备的证书时，Kerberos 首先使用证书进行身份验证，并在发生故障后使用密码进行身份验证。 这允许设备向下级 Dc 进行身份验证。

由于自动设置的公钥具有自签名证书，因此，不支持密钥信任帐户映射的域控制器上的证书验证失败。 默认情况下，Windows 将使用设备的域密码重试身份验证。



---
title: 配置 DNS 转发和域信任
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: ebc9c2a3cac85ab998075d784111808b3d590d46
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854138"
---
# <a name="configure-dns-forwarding-in-the-hgs-domain-and-a-one-way-trust-with-the-fabric-domain"></a>HGS 域和单向信任关系的 fabric 域中配置 DNS 转发

>适用于：Windows 服务器 （半年频道），Windows Server 2016

>[!IMPORTANT]
>AD 模式与 Windows Server 2019 从开始已弃用。 对于 TPM 证明不可能的环境，配置[托管密钥证明](guarded-fabric-initialize-hgs-key-mode.md)。 主机密钥证明提供了类似保障为 AD 模式，并且易于设置。 

使用以下步骤来设置 DNS 转发和建立与 fabric 域之间的单向信任。 这些步骤允许域控制器的 HGS 来查找在构造，并验证组成员身份的 HYPER-V 主机。

1.  在配置 DNS 转发权限提升的 PowerShell 会话中运行以下命令。 替换构造域的名称为 fabrikam.com 和键入 fabric 域中的 DNS 服务器的 IP 地址。 为了提高可用性，指向多个 DNS 服务器。

    ```powershell
    Add-DnsServerConditionalForwarderZone -Name "fabrikam.com" -ReplicationScope "Forest" -MasterServers <DNSserverAddress1>, <DNSserverAddress2>
    ```

2.  若要创建单向林信任，请在提升的命令提示符中运行以下命令：

    替换`bastion.local`HGS 域的名称和`fabrikam.com`fabric 域的名称。 提供的 fabric 域管理员的密码。

        netdom trust bastion.local /domain:fabrikam.com /userD:fabrikam.com\Administrator /passwordD:<password> /add

## <a name="next-step"></a>下一步 

>[!div class="nextstepaction"]
[配置 HTTPS](guarded-fabric-configure-hgs-https.md)

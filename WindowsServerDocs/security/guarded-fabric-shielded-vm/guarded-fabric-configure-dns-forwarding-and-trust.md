---
title: 配置 DNS 转发和域信任
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 5d8ffe82065caeee27c5d13f5243f13addc6c325
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386736"
---
# <a name="configure-dns-forwarding-in-the-hgs-domain-and-a-one-way-trust-with-the-fabric-domain"></a>使用 fabric 域在 HGS 域和单向信任中配置 DNS 转发

>适用于：Windows Server（半年频道）、Windows Server 2016

>[!IMPORTANT]
>从 Windows Server 2019 开始，AD 模式已弃用。 对于不可能进行 TPM 证明的环境，请配置[主机密钥证明](guarded-fabric-initialize-hgs-key-mode.md)。 主机密钥证明向 AD 模式提供类似的保障，并更易于设置。 

使用以下步骤设置 DNS 转发并与 fabric 域建立单向信任。 这些步骤允许 HGS 查找构造域控制器并验证 Hyper-v 主机的组成员身份。

1.  在已提升权限的 PowerShell 会话中运行以下命令，配置 DNS 转发。 将 fabrikam.com 替换为 fabric 域的名称，并键入 fabric 域中 DNS 服务器的 IP 地址。 若要获得更高的可用性，请指向多个 DNS 服务器。

    ```powershell
    Add-DnsServerConditionalForwarderZone -Name "fabrikam.com" -ReplicationScope "Forest" -MasterServers <DNSserverAddress1>, <DNSserverAddress2>
    ```

2.  若要创建单向林信任，请在提升的命令提示符下运行以下命令：

    将 `bastion.local` 替换为 HGS 域的名称，并将 `fabrikam.com` 替换为构造域的名称。 为 fabric 域的管理员提供密码。

        netdom trust bastion.local /domain:fabrikam.com /userD:fabrikam.com\Administrator /passwordD:<password> /add

## <a name="next-step"></a>下一步 

> [!div class="nextstepaction"]
> [配置 HTTPS](guarded-fabric-configure-hgs-https.md)

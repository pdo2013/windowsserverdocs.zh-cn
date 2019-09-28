---
title: 为受保护的主机创建安全组，并向 HGS 注册组
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 1a36cfa10cb16033f5ca92b7e408132e38f5989c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386847"
---
# <a name="create-a-security-group-for-guarded-hosts-and-register-the-group-with-hgs"></a>为受保护的主机创建安全组，并向 HGS 注册组

>适用于：Windows Server（半年频道）、Windows Server 2016

>[!IMPORTANT]
>从 Windows Server 2019 开始，AD 模式已弃用。 对于不可能进行 TPM 证明的环境，请配置[主机密钥证明](guarded-fabric-initialize-hgs-key-mode.md)。 主机密钥证明向 AD 模式提供类似的保障，并更易于设置。 


本主题介绍了使用管理受信任的证明（AD 模式）准备 Hyper-v 主机成为受保护主机的中间步骤。 执行这些步骤之前，请完成[为将成为受保护主机的主机配置构造 DNS](guarded-fabric-configuring-fabric-dns-ad.md)中的步骤。


## <a name="create-a-security-group-and-add-hosts"></a>创建安全组并添加主机

1. 在 fabric 域中创建新的**全局**安全组，并添加将运行受防护的 Vm 的 hyper-v 主机。 重新启动主机以更新其组成员身份。

2. 使用 New-adgroup 获取安全组的安全标识符（SID），并将其提供给 HGS 管理员。 

    ```powershell
    Get-ADGroup "Guarded Hosts"
    ```

    ![带有输出的 New-adgroup 命令](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>向 HGS 注册安全组的 SID  

1. 在 HGS 服务器上，运行以下命令以向 HGS 注册安全组。 
   如果需要，请重新运行其他组的命令。 
   提供组的友好名称。 
   它不需要与 Active Directory 安全组名称匹配。 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. 若要验证是否已添加组，请运行[HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx)。 

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [确认证明](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>请参阅

- [为受保护的主机和受防护的 Vm 部署主机保护者服务](guarded-fabric-deploying-hgs-overview.md)

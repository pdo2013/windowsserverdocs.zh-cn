---
title: 为受保护的主机创建安全组并向 HGS 注册组
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: ba547dff862a283b68ff105a14b14ed367891745
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816338"
---
# <a name="create-a-security-group-for-guarded-hosts-and-register-the-group-with-hgs"></a>为受保护的主机创建安全组并向 HGS 注册组

>适用于：Windows 服务器 （半年频道），Windows Server 2016

>[!IMPORTANT]
>AD 模式与 Windows Server 2019 从开始已弃用。 对于 TPM 证明不可能的环境，配置[托管密钥证明](guarded-fabric-initialize-hgs-key-mode.md)。 主机密钥证明提供了类似保障为 AD 模式，并且易于设置。 


本主题介绍准备 HYPER-V 主机，成为受保护的主机使用受信任的管理员证明 （AD 模式） 的中间步骤。 执行这些步骤之前, 完成中的步骤[将成为受保护的主机的主机的配置结构 DNS](guarded-fabric-configuring-fabric-dns-ad.md)。


## <a name="create-a-security-group-and-add-hosts"></a>创建安全组并添加主机

1. 创建一个新**GLOBAL**安全 fabric 域中组和添加的 HYPER-V 主机运行受防护的 Vm。 重新启动主机以更新其组成员身份。

2. 使用 Get ADGroup 获取的安全组的安全标识符 (SID) 并将其提供给 HGS 管理员。 

    ```powershell
    Get-ADGroup "Guarded Hosts"
    ```

    ![Get AdGroup 命令输出](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>向 HGS 注册的安全组的 SID  

1. HGS 服务器上，运行以下命令以向 HGS 注册安全组。 
   重新运行该命令，如有必要的其他组。 
   提供组的友好名称。 
   它不需要以与 Active Directory 安全组名称匹配。 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. 若要验证是否添加了组，请运行[Get HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx)。 

## <a name="next-step"></a>下一步

>[!div class="nextstepaction"]
[确认证明](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>请参阅

- [部署主机保护者服务的受保护的主机和受防护的 Vm](guarded-fabric-deploying-hgs-overview.md)

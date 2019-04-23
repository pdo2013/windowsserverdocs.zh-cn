---
title: 添加受信任的管理员证明的主机信息
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 87089ebc-b953-4aa3-96b5-966cf91acb02
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7949711dbb0f89f5404b491d60938985bfa98c22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849458"
---
>适用于：Windows 服务器 （半年频道），Windows Server 2016

# <a name="authorize-hyper-v-hosts-using-admin-trusted-attestation"></a>授权使用受信任的管理员证明 HYPER-V 主机

>[!IMPORTANT]
>受信任的管理员证明 （AD 模式） 与 Windows Server 2019 从开始已弃用。 对于 TPM 证明不可能的环境，配置[托管密钥证明](guarded-fabric-initialize-hgs-key-mode.md)。 主机密钥证明提供了类似保障为 AD 模式，并且易于设置。 


若要授权在 AD 模式下受保护的主机： 

1. 在构造域中，将 HYPER-V 主机添加到安全组。
2. 在 HGS 域中，向 HGS 注册的安全组的 SID。 

## <a name="add-the-hyper-v-host-to-a-security-group-and-reboot-the-host"></a>将 HYPER-V 主机添加到安全组和重启主机

1. 创建**GLOBAL**安全 fabric 域中组和添加的 HYPER-V 主机运行受防护的 Vm。 
   重新启动主机以更新其组成员身份。

2. 使用 Get ADGroup 获取的安全组的安全标识符 (SID) 并将其提供给 HGS 管理员。 

   ```powershell
   Get-ADGroup "Guarded Hosts"
   ```

   ![Get AdGroup 命令输出](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>向 HGS 注册的安全组的 SID  

1. 获取构造管理员从受保护的主机的安全组的 SID，并运行以下命令以向 HGS 注册安全组。 
   重新运行该命令，如有必要的其他组。 
   提供组的友好名称。 
   它不需要以与 Active Directory 安全组名称匹配。 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. 若要验证是否添加了组，请运行[Get HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx)。 



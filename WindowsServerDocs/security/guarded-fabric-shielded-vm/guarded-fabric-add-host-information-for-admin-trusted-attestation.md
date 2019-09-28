---
title: 为管理员信任的证明添加主机信息
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 87089ebc-b953-4aa3-96b5-966cf91acb02
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 946f91d05063475ae45fb334c67f8d5081d3984d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403715"
---
>适用于：Windows Server（半年频道）、Windows Server 2016

# <a name="authorize-hyper-v-hosts-using-admin-trusted-attestation"></a>使用管理员信任的证明授权 Hyper-v 主机

>[!IMPORTANT]
>从 Windows Server 2019 开始，已弃用管理受信任的证明（AD 模式）。 对于不可能进行 TPM 证明的环境，请配置[主机密钥证明](guarded-fabric-initialize-hgs-key-mode.md)。 主机密钥证明向 AD 模式提供类似的保障，并更易于设置。 


若要在 AD 模式下授权受保护的主机： 

1. 在 fabric 域中，将 Hyper-v 主机添加到安全组。
2. 在 HGS 域中，用 HGS 注册安全组的 SID。 

## <a name="add-the-hyper-v-host-to-a-security-group-and-reboot-the-host"></a>将 Hyper-v 主机添加到安全组，然后重新启动主机

1. 在 fabric 域中创建一个**全局**安全组，并添加将运行受防护的 Vm 的 hyper-v 主机。 
   重新启动主机以更新其组成员身份。

2. 使用 New-adgroup 获取安全组的安全标识符（SID），并将其提供给 HGS 管理员。 

   ```powershell
   Get-ADGroup "Guarded Hosts"
   ```

   ![带有输出的 New-adgroup 命令](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>向 HGS 注册安全组的 SID  

1. 从构造管理员获取受保护的主机的安全组的 SID，并运行以下命令以向 HGS 注册安全组。 
   如果需要，请重新运行其他组的命令。 
   提供组的友好名称。 
   它不需要与 Active Directory 安全组名称匹配。 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. 若要验证是否已添加组，请运行[HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx)。 



---
title: 分支机构注意事项
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: 5a07553e6662fd79230d566ba2049c5e8997f4d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403577"
---
# <a name="branch-office-considerations"></a>分支机构注意事项

> 适用于：Windows Server 2019、Windows Server （半年频道）、 

本文介绍了如何在分支机构和其他远程方案中运行受防护的虚拟机的最佳实践，其中 Hyper-v 主机的连接时间可能与 HGS 的连接受限。

## <a name="fallback-configuration"></a>回退配置

从 Windows Server 版本1709开始，可以在 Hyper-v 主机上配置另一组主机保护者服务 Url，以便在主 HGS 无响应时使用。
这允许您运行作为主服务器使用的本地 HGS 群集，以获得更好的性能，如果本地服务器关闭，则能够回退到您的企业数据中心的 HGS。

若要使用回退选项，需要设置两个 HGS 服务器。 它们可以运行 Windows Server 2019 或 Windows Server 2016，也可以是相同或不同群集的一部分。 如果它们是不同的群集，你将需要建立操作做法，以确保在两个服务器之间同步认证策略。 它们都需要能够正确授权 Hyper-v 主机运行受防护的 Vm，并具有启动受防护的 Vm 所需的密钥材料。 你可以选择在两个群集之间具有一对共享加密和签名证书，也可以使用单独的证书并将 HGS 受防护的 VM 配置为在屏蔽数据中授权这两个监护人（加密/签名证书对）文件.

然后，将 Hyper-v 主机升级到 Windows Server 版本1709或 Windows Server 2019，并运行以下命令：
```powershell
# Replace https://hgs.primary.com and https://hgs.backup.com with your own domain names and protocols
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation' -FallbackKeyProtectionServerUrl 'https://hgs.backup.com/KeyProtection' -FallbackAttestationServerUrl 'https://hgs.backup.com/Attestation'
```

若要取消配置回退服务器，只需省略这两个回退参数：
```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation'
```

为了使 Hyper-v 主机能够通过主服务器和备用服务器传递证明，你需要确保你的证明信息对于这两个 HGS 群集都是最新的。
此外，用于解密虚拟机的 TPM 的证书需要同时在这两个 HGS 群集中可用。
你可以使用不同的证书配置每个 HGS，并将 VM 配置为信任这两者，或将共享的一组证书添加到这两个 HGS 群集。

若要详细了解如何在分支机构中使用后备 Url 配置 HGS，请参阅博客文章[Windows Server 中受防护的 vm （版本1709）的改进分支机构支持](https://blogs.technet.microsoft.com/datacentersecurity/2017/11/15/improved-branch-office-support-for-shielded-vms-in-windows-server-version-1709/)。


## <a name="offline-mode"></a>脱机模式

脱机模式允许受防护的 VM 在无法到达 HGS 时启用，只要 Hyper-v 主机的安全配置尚未更改。
脱机模式通过在 Hyper-v 主机上缓存 VM TPM 密钥保护程序的特殊版本来运行。
密钥保护程序将加密为主机的当前安全配置（使用基于虚拟化的安全标识密钥）。
如果主机无法与 HGS 通信，而且其安全配置未更改，它将能够使用缓存的密钥保护程序启动受防护的 VM。
当系统上的安全设置发生更改时（例如正在应用的新代码完整性策略或禁用安全启动），缓存的密钥保护程序将无效，并且在任何受防护的 Vm 可以再次脱机启动之前，主机必须使用 HGS 进行证明。

脱机模式要求为主机保护者服务群集和 Hyper-v 主机提供 Windows Server 有问必答 Preview 版本17609或更高版本。
它由 HGS 上的策略控制，该策略在默认情况下处于禁用状态。
若要启用对脱机模式的支持，请在 HGS 节点上运行以下命令：

```powershell
Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
```

由于可缓存的密钥保护程序对于每个受防护的 VM 都是唯一的，因此，在 HGS 启用此设置后，你需要完全关闭（不重新启动）并启动受防护的 Vm 以获取可缓存的密钥保护程序。
如果受防护的 VM 迁移到运行较早版本的 Windows Server 的 Hyper-v 主机，或从早期版本的 HGS 获取新的密钥保护程序，则它将不能在脱机模式下启动自身，但可以继续在脱机模式下运行只能.

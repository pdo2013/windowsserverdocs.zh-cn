---
title: 分支 Office 注意事项
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: d93c37227af1eb62368fbcd4ec5d6a48374b45ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877058"
---
# <a name="branch-office-considerations"></a>分支机构注意事项

> 适用于：Windows Server 2019，Windows Server （半年频道） 

本指南介绍了运行受防护的虚拟机在分支机构和其他远程应用场景的 HYPER-V 主机可能的与 HGS 的有限连接的时间段的最佳做法。

## <a name="fallback-configuration"></a>回退配置

从 Windows Server 1709 版开始，可以配置一组额外的主机保护者服务 Url 用于 HYPER-V 主机上时主 HGS 没有响应。
这样，您可以运行一个本地的 HGS 群集，用于作为主服务器更好的性能和进行故障回复到企业数据中心的 HGS 如果本地服务器都已关闭的功能。

若要使用回退选项，你将需要设置两个 HGS 服务器。 它们可以运行 Windows Server 2019 或 Windows Server 2016，并为相同或不同群集的一部分。 如果它们是不同的群集，需要建立操作实践，以确保证明策略是两个服务器之间同步。 它们都需要能够正确授权的 HYPER-V 主机运行受防护的 Vm，并具有启动受防护的 Vm 所需的密钥材料。 可以选择已在共享的加密和签名证书的两个分类之间的一对或使用单独的证书并配置 HGS 受防护的 VM，以授权屏蔽数据中的这两个 guardians （加密/签名证书对）文件。

然后升级到 Windows Server 1709 版或 Windows Server 2019 的 HYPER-V 主机并运行以下命令：
```powershell
# Replace https://hgs.primary.com and https://hgs.backup.com with your own domain names and protocols
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation' -FallbackKeyProtectionServerUrl 'https://hgs.backup.com/KeyProtection' -FallbackAttestationServerUrl 'https://hgs.backup.com/Attestation'
```

若要取消配置回退服务器，只需省略这两个回退参数：
```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation'
```

为了使 HYPER-V 主机将与主要和回退服务器证明，需要确保你的证明信息是最新的两个 HGS 群集。
此外，证书用于解密虚拟机的 TPM 需要在两个 HGS 群集中不可用。
您可以用不同的证书配置每个 HGS 和将 VM 配置为信任和 / 或将共享的一组证书添加到这两个 HGS 群集。

有关配置 HGS 回退 Url 使用的分支机构中的其他信息，请参阅博客文章[改进了 Windows Server 版本 1709年中的受防护 Vm 的分支机构支持](https://blogs.technet.microsoft.com/datacentersecurity/2017/11/15/improved-branch-office-support-for-shielded-vms-in-windows-server-version-1709/)。


## <a name="offline-mode"></a>脱机模式

脱机模式允许您受防护的 VM 以打开时不能达到 HGS，只要 HYPER-V 主机的安全配置未更改。
脱机模式下工作原理是缓存的 HYPER-V 主机上的 VM TPM 密钥保护程序的特殊版本。
密钥保护程序进行加密 （使用虚拟化基于安全标识密钥） 的主机的当前安全配置。
如果你的主机是无法与 HGS 通信，并且其安全配置未更改，它将能够使用缓存的密钥保护程序启动受防护的 VM。
当系统上更改安全设置时，例如新代码完整性策略应用或禁用安全启动，缓存的密钥保护程序都将失效，主机将需要与任何之前 HGS 证明受防护的 Vm 可以脱机重新启动。

脱机模式下需要 17609 或更高版本的 Windows Server Insider Preview 版本的主机保护者服务群集和 HYPER-V 主机。
它由 HGS，默认情况下禁用的策略控制。
若要支持脱机模式下，HGS 节点上运行以下命令：

```powershell
Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
```

由于可缓存的密钥保护程序都是唯一的每个受防护的 VM，将需要完全关闭的情况下 （不重启） 和启动受防护的 Vm 上 HGS 启用此设置后获取可缓存的密钥保护程序。
如果你受防护的 VM 迁移到运行 Windows Server 的较旧版本的 HYPER-V 主机或从较旧版本的 HGS 获取新的密钥保护程序，它将不能在脱机模式下启动本身，但可以继续访问 HGS 时 avail 在联机模式下运行可以。

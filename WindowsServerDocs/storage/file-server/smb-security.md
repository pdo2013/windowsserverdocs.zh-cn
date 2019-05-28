---
title: SMB 安全增强功能
description: Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2016 中的 SMB 加密功能的说明。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: b1586c8c63e46452075b4106c944670395734142
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034400"
---
# <a name="smb-security-enhancements"></a>SMB 安全增强功能

>适用于：Windows Server 2012 R2、 Windows Server 2012 中，Windows Server 2016

本主题介绍 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2016 中的 SMB 安全性增强功能。

## <a name="smb-encryption"></a>SMB 加密

SMB 加密提供了端到端加密的 SMB 数据并遭受窃听不受信任网络中保护数据。 可以将 SMB 加密部署最小的工作量，但它可能需要较小的额外成本的专用的硬件或软件。 它具有对 Internet 协议安全性 (IPsec) 或 WAN 加速器没有要求。 在每个共享的基础上或整个文件服务器中，可以配置 SMB 加密，可以启用它以各种数据遍历未受信任的网络的方案。

>[!NOTE]
>SMB 加密不涉及其他部分，这通常由 BitLocker 驱动器加密处理的安全性。

SMB 加密应视为敏感数据需要从拦截的攻击保护任何方案。 可能的方案包括：

- 使用 SMB 协议移动信息工作者的敏感数据。 SMB 加密，提供文件服务器和客户端，而不考虑遍历，如广域网 (WAN) 连接由非 Microsoft 提供程序维护的网络之间的端到端的保密性和完整性保障。
- SMB 3.0，文件服务器，为服务器应用程序，如 SQL Server 或 HYPER-V 提供连续可用的存储。 启用 SMB 加密提供的机会，以防止窥探攻击，保护该信息。 SMB 加密是使用所需的大多数存储区域网络 (San) 的专用的硬件解决方案比简单得多。

>[!IMPORTANT]
>您应注意，没有运行任何端到端加密保护到非加密进行比较时使用的成本显著的性能。

## <a name="enable-smb-encryption"></a>启用 SMB 加密

您可以启用 SMB 加密，整个文件服务器或仅为指定的文件共享。 使用以下过程之一来启用 SMB 加密：

### <a name="enable-smb-encryption-with-windows-powershell"></a>启用使用 Windows PowerShell 启用 SMB 加密

1. 若要对单个文件共享启用 SMB 加密，请在服务器上键入以下脚本：
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. 若要为整个文件服务器启用 SMB 加密，请在服务器上键入以下脚本：
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. 若要启用 SMB 加密与创建新的 SMB 文件共享，请键入以下脚本：
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>启用 SMB 加密使用服务器管理器

1. 在服务器管理器中，打开**文件和存储服务**。
2. 选择**共享**以打开共享管理页。
3. 右键单击你想要启用 SMB 加密，然后选择的共享**属性**。
4. 上**设置**页上的共享中，选择**加密数据访问**。 远程文件访问此共享进行加密。

### <a name="considerations-for-deploying-smb-encryption"></a>部署 SMB 加密的注意事项

默认情况下，为的文件共享或服务器启用 SMB 加密时只有 SMB 3.0 客户端被允许访问指定的文件共享。 这会强制的数据保护的所有客户端的访问共享的管理员的意图。 但是，在某些情况下，管理员可能想要允许客户端 （例如，在过渡期，混合客户端操作系统版本的使用时） 不支持 SMB 3.0 进行未加密的访问。 若要允许的客户端不支持 SMB 3.0 的未加密的访问，请在 Windows PowerShell 中键入以下脚本：

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

下一节中所述的安全方言协商功能可防止人为干预攻击降级从 SMB 3.0 到 SMB 2.0 （它将使用未加密的访问） 的连接。 但是，它不会阻止降级到 SMB 1.0，它还会导致未加密访问。 若要保证，SMB 3.0 客户端始终使用 SMB 加密访问加密的共享，必须禁用 SMB 1.0 服务器。 (有关说明，请参阅明[禁用 SMB 1.0](#disabling-smb-10)。)如果 **– RejectUnencryptedAccess**设置保留为其默认设置为 **$true**，仅支持加密的 SMB 3.0 客户端被允许访问文件共享 （也会拒绝 SMB 1.0 客户端）。

>[!NOTE]
>* SMB 加密使用高级加密标准 (AES) 的 CCM 算法来加密和解密数据。 AES CCM 还提供了数据完整性验证 （签名） 的加密的文件共享，而不考虑 SMB 签名设置。 如果你想要启用 SMB 签名但未加密，你可以继续执行此操作。 有关详细信息，请参阅[基础知识的 SMB 签名](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/)。
>* 当您尝试访问文件共享或服务器，如果你的组织使用广域网 (wan) 加速设备时，可能会遇到问题。
>* 使用默认配置 （其中不允许对加密的文件共享的未加密的访问），如果不支持 SMB 3.0 尝试访问加密的文件共享，事件 ID 1003 的客户端登录到 Microsoft-Windows-SmbServer/Operational 事件日志并且客户端将收到**访问被拒绝**错误消息。
>* SMB 加密和加密文件系统 (EFS) 在 NTFS 文件系统中是不相关，并且 SMB 加密并不要求或依赖于使用 EFS。
>* SMB 加密和 BitLocker 驱动器加密是不相关，并且 SMB 加密并不要求或依赖于使用 BitLocker 驱动器加密。

## <a name="secure-dialect-negotiation"></a>安全方言协商

SMB 3.0 都能够检测尝试降级 SMB 2.0 或 SMB 3.0 协议或功能的客户端和服务器协商的拦截的攻击。 客户端或服务器检测到此类攻击时，连接将断开连接，并且 Microsoft-Windows-SmbServer/Operational 事件日志中记录事件 ID 为 1005年。 安全方言协商不能检测或防止从 SMB 2.0 或 3.0 降级到 SMB 1.0。 正因为如此，并充分利用 SMB 加密的所有功能，我们强烈建议禁用 SMB 1.0 服务器。 有关详细信息，请参阅[禁用 SMB 1.0](#disabling-smb-10)。

下一节中描述的安全方言协商功能可防止人为干预攻击降级从 SMB 3 到 SMB 2 （它将使用未加密的访问）; 的连接但是，它不会阻止降级到 SMB 1，这也会导致未加密访问。 使用前面的 SMB 的非 Windows 实现的潜在问题的详细信息，请参阅[Microsoft 知识库文章](http://support.microsoft.com/kb/2686098)。

## <a name="new-signing-algorithm"></a>新的签名算法

SMB 3.0 将较新的加密算法用于签名：高级的加密标准 (AES) 和密码的基于消息身份验证代码 (CMAC)。 SMB 2.0 使用较旧的 HMAC-SHA256 加密算法。 AES CMAC 和 AES CCM 可显著加快数据加密已支持 AES 指令的大多数现代 Cpu 上。 有关详细信息，请参阅[基础知识的 SMB 签名](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/)。

## <a name="disabling-smb-10"></a>禁用 SMB 1.0

旧计算机浏览器服务和 SMB 1.0 中的远程管理协议功能现在是独立的并且可以消除。 这些功能默认情况下，仍处于启用状态，但如果你没有旧版 SMB 客户端，例如运行 Windows Server 2003 或 Windows XP 的计算机可以删除 SMB 1.0 功能以提高安全性，并可以减少修补。

>[!NOTE]
>Windows Server 2008 和 Windows Vista 中引入了 SMB 2.0。 旧版客户端，例如运行 Windows Server 2003 或 Windows XP 的计算机不支持 SMB 2.0;因此，它们将不能访问文件共享或打印共享，如果 SMB 1.0 服务器处于禁用状态。 此外，某些非 Microsoft SMB 客户端可能无法再访问 SMB 2.0 文件共享或打印共享 （例如，"扫描到共享"功能的打印机）。

禁用 SMB 1.0 开始之前，您需要找出是否 SMB 客户端当前连接到运行 SMB 1.0 的服务器。 若要执行此操作，请在 Windows PowerShell 中输入以下 cmdlet:

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>在过去的一周 （多个时间每一天） 生成的审核线索，应该反复运行此脚本。 也可以运行这作为计划任务。

若要禁用 SMB 1.0，请在 Windows PowerShell 中输入以下脚本：

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>如果 SMB 客户端连接被拒绝，因为运行 SMB 1.0 服务器已被禁用，将 Microsoft-Windows-SmbServer/Operational 事件日志中记录事件 ID 为 1001年。

## <a name="more-information"></a>详细信息

以下是有关 SMB 和 Windows Server 2012 中的相关的技术的一些其他资源。

- [服务器消息块](file-server-smb-overview.md)
- [Windows Server 中存储](../storage.md)
- [应用程序数据的横向扩展文件服务器](../../failover-clustering/sofs-overview.md)
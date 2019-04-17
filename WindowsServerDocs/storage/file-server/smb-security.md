---
title: SMB 安全性增强功能
description: 在 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2016 SMB 加密功能的说明。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 831ca8266c3ec18ffb83227dcb2d39b3f953ad1a
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233523"
---
# <a name="smb-security-enhancements"></a>SMB 安全性增强功能

>适用于： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2016

本主题介绍在 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2016 SMB 安全性增强功能。

## <a name="smb-encryption"></a>SMB 加密

SMB 加密提供 SMB 数据的端到端加密，并防止窃听出现在不受信任的网络上的数据。 您可以部署 SMB 加密很少的操作，但可能需要小型额外的成本的专用的硬件或软件。 具有 Internet 协议安全性 (IPsec) 或 WAN 加速器不要求。 SMB 加密可根据每个共享或为整个文件服务器配置和启用各种方案数据遍历网络不受信任的位置。

>[!NOTE]
>SMB 加密不包括安全，网址 rest，通常由 BitLocker 驱动器加密处理。

需要用从中间中中间人攻击保护敏感数据任何方案，应考虑授予 SMB 加密。 可能的方案包括：

- 使用 SMB 协议移动信息工作者的敏感数据。 SMB 加密提供了文件服务器和客户端，无论遍历，如广域网 (WAN) 连接的非 Microsoft 提供商维护的网络之间的端到端隐私和完整性保证。
- SMB 3.0 允许文件服务器的服务器应用程序，如 SQL Server 或 HYPER-V 提供连续可用存储。 启用 SMB 加密提供这些信息防止窥探攻击的机会。 SMB 加密是使用比所需的大多数存储区域网络 (San) 的专用的硬件解决方案更简单。

>[!IMPORTANT]
>您应注意，没有运营成本与任何端到端加密保护与非加密相比重要性能。

## <a name="enable-smb-encryption"></a>启用 SMB 加密

为整个文件服务器还是仅对特定的文件共享，则可以启用 SMB 加密。 使用以下过程之一来启用 SMB 加密：

### <a name="enable-smb-encryption-with-windows-powershell"></a>启用使用 Windows PowerShell SMB 加密

1. SMB 加密启用单个文件共享，请在服务器上键入以下脚本：
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. 要为整个文件服务器启用 SMB 加密，请在服务器上键入以下脚本：
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. 若要创建新的 SMB 文件共享与启用 SMB 加密，请键入以下脚本：
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>启用 SMB 加密使用服务器管理器

1. 在服务器管理器中，打开**文件和存储服务**。
2. 选择**共享**以打开共享管理页。
3. 右键单击您要在其启用 SMB 加密的共享，然后选择**属性**。
4. 在共享的**设置**页中，选择**加密数据访问**。 对此共享的远程文件访问进行加密。

### <a name="considerations-for-deploying-smb-encryption"></a>SMB 加密的部署注意事项

默认情况下，为文件共享或服务器，启用 SMB 加密时仅 SMB 3.0 客户端允许访问指定的文件共享。 这强制执行的所有客户端的访问共享保护数据的管理员的用途。 但是，在某些情况下，管理员可能需要允许客户端 （例如，在使用混合客户端操作系统版本的时转换期间） 不支持 SMB 3.0 的未加密的访问。 若要允许客户端不支持 SMB 3.0 的未加密的访问，请在 Windows PowerShell 中键入以下脚本：

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

下一节中所述的安全方言协商功能可以防止中间中中间人攻击降级从 SMB 3.0 SMB 2.0 （这将使用未加密的访问） 的连接。 但是，它不阻止降级到 SMB 1.0，还会导致在未加密的访问。 若要保证，SMB 3.0 客户端始终使用 SMB 加密访问加密的共享，必须禁用 SMB 1.0 服务器。 （有关说明，请参阅部分[禁用 SMB 1.0](#disabling-smb-1.0)）。如果 **– RejectUnencryptedAccess**设置保留 **$true**默认设置，允许仅支持加密的 SMB 3.0 客户端访问 （还将拒绝 SMB 1.0 客户端） 的文件共享。

>[!NOTE]
>* SMB 加密使用高级加密标准 (AES)-CCM 算法进行加密和解密的数据。 AES CCM 还提供了 （签名） 加密的文件共享，无论 SMB 签名设置的数据完整性验证。 如果您想要启用 SMB 签名不加密，您可以继续执行此操作。 有关详细信息，请参阅[基础知识的 SMB 签名](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/)。
>* 当您尝试访问的文件共享或服务器，如果您的组织使用广域网 (wan) 加速 appliance 时，可能会遇到问题。
>* 使用默认配置 （如果没有未加密的访问加密的文件共享允许），如果不支持 SMB 3.0 尝试访问加密的文件共享，事件 ID 1003 的客户端登录到 Microsoft Windows-SmbServer/运营事件日志和客户端将收到**访问拒绝**错误消息。
>* SMB 加密和加密文件系统 (EFS) NTFS 文件系统中不相关，并且在 SMB 加密不需要或取决于使用 EFS。
>* SMB 加密和 BitLocker 驱动器加密无关，并且在 SMB 加密不需要或取决于使用 BitLocker 驱动器加密。

## <a name="secure-dialect-negotiation"></a>安全方言协商

SMB 3.0 是可以检测尝试降级 SMB 2.0 或 SMB 3.0 协议或客户端和服务器协商的功能的中间中中间人攻击。 通过客户端或服务器检测到此类攻击时，将断开连接和 Microsoft Windows-SmbServer/运营事件日志中记录事件 ID 1005。 安全方言协商无法检测或阻止从 SMB 2.0 或 3.0 降级到 SMB 1.0。 因此，并利用 SMB 加密的完整功能，我们强烈建议您禁用 SMB 1.0 服务器。 有关详细信息，请参阅[禁用 SMB 1.0](#disabling-smb-1.0)。

下一节所述的安全方言协商功能可以防止中间中中间人攻击降级到 SMB 2 （它将使用未加密的访问）; SMB 3 的连接但是，它不阻止降级到 SMB 1，还会导致在未加密的访问。 与非 Windows 实现更早版本的 SMB 的潜在问题的详细信息，请参阅[Microsoft 知识库文章](http://support.microsoft.com/kb/2686098)。

## <a name="new-signing-algorithm"></a>新的签名算法

SMB 3.0 签名使用较新的加密算法： 高级加密标准 (AES)-加密-基于消息验证代码 (CMAC)。 SMB 2.0 使用较旧的 HMAC SHA256 加密算法。 AES CMAC 和 AES CCM 可以显著加快数据加密上具有 AES 指令支持的最现代 Cpu。 有关详细信息，请参阅[基础知识的 SMB 签名](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/)。

## <a name="disabling-smb-10"></a>禁用 SMB 1.0

旧计算机浏览器服务和远程管理协议中 SMB 1.0 的功能现在是独立的并可以消除。 这些功能默认情况下，仍处于启用状态，但如果您没有旧 SMB 客户端，例如计算机运行 Windows Server 2003 或 Windows XP 中，您可以删除 SMB 1.0 功能，以提高安全性，并可能会降低修补。

>[!NOTE]
>SMB 2.0 是 Windows Server 2008 和 Windows Vista 中引入的。 SMB 2.0; 不支持旧客户端，如运行 Windows Server 2003 或 Windows XP 的计算机因此，它们将无法访问文件共享或打印共享如果 SMB 1.0 服务器被禁用。 此外，某些 Microsoft SMB 客户端可能无法访问 SMB 2.0 文件共享或打印共享 （例如，使用"扫描共享"功能的打印机）。

在开始禁用 SMB 1.0 之前，您将需要以找出是否 SMB 客户端当前连接到运行 SMB 1.0 的服务器。 若要执行此操作，请在 Windows PowerShell 中输入以下 cmdlet:

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>每周 （多次每一天） 生成审计线索的过程，应重复运行此脚本。 您还可以运行此作为计划任务。

若要禁用 SMB 1.0，请在 Windows PowerShell 中输入以下脚本：

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>如果 SMB 客户端连接被拒绝，因为运行 SMB 1.0 的服务器已被禁用，将在 Microsoft Windows-SmbServer/运营事件日志中记录事件 ID 1001。

## <a name="more-information"></a>详细信息

以下是一些有关 SMB 和 Windows Server 2012 中的相关的技术的其他资源。

- [服务器消息块](file-server-smb-overview.md)
- [Windows Server 中的存储](../storage.md)
- [向外扩展文件服务器应用程序数据](../../failover-clustering/sofs-overview.md)
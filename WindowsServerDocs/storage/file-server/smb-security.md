---
title: SMB 安全增强功能
description: Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2016 中的 SMB 加密功能的说明。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7221d3ea94ff9f2d7fca8e95cee66597e2dc6270
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402071"
---
# <a name="smb-security-enhancements"></a>SMB 安全增强功能

>适用于：Windows Server 2012 R2、Windows Server 2012、Windows Server 2016

本主题介绍 Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2016 中的 SMB 安全增强功能。

## <a name="smb-encryption"></a>SMB 加密

SMB 加密提供 SMB 数据的端对端加密并防止数据在不受信任的网络上窃听。 你可以通过最少的工作量来部署 SMB 加密，但对于专用硬件或软件，可能需要少量额外的费用。 它不要求提供 Internet 协议安全性（IPsec）或 WAN 加速器。 可以基于每个共享配置 SMB 加密，也可以针对整个文件服务器进行配置，并且可以在数据遍历不受信任网络的各种情况下启用。

>[!NOTE]
>SMB 加密不涉及静态安全性，这通常由 BitLocker 驱动器加密处理。

如果需要保护敏感数据免受中间人攻击的情况，应考虑进行 SMB 加密。 可能的方案包括：

- 使用 SMB 协议移动信息工作者的敏感数据。 SMB 加密在文件服务器和客户端之间提供端到端的隐私和完整性保障，而不考虑所遍历的网络，例如由非 Microsoft 提供商维护的广域网（WAN）连接。
- SMB 3.0 使文件服务器能够为服务器应用程序（如 SQL Server 或 Hyper-v）提供持续可用的存储。 启用 SMB 加密可提供保护该信息免受窥探攻击的机会。 SMB 加密比大多数存储区域网络（San）所需的专用硬件解决方案更容易使用。

>[!IMPORTANT]
>请注意，与非加密相比，任何端到端的加密保护都会产生显著的性能操作成本。

## <a name="enable-smb-encryption"></a>启用 SMB 加密

您可以为整个文件服务器启用 SMB 加密，也可以只为特定文件共享启用 SMB 加密。 使用以下过程之一来启用 SMB 加密：

### <a name="enable-smb-encryption-with-windows-powershell"></a>使用 Windows PowerShell 启用 SMB 加密

1. 若要为单个文件共享启用 SMB 加密，请在服务器上键入以下脚本：
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. 若要为整个文件服务器启用 SMB 加密，请在服务器上键入以下脚本：
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. 若要创建新的 SMB 文件共享并启用 SMB 加密，请键入以下脚本：
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>使用服务器管理器启用 SMB 加密

1. 在服务器管理器中，打开 "**文件和存储服务**"。
2. 选择 "**共享**" 以打开 "共享" 管理页。
3. 右键单击要启用 SMB 加密的共享，然后选择 "**属性**"。
4. 在共享的 "**设置**" 页上，选择 "**加密数据访问**"。 对此共享的远程文件访问已加密。

### <a name="considerations-for-deploying-smb-encryption"></a>部署 SMB 加密的注意事项

默认情况下，为文件共享或服务器启用 SMB 加密时，只允许 SMB 3.0 客户端访问指定的文件共享。 这会强制管理员为访问共享的所有客户端的数据提供保护。 但是，在某些情况下，管理员可能希望对不支持 SMB 3.0 的客户端（例如，在使用混合客户端操作系统版本时在转换期间）允许未加密的访问。 若要允许不支持 SMB 3.0 的客户端进行非加密访问，请在 Windows PowerShell 中键入以下脚本：

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

下一节中所述的安全方言协商功能可防止中间人攻击从 SMB 3.0 降级到 SMB 2.0 （这将使用未加密的访问）。 但是，它不会阻止降级到 SMB 1.0，这也会导致未加密的访问。 为了保证 SMB 3.0 客户端始终使用 SMB 加密来访问加密的共享，必须禁用 SMB 1.0 服务器。 （有关说明，请参阅[禁用 SMB 1.0](#disabling-smb-10)部分。）如果 " **– RejectUnencryptedAccess** " 设置保留 **$true**的默认设置，则仅允许支持加密的 SMB 3.0 客户端访问文件共享（SMB 1.0 客户端也将被拒绝）。

>[!NOTE]
>* SMB 加密使用高级加密标准（AES）-CCM 算法加密和解密数据。 AES-CCM 还为加密文件共享提供数据完整性验证（签名），而不考虑 SMB 签名设置。 如果要在不加密的情况下启用 SMB 签名，则可继续执行此操作。 有关详细信息，请参阅[SMB 签名基础知识](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/)。
>* 如果你的组织使用广域网络（WAN）加速设备，则当你尝试访问文件共享或服务器时，可能会遇到问题。
>* 如果客户端不支持 SMB 3.0 尝试访问加密文件共享，则使用默认配置（不允许对加密文件共享进行未加密的访问），将事件 ID 1003 记录到 SmbServer/操作事件日志，客户端将收到 "**拒绝访问**" 错误消息。
>* SMB 加密和 NTFS 文件系统中的加密文件系统（EFS）是不相关的，SMB 加密不需要或依赖于使用 EFS。
>* SMB 加密与 BitLocker 驱动器加密无关，SMB 加密不需要或依赖于使用 BitLocker 驱动器加密。

## <a name="secure-dialect-negotiation"></a>安全方言协商

SMB 3.0 能够检测到中间人攻击，这些攻击会尝试对客户端和服务器协商的 SMB 2.0 或 SMB 3.0 协议或功能进行降级。 当客户端或服务器检测到这种攻击时，连接将断开连接，并在 SmbServer/operation 事件日志中记录事件 ID 1005。 安全方言协商无法检测或阻止从 SMB 2.0 或3.0 降级到 SMB 1.0。 因此，为了充分利用 SMB 加密的全部功能，我们强烈建议你禁用 SMB 1.0 服务器。 有关详细信息，请参阅[禁用 SMB 1.0](#disabling-smb-10)。

下一节中所述的安全方言协商功能可防止中间人攻击从 SMB 3 降级到 SMB 2 （这将使用未加密的访问）;但是，它不会阻止对 SMB 1 的降级，这也会导致未加密的访问。 有关 SMB 的早期非 Windows 实现的潜在问题的详细信息，请参阅[Microsoft 知识库](http://support.microsoft.com/kb/2686098)。

## <a name="new-signing-algorithm"></a>新签名算法

SMB 3.0 使用较新的加密算法进行签名：高级加密标准（AES）-基于密码的消息身份验证代码（CMAC）。 SMB 2.0 使用了较旧的 HMAC-SHA256 加密算法。 AES-CMAC 和 AES-CCM 可以极大地加快支持 AES 指令的大多数现代 Cpu 的数据加密。 有关详细信息，请参阅[SMB 签名基础知识](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/)。

## <a name="disabling-smb-10"></a>禁用 SMB 1。0

SMB 1.0 中的旧的计算机浏览器服务和远程管理协议功能现在是独立的，可以消除它们。 默认情况下，这些功能仍处于启用状态，但是，如果你没有旧的 SMB 客户端，如运行 Windows Server 2003 或 Windows XP 的计算机，则可以删除 SMB 1.0 功能以提高安全性，并可能减少修补。

>[!NOTE]
>SMB 2.0 是在 Windows Server 2008 和 Windows Vista 中引入的。 旧客户端（例如运行 Windows Server 2003 或 Windows XP 的计算机）不支持 SMB 2.0;因此，如果禁用 SMB 1.0 服务器，他们将无法访问文件共享或打印共享。 此外，某些非 Microsoft SMB 客户端可能无法访问 SMB 2.0 文件共享或打印共享（例如，具有 "扫描共享" 功能的打印机）。

开始禁用 SMB 1.0 之前，你需要了解你的 SMB 客户端当前是否连接到运行 SMB 1.0 的服务器。 为此，请在 Windows PowerShell 中输入以下 cmdlet：

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>你应在一周的某一周（每天多次）重复运行此脚本，以生成审核记录。 你还可以将其作为计划任务运行。

若要禁用 SMB 1.0，请在 Windows PowerShell 中输入以下脚本：

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>如果 SMB 客户端连接被拒绝，因为运行 SMB 1.0 的服务器已被禁用，事件 ID 1001 将记录在 Microsoft SmbServer/操作事件日志中。

## <a name="more-information"></a>详细信息

下面是有关 Windows Server 2012 中的 SMB 和相关技术的一些其他资源。

- [服务器消息块](file-server-smb-overview.md)
- [Windows Server 中的存储](../storage.md)
- [应用程序数据的横向扩展文件服务器](../../failover-clustering/sofs-overview.md)
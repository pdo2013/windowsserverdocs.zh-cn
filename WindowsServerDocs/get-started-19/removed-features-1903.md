---
title: 从 Windows Server 版本 1903 开始已删除或计划取代的功能
description: 以下是 Windows Server 版本 1903 中已从该版本的产品中删除或开始考虑在后续版本中可能取代的功能列表。 本文档面向在商业环境中更新操作系统的 IT 专业人员。
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
ms.date: 08/22/2019
author: jasongerend
ms.author: jgerend
manager: daveba
ms.openlocfilehash: b31cde8216b3ceb230c9c197924b40e8cc8fc3f8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361184"
---
# <a name="features-removed-or-planned-for-replacement-starting-with-windows-server-version-1903"></a>从 Windows Server 版本 1903 开始已删除或计划取代的功能

>适用于：Windows Server 版本 1903

以下是 Windows Server 版本 1903 中已从该版本的产品中删除或开始考虑在后续版本中可能取代的功能列表。 本文档面向在商业环境中更新操作系统的 IT 专业人员。 **在后续的版本中可能会对该列表进行更改，并且可能不包含任何受影响的功能。**

另请参阅 [Windows Server 中已删除或计划取代的功能](removed-features.md)。

## <a name="features-were-no-longer-developing"></a>不再开发的功能

我们不再积极开发这些功能，并有可能在未来更新中将其删除。 部分功能已被替换为其他特性或功能，另外一些现在从其他提供源提供。 

如果你有关于建议替换其中任何一项功能方面的反馈，可以使用[反馈中心应用](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)。 


|                         功能                         |                                                                                                                                                                                                                                                                                                                                                                                                                           可以改用                                                                                                                                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              Wi-Fi WEP 和 TKIP（**新**）               |                                                                                                                                                                  使用旧式 WEP 和 TKIP 加密法的 Wi-Fi 网络不如使用 AES 的 WPA2 和 WPA3 等网络那样安全。 在 Windows 10 版本 1903 上，连接到 WEP 或 TKIP 网络会显示一条指出该网络不安全的警告消息；但 Windows Server 版本 1903 上不会显示此类消息。 将来的版本不允许使用这些旧式加密法来与 Wi-Fi 网络建立任何连接。 有关 WEP 和 TKIP 安全风险的详细信息，请参阅此[博客文章](https://go.microsoft.com/fwlink/p/?linkid=2008426)。                                                                                                                                                                   |
|       基于 XDDM 的远程显示驱动程序（**新**）        |                                                                                                                                          从此版本开始，远程桌面服务将为单个会话远程桌面使用基于 Windows 显示驱动程序模型 (WDDM) 的间接显示驱动程序 (IDD)。 在将来的版本中，会删除对基于 Windows 2000 显示驱动程序模型 (XDDM) 的远程显示驱动程序的支持。 使用基于 XDDM 的远程显示驱动程序的独立软件供应商应该计划迁移到 WDDM 驱动程序模型。 有关实现远程显示的详细信息，间接显示驱动程序 ISV 可以联系 [rdsdev@microsoft.com](mailto:rdsdev@microsoft.com)。                                                                                                                                           |
|            UCS 日志收集工具（**新**）            |                                                                                                                                                                                                                                                                                                                                                         UCS 日志收集工具尽管并不是专门在 Windows Server 上使用，但它即将由 Windows 10 中的反馈中心取代。                                                                                                                                                                                                                                                                                                                                                         |
|              Hyper-V 中的密钥存储驱动器               |                                                                                                                                                                                                        我们不再开发 Hyper-V 中的密钥存储驱动器功能。 如果你使用的是第 1 代 VM，请查看[第 1 代 VM 虚拟化安全性](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v)来了解今后的选项。 如果你正在创建新的 VM，请结合 TPM 设备使用第 2 代虚拟机，以实现更安全的解决方案。                                                                                                                                                                                                         |
|    受信任的平台模块 (TPM) 管理控制台     |                                                                                                                                                                                                                          以前在 TPM 管理控制台中提供的信息现在会在 [Windows Defender 安全中心](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center)的[**设备安全性**](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security)页中提供。                                                                                                                                                                                                                          |
| 主机防护服务 Active Directory 证明模式 | 我们不再开发主机防护服务 Active Directory 证明模式 - 而是添加了一种新的证明模式[主机密钥证明](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md)，与基于 Active Directory 的证明相比，此模式要简单得多，且兼容性相当。  与 Active Directory 证明相比，此新模式为安装体验提供等效的功能、更简单的管理和更少的基础结构依赖关系。 主机密钥证明的硬件要求不比 Active Directory 证明更高，因此，所有现有系统可与新模式保持兼容。 有关证明选项的详细信息，请参阅[部署受保护的主机](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)。 |
|                     OneSync 服务                     |                                                                                                                                                                                                                                                                                                                                                   OneSync 服务可以同步邮件、日历和人员应用的数据。 我们已在 Outlook 应用中添加了一个同步引擎用于提供相同的同步。                                                                                                                                                                                                                                                                                                                                                    |
|       远程差异压缩 API 支持       |                                                                                                                                                                                                                                                                                                           借助远程差异压缩 API 支持，可以使用压缩技术来实现与远程源同步数据，最大程度地减少通过网络发送的数据量。 |
|         WFP 轻型筛选交换机扩展         |                                                                                                                                                                                                                                      WFP 轻型筛选交换机扩展可让开发人员构建 [Hyper-V 虚拟交换机的简单网络数据包筛选扩展](https://docs.microsoft.com/windows-hardware/drivers/network/using-virtual-switch-filtering)。 可以通过创建完整的筛选扩展来实现相同的功能。 因此，我们将来会删除此扩展。                                                                                                                                                                                                                                      |


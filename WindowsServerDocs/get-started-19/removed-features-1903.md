---
title: 已删除或替换从 Windows Server，版本 1903年开始计划的功能
description: 以下是一系列功能和在 Windows Server，版本 1903年从产品中的已删除的功能版本或正在启动以作为在后续版本中的潜在替换。 本文档面向在商业环境中更新操作系统的 IT 专业人员。
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.date: 06/11/2019
author: jasongerend
ms.author: jgerend
manager: daveba
ms.openlocfilehash: 510f93d64d9cbd994778b8e1733e53e3cf7be0f6
ms.sourcegitcommit: 2cce92e99e237a271722955c86544826ba205c69
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2019
ms.locfileid: "66841354"
---
# <a name="features-removed-or-planned-for-replacement-starting-with-windows-server-version-1903"></a>已删除或替换从 Windows Server，版本 1903年开始计划的功能

>适用于：Windows Server 版本 1903

以下是一系列功能和在 Windows Server，版本 1903年从产品中的已删除的功能版本或正在启动以作为在后续版本中的潜在替换。 本文档面向在商业环境中更新操作系统的 IT 专业人员。 **此列表可能会发生在后续版本中的更改，可能不包括每个受影响的功能。**

另请参阅[功能已删除或计划来启动 Windows Server 2019 更换](removed-features-19.md)。

## <a name="features-were-no-longer-developing"></a>不再开发的功能

我们不再主动制订这些功能，并可能会从将来的更新中删除它们。 部分功能已被替换为其他特性或功能，另外一些现在从其他提供源提供。 

如果你有关于建议替换其中任何一项功能方面的反馈，可以使用[反馈中心应用](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)。 


|                         功能                         |                                                                                                                                                                                                                                                                                                                                                                                                                           您可以改用                                                                                                                                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              Wi-fi WEP 和 TKIP (**新**)               |                                                                                                                                                                  使用较旧的 WEP 和 TKIP 加密法的 Wi-fi 网络的安全性与使用 AES，WPA2 和 WPA3 等。 Windows 10，版本 1903，连接到的 WEP 或 TKIP 网络会显示一条警告消息在网络并不安全，但没有消息显示在 Windows Server 版本 1903年。 未来版本中将不允许任何连接到 Wi-fi 网络使用这些旧的密码。 WEP 和 TKIP 的安全风险的详细信息，请参阅此[博客文章](https://go.microsoft.com/fwlink/p/?linkid=2008426)。                                                                                                                                                                   |
|       XDDM 远程显示器驱动程序 (**新**)        |                                                                                                                                          此版本中启动远程桌面服务使用 Windows 显示器驱动程序模型 (WDDM) 的基于单个会话的远程桌面的间接显示驱动程序 (IDD)。 将在未来的版本中删除 Windows 2000 显示驱动程序模型 (XDDM) 基于远程显示器驱动程序的支持。 使用 XDDM 远程显示器驱动程序的独立软件供应商应计划迁移到 WDDM 驱动程序模型。 有关详细信息实现远程显示间接显示器驱动程序，Isv 可以联系[ rdsdev@microsoft.com ](mailto:rdsdev@microsoft.com)。                                                                                                                                           |
|            UCS 日志收集工具 (**新**)            |                                                                                                                                                                                                                                                                                                                                                         Windows 10 上的反馈中心尽管如此替换 UCS 日志收集工具，而不显式适用于 Windows Server。                                                                                                                                                                                                                                                                                                                                                         |
|              在 HYPER-V 中的密钥存储驱动器               |                                                                                                                                                                                                        我们不再正致力于在 HYPER-V 中的密钥存储驱动器功能。 如果使用的第 1 代 Vm，请查看[代 1 VM 虚拟化的安全性](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v)今后的选项有关的信息。 如果你是要使用更安全的解决方案的 TPM 设备创建新的 Vm 使用第 2 代虚拟机。                                                                                                                                                                                                         |
|    受信任的平台模块 (TPM) 管理控制台     |                                                                                                                                                                                                                          以前在 TPM 管理控制台中提供的信息现在已接入[**设备安全**](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security)页[Windows Defender 安全中心](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center)。                                                                                                                                                                                                                          |
| 主机保护者服务 Active Directory 证明模式 | 我们不会再开发主机保护者服务 Active Directory 证明模式-现在我们已添加了新的证明模式，[托管密钥证明](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md)，这就是简单得多和同样与基于 Active Directory 为兼容证明。  这个新模式提供了等效功能的安装体验、 更简单的管理和 Active Directory 证明比较少的基础结构依赖项。 主机密钥证明具有必需的哪些 Active Directory 证明以外没有其他硬件要求，因此所有的现有系统将保留与新模式兼容。 请参阅[部署受保护的主机](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)证明选项有关的详细信息。 |
|                     OneSync 服务                     |                                                                                                                                                                                                                                                                                                                                                   OneSync 服务会同步邮件、 日历和人员应用的数据。 我们已添加到的 Outlook 应用程序提供相同的同步的同步引擎。                                                                                                                                                                                                                                                                                                                                                    |
|       远程差分压缩 API 支持       |                                                                                                                                                                                                                                                                                                           远程差分压缩 API 支持启用与使用压缩技术，通过网络发送的数据量最小化的远程源同步数据。 |
|         WFP 轻型筛选器交换机扩展         |                                                                                                                                                                                                                                      WFP 轻型筛选器交换机扩展开发人员可以构建[简单的网络数据包筛选的 HYPER-V 虚拟交换机扩展](https://docs.microsoft.com/en-us/windows-hardware/drivers/network/using-virtual-switch-filtering)。 可以通过创建完整的筛选扩展来实现相同的功能。 在这种情况下，我们将删除此扩展在将来。                                                                                                                                                                                                                                      |


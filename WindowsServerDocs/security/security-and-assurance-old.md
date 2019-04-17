---
title: 安全和保障
description: Windows Server 2016 安全概述
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/27/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: medium
ms.openlocfilehash: b32b4879ad454d1154c3d65dbf690cdaae73d76c
ms.sourcegitcommit: 07ac08dea2b8f2763c2614a999dc7967018aa0b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/07/2018
ms.locfileid: "6121446"
---
# Windows Server 中的安全和保障 

>适用于：Windows Server（半年频道）、Windows Server 2016

>[!TIP]
> 需要有关较旧版本的 Windows Server 的信息？ 在 docs.microsoft.com 上查看我们的其他 [Windows Server 库](/previous-versions/windows/)。 你还可以[搜索此网站](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)查找特定信息。

<img src="../media/landing-icons/security.png" style='float:left; padding:.5em;' alt="Icon representing a lock"> 你可以依靠内置于操作系统的新保护层进一步防止出现安全漏洞。 帮助阻止恶意攻击并提高虚拟机、应用程序和数据的安全性。


### [Windows Server 安全博客文章](https://blogs.technet.microsoft.com/windowsserver/2016/04/25/ten-reasons-youll-love-windows-server-2016-8-security/)
Windows Server 安全团队的这篇博客文章重点介绍了 Windows Server 中可以提高托管和混合云环境的安全的许多改进。

### [数据中心和私有云安全博客](https://blogs.technet.microsoft.com/datacentersecurity/)
这是来自 Microsoft 数据中心和私有云安全团队的技术内容的中心博客站点。                                    

### [应对新兴威胁和横向转移](https://www.youtube.com/watch?v=B5JMYxYWx1k&feature=youtu.be)
在这段时长 6 分钟的视频中，Anders Vinberg 概述了 Microsoft 的安全和保障策略，并讨论了与安全相关的行业趋势和横向转移。 随后重点讨论了通过基础构造保护工作负荷，以及防止从特权帐户发起的直接攻击等 Microsoft 主要计划。 最后，他介绍了在出现漏洞的情况下，如何利用新的检测和取证功能来帮助更好地识别威胁。

### [保护你的数据中心和云免受新兴威胁博客文章](http://blogs.technet.com/b/windowsserver/archive/2015/11/18/protecting-your-datacenter-and-cloud-november-update.aspx)
这篇博客文章讨论了如何使用 Microsoft 技术保护你的数据中心和云投资免受新兴威胁。                   

### [Ignite 安全和保障概述会议](http://channel9.msdn.com/events/ignite/2015/brk2482)
此次 Ignite 会议解决了持续威胁、内部违规、有组织的网络犯罪问题，旨在为 Microsoft 云平台（本地服务以及使用 Azure 的连接的服务）提供保护。 会议提供了用于保护工作负荷、大型企业租户和服务提供商的方案。                                                                   

## 使用受防护的 VM 保障虚拟化

### [通道 9 中的受防护的 VM](http://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
受防护的虚拟机技术演练和权益。                           

### [受防护的 VM 演示](https://www.youtube.com/watch?v=xip5Qtk-7d8)
这段时长 4 分钟的视频介绍了受防护的 VM 的价值以及受防护的 VM 和未受防护的 VM 之间的区别。                                   

### [Windows Server 中受防护的虚拟机视频演练](http://microsoft-cloud.cloudguides.com/Guides/Shielded Virtual Machines in Windows Server.htm)
本视频演练演示主机保护者服务如何启用受防护的虚拟机，以便防止 Hyper-V 主机管理员对敏感数据进行未授权的访问。

### [增强构造：保护 Hyper-V 中的租户密钥（Ignite 视频）](http://channel9.msdn.com/events/ignite/2015/brk3457)

此 Ignite 演示文稿讨论了 Hyper-V 中的改进、Virtual Machine Manager 以及启用受防护的虚拟机的新主机保护者服务器角色。                

### [受保护的构造部署指南](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview)
本指南提供用于受保护的构造主机和受防护的 VM 的 Windows Server 和 System Center Virtual Machine Manager 的安装和验证信息。

### [分支机构中的受防护的虚拟机和受保护的构造](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office)
本指南提供在分支机构和其他远程场景（Hyper-V 主机在一段时间内与 HGS 的连接受限）中运行受防护的虚拟机的最佳实践。

### [受防护的 VM 和受保护的构造疑难解答指南](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-overview)
本指南提供有关如何解决在受防护的 VM 环境中可能遇到的问题的信息。

### [受防护的 VM 相关文章](http://windowsitpro.com/hyper-v/super-secure-hyper-v-environments-shielded-vms-2016)
本白皮书概述了受防护的 VM 如何提供增强的总体安全性以防止篡改。                                         

## 特权访问管理
### [保护特权访问](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access)
有关如何保护特权访问的路线图。 此路线图基于服务器安全团队、Microsoft IT、Azure 团队和 Microsoft 咨询服务部门的综合专业知识建立而成                           

### [使用 Microsoft Identity Manager 进行实时管理](https://technet.microsoft.com/library/mt150258.aspx)
本文讨论 Microsoft Identity Manager 中所包含的特性和功能，包括对实时 (JIT) 特权访问管理的支持。                                                                    

### [使用 Privileged Access Management 保护 Windows 和 Microsoft Azure Active Directory](http://channel9.msdn.com/events/ignite/2015/brk3873)
此 Ignite 演示文稿介绍用于解决通过更强的身份验证进行管理员访问的风险，以及使用实时和 Just Enough Administration (JEA) 管理访问的 Windows Server、PowerShell、Active Directory、Identity Manager 和 Azure Active Directory 中的 Microsoft 策略和投资。

### [Just Enough Administration 相关文章](http://aka.ms/JEA)
本文档分享 Just Enough Administration 的愿景和技术详细信息，这是一个 PowerShell 工具包，旨在帮助组织通过限制操作员仅具有执行特定任务所需的访问权限来降低风险。

### [Just Enough Administration 演示视频](https://www.youtube.com/watch?v=xnBrbkY9P20)
Just Enough Administration 演示演练。                                                                                                                  
## 凭据保护

### [使用 Credential Guard 保护派生的域凭据](https://docs.microsoft.com/windows/security/identity-protection/credential-guard/credential-guard)
Credential Guard 使用基于虚拟化的安全性来隔离密钥，只有特权系统软件可以访问它们。 在未授权的情况下访问这些密钥将导致凭据盗窃攻击，例如哈希传递或票证传递。 Credential Guard 通过保护 NTLM 密码哈希和 Kerberos 票证授予票证来防止这些攻击。

### [使用远程 Credential Guard 来保护远程桌面凭据](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard)
远程 Credential Guard 可通过将 Kerberos 请求重定向回请求连接的设备，通过远程桌面连接帮助保护凭据。 它还提供远程桌面会话的单一登录体验。                                                                                                        |
### [Credential Guard 演示视频](https://www.youtube.com/watch?v=eUpKOGSl7yk)
这段时长 5 分钟的视频演示了 Credential Guard 和远程 Credential Guard。         

## 强化的操作系统和应用程序
### [Windows Defender 应用程序控制 (WDAC) 部署指南](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control)
WDAC 是可配置代码完整性 (CI) 策略，有助于企业控制其环境中运行的应用程序，但除了运行 Windows 10 之外没有具体的硬件或软件要求。

### [Device Guard 演示视频](https://www.youtube.com/watch?v=F-pTkesjkhI)
Device Guard 是 WDAC 和虚拟机监控程序保护的代码完整性 (HVCI) 的组合。 这段时长 7 分钟的视频展示了 Device Guard 及其在 Windows Server 上的使用。

### [传输层安全性注册表设置](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings)
支持用于传输层安全性 (TLS) 协议和安全套接字层 (SSL) 协议的 Windows 实现的注册表设置信息。

### [控制流防护](https://docs.microsoft.com/windows/desktop/SecBP/control-flow-guard)
控制流防护针对一些类的内存损坏攻击提供内置保护。

### [Windows Defender](https://technet.microsoft.com/windows-server-docs/security/windows-defender/windows-defender-overview-windows-server)
Windows Defender 提供阻止已知恶意软件的活动检测功能。 Windows Defender 在默认情况下启用，经过优化，可在 Windows Server 中支持各种服务器角色。

##检测和响应威胁
### [使用 Microsoft Operations Management Suite 的安全威胁分析](https://channel9.msdn.com/events/ignite/2015/brk3464)
此 Ignite 演示文稿讨论如何使用操作见解执行安全威胁分析。

### [Microsoft Operations Management Suite (OMS)](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx)
Microsoft Operations Management Suite (OMS) 安全和审核解决方案处理本地和云环境中的安全日志和防火墙事件以分析和检测恶意行为。

### [OMS 和 Windows Server](https://www.youtube.com/watch?v=_SaDw1dRy2k)
这段时长 3 分钟的视频演示 OMS 如何可以帮助检测 Windows Server 阻止的潜在的恶意行为。  

### [Microsoft 高级威胁分析](http://blogs.technet.com/b/ad/archive/2015/07/22/microsoft-advanced-threat-analytics-coming-next-month.aspx)
这篇博客文章讨论了 Microsoft 高级威胁分析，这是一种本地产品，可使用 Active Directory 网络流量和 SIEM 数据发现潜在威胁并发出警报。

### [Microsoft 高级威胁分析](https://www.youtube.com/watch?v=0nA9FeTRZFw&list=PL8nfc9haGeb5IZGM8HvmRozetHRpBDKSw)
这个 3 分钟的视频概述了 Microsoft 如何在 Windows Server 中添加威胁分析功能。                                                                                 |

## 网络安全性

### [数据中心防火墙概述](https://technet.microsoft.com/library/dn920240.aspx)
本概述介绍了数据中心防火墙、网络层、5 元组（协议、源端口号、目标端口号、源 IP 地址和目标 IP 地址）、有状态的多租户防火墙。

### [Windows Server 中 DNS 的新增功能](https://technet.microsoft.com/windows-server-docs/networking/dns/what-s-new-in-dns-server)
本概述主题简要描述了 DNS 中的新增功能以及详细信息的链接。                                                                           

## 将安全功能映射到合规性法规

合规性是安全功能的一个重要方面。 对于如何实现合规性以及对受信任的合规性顾问而言合规性是什么，我们同意专家就此提出的建议，但我们也希望提供初始映射，以便在评估 Windows Server 时能够使用。

-   [Hyper-V 受防护的 VM 合规性映射白皮书](https://download.microsoft.com/download/6/D/0/6D06E149-B4C1-4EED-ACD5-DF6066E93CC0/Coalfire_Branded_Hyper_V_Shielded_VMs_Whitepaper_EN_US.pdf)

-   [JEA 和 JIT 合规性映射白皮书](https://download.microsoft.com/download/2/7/A/27A2B5BB-6B52-4482-87C1-DA9D6B6D8C8D/Coalfire_Branded_Privileged_Identity_Manager_Whitepaper_EN_US.pdf)

-   [Device Guard 合规性映射白皮书](https://download.microsoft.com/download/6/9/D/69D9E610-D23C-4F7E-A8CC-D65B87CEB4F8/Coalfire_Branded_Device_Guard_Whitepaper_EN_US.pdf)

-   [Credential Guard 合规性映射白皮书](https://download.microsoft.com/download/8/1/2/812009C9-E4B8-4D4B-AADD-FDC373D0A076/Coalfire_Branded_Credential_Guard_Whitepaper_EN_US.pdf)

-   [Windows Defender 合规性映射白皮书](https://download.microsoft.com/download/C/7/7/C778B7BB-0783-42D7-93A9-B86DFB5A7BAD/Coalfire_Branded_Windows_Defender_Whitepaper_EN_US.pdf)

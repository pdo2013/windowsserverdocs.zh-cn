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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827218"
---
# <a name="security-and-assurance-in-windows-server"></a>Windows Server 中的安全和保障 

>适用于：Windows 服务器 （半年频道），Windows Server 2016

>[!TIP]
> 要查找有关较旧版 Windows Server 的信息？ 在 docs.microsoft.com 上查看我们的其他 [Windows Server 库](/previous-versions/windows/)。 也可以[搜索此站点](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)了解具体信息。

<img src="../media/landing-icons/security.png" style='float:left; padding:.5em;' alt="Icon representing a lock"> 你可以依靠内置于操作系统的新保护层进一步防止出现安全漏洞。 帮助阻止恶意攻击并提高虚拟机、应用程序和数据的安全性。


### <a name="windows-server-security-blog-posthttpsblogstechnetmicrosoftcomwindowsserver20160425ten-reasons-youll-love-windows-server-2016-8-security"></a>[Windows Server 安全博客文章](https://blogs.technet.microsoft.com/windowsserver/2016/04/25/ten-reasons-youll-love-windows-server-2016-8-security/)
Windows Server 安全团队的这篇博客文章重点介绍了 Windows Server 中可以提高托管和混合云环境的安全的许多改进。

### <a name="datacenter-and-private-cloud-security-bloghttpsblogstechnetmicrosoftcomdatacentersecurity"></a>[数据中心和私有云安全博客](https://blogs.technet.microsoft.com/datacentersecurity/)
这是来自 Microsoft 数据中心和私有云安全团队的技术内容的中心博客站点。                                    

### <a name="addressing-emerging-threats-and-landscape-shiftshttpswwwyoutubecomwatchvb5jmyxywx1kfeatureyoutube"></a>[应对新兴威胁和横向转移](https://www.youtube.com/watch?v=B5JMYxYWx1k&feature=youtu.be)
在这段时长 6 分钟的视频中，Anders Vinberg 概述了 Microsoft 的安全和保障策略，并讨论了与安全相关的行业趋势和横向转移。 随后重点讨论了通过基础构造保护工作负荷，以及防止从特权帐户发起的直接攻击等 Microsoft 主要计划。 最后，他介绍了在出现漏洞的情况下，如何利用新的检测和取证功能来帮助更好地识别威胁。

### <a name="protecting-your-datacenter-and-cloud-from-emerging-threats-blog-posthttpblogstechnetcombwindowsserverarchive20151118protecting-your-datacenter-and-cloud-november-updateaspx"></a>[保护数据中心和云免受新兴威胁博客文章](http://blogs.technet.com/b/windowsserver/archive/2015/11/18/protecting-your-datacenter-and-cloud-november-update.aspx)
这篇博客文章讨论了如何使用 Microsoft 技术保护你的数据中心和云投资免受新兴威胁。                   

### <a name="security-and-assurance-overview-session-at-ignitehttpchannel9msdncomeventsignite2015brk2482"></a>[Ignite 上的安全和保障概述会话](http://channel9.msdn.com/events/ignite/2015/brk2482)
此 Ignite 会话解决了持续威胁、内部违规、有组织的网络犯罪以及保护 Microsoft 云平台（本地服务以及使用 Azure 的连接的服务）。 它包括用于保护工作负荷、大型企业租户和服务提供商的方案。                                                                   

## <a name="secure-virtualization-with-shielded-vms"></a>使用受防护的 VM 保障虚拟化

### <a name="shielded-vm-in-channel-9httpchannel9msdncomshowsmechanicsintroduction-to-shielded-virtual-machines-in-windows-server-2016"></a>[在第 9 频道中受防护的 VM](http://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
受防护的虚拟机技术演练和权益。                           

### <a name="shielded-vm-demohttpswwwyoutubecomwatchvxip5qtk-7d8"></a>[受防护的 VM 演示](https://www.youtube.com/watch?v=xip5Qtk-7d8)
这段时长 4 分钟的视频介绍了受防护的 VM 的价值以及受防护的 VM 和未受防护的 VM 之间的区别。                                   

### <a name="shielded-virtual-machines-in-windows-server-video-walkthroughhttpmicrosoft-cloudcloudguidescomguidesshielded-virtual-machines-in-windows-serverhtm"></a>[受防护的虚拟机在 Windows Server 视频演练](http://microsoft-cloud.cloudguides.com/Guides/Shielded Virtual Machines in Windows Server.htm)
本视频演练演示主机保护者服务如何启用受防护的虚拟机，以便防止 Hyper-V 主机管理员对敏感数据进行未授权的访问。

### <a name="harden-the-fabric-protecting-tenant-secrets-in-hyper-v-ignite-videohttpchannel9msdncomeventsignite2015brk3457"></a>[增强构造：保护的 HYPER-V 中的租户密钥 (Ignite 视频)](http://channel9.msdn.com/events/ignite/2015/brk3457)

此 Ignite 演示文稿讨论了 Hyper-V 中的改进、Virtual Machine Manager 以及启用受防护的虚拟机的新主机保护者服务器角色。                

### <a name="guarded-fabric-deployment-guidehttpsdocsmicrosoftcomwindows-servervirtualizationguarded-fabric-shielded-vmguarded-fabric-deploying-hgs-overview"></a>[受保护的构造部署指南](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview)
本指南提供用于受保护的构造主机和受防护的 VM 的 Windows Server 和 System Center Virtual Machine Manager 的安装和验证信息。

### <a name="shielded-vm-and-guarded-fabric-in-branch-officeshttpsdocsmicrosoftcomwindows-servervirtualizationguarded-fabric-shielded-vmguarded-fabric-manage-branch-office"></a>[受防护的 VM 和分支机构中受保护的构造](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office)
本指南提供在分支机构和其他远程场景（Hyper-V 主机在一段时间内与 HGS 的连接受限）中运行受防护的虚拟机的最佳实践。

### <a name="shielded-vm-and-guarded-fabric-troubleshooting-guidehttpsdocsmicrosoftcomwindows-servervirtualizationguarded-fabric-shielded-vmguarded-fabric-troubleshoot-overview"></a>[受防护的 VM 和受保护的构造故障排除指南](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-overview)
本指南提供有关如何解决在受防护的 VM 环境中可能遇到的问题的信息。

### <a name="shielded-vm-articlehttpwindowsitprocomhyper-vsuper-secure-hyper-v-environments-shielded-vms-2016"></a>[受防护的 VM 文章](http://windowsitpro.com/hyper-v/super-secure-hyper-v-environments-shielded-vms-2016)
本白皮书概述了受防护的 VM 如何提供增强的总体安全性以防止篡改。                                         

## <a name="privileged-access-management"></a>保护 Windows 和 Microsoft Azure Active Directory
### <a name="securing-privileged-accesshttpstechnetmicrosoftcomwindows-server-docssecuritysecuring-privileged-accesssecuring-privileged-access"></a>[保护特权的访问](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access)
有关如何保护特权访问的道路地图。 此道路地图基于服务器安全团队、Microsoft IT、Azure 团队和 Microsoft 咨询服务部门的综合专业知识建立而成                           

### <a name="just-in-time-administration-with-microsoft-identity-managerhttpstechnetmicrosoftcomlibrarymt150258aspx"></a>[进行实时管理与 Microsoft 标识管理器](https://technet.microsoft.com/library/mt150258.aspx)
本文讨论 Microsoft Identity Manager 中所包含的特性和功能，包括对实时 (JIT) 特权访问管理的支持。                                                                    

### <a name="protecting-windows-and-microsoft-azure-active-directory-with-privileged-access-managementhttpchannel9msdncomeventsignite2015brk3873"></a>[保护 Windows 和 Microsoft Azure Active Directory 与特权的访问管理](http://channel9.msdn.com/events/ignite/2015/brk3873)
此 Ignite 演示文稿介绍用于解决通过更强的身份验证进行管理员访问的风险，以及使用实时和 Just Enough Administration (JEA) 管理访问的 Windows Server、PowerShell、Active Directory、Identity Manager 和 Azure Active Directory 中的 Microsoft 策略和投资。

### <a name="just-enough-administration-articlehttpakamsjea"></a>[Just Enough Administration 文章](http://aka.ms/JEA)
本文档分享 Just Enough Administration 的愿景和技术详细信息，这是一个 PowerShell 工具包，旨在帮助组织通过限制操作员仅具有执行特定任务所需的访问权限来降低风险。

### <a name="just-enough-administration-demo-videohttpswwwyoutubecomwatchvxnbrbky9p20"></a>[只需 Enough Administration 演示视频](https://www.youtube.com/watch?v=xnBrbkY9P20)
Just Enough Administration 演示演练。                                                                                                                  
## <a name="credential-protection"></a>凭据保护

### <a name="protect-derived-domain-credentials-with-credential-guardhttpsdocsmicrosoftcomwindowssecurityidentity-protectioncredential-guardcredential-guard"></a>[使用 Credential Guard 保护派生的域凭据](https://docs.microsoft.com/windows/security/identity-protection/credential-guard/credential-guard)
凭据保护使用基于虚拟化的安全性来隔离密钥，以便只有特权系统软件可以访问它们。 在未授权的情况下访问这些密钥将导致凭据盗窃攻击，例如哈希传递或票证传递。 Credential Guard 通过保护 NTLM 密码哈希和 Kerberos 票证授予票证来防止这些攻击。

### <a name="protect-remote-desktop-credentials-with-remote-credential-guardhttpsdocsmicrosoftcomwindowssecurityidentity-protectionremote-credential-guard"></a>[保护远程 Credential Guard 与远程桌面凭据](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard)
远程凭据保护可通过将 Kerberos 请求重定向回请求连接的设备，通过远程桌面连接帮助保护凭据。 它还提供远程桌面会话的单一登录体验。                                                                                                        |
### <a name="credential-guard-demo-videohttpswwwyoutubecomwatchveupkogsl7yk"></a>[Credential Guard 演示视频](https://www.youtube.com/watch?v=eUpKOGSl7yk)
这段时长 5 分钟的视频演示了 Credential Guard 和远程 Credential Guard。         

## <a name="hardening-the-os-and-applications"></a>强化的操作系统和应用程序
### <a name="windows-defender-application-control-wdac-deployment-guidehttpsdocsmicrosoftcomwindowssecuritythreat-protectionwindows-defender-application-controlwindows-defender-application-control"></a>[Windows Defender 应用程序控制 (WDAC) 部署指南](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control)
WDAC 是可配置代码完整性 (CI) 策略，有助于企业控制其环境中运行的应用程序，但除了运行 Windows 10 之外没有具体的硬件或软件要求。

### <a name="device-guard-demo-videohttpswwwyoutubecomwatchvf-ptkesjkhi"></a>[Device Guard 演示视频](https://www.youtube.com/watch?v=F-pTkesjkhI)
Device Guard 是 WDAC 和虚拟机监控程序保护的代码完整性 (HVCI) 的组合。 这段时长 7 分钟的视频展示了 Device Guard 及其在 Windows Server 上的使用。

### <a name="transport-layer-security-registry-settingshttpsdocsmicrosoftcomwindows-serversecuritytlstls-registry-settings"></a>[传输层安全注册表设置](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings)
支持用于传输层安全性 (TLS) 协议和安全套接字层 (SSL) 协议的 Windows 实现的注册表设置信息。

### <a name="control-flow-guardhttpsdocsmicrosoftcomwindowsdesktopsecbpcontrol-flow-guard"></a>[控制流防护](https://docs.microsoft.com/windows/desktop/SecBP/control-flow-guard)
控制流防护针对一些类的内存损坏攻击提供内置保护。

### <a name="windows-defenderhttpstechnetmicrosoftcomwindows-server-docssecuritywindows-defenderwindows-defender-overview-windows-server"></a>[Windows Defender](https://technet.microsoft.com/windows-server-docs/security/windows-defender/windows-defender-overview-windows-server)
Windows Defender 提供阻止已知恶意软件的活动检测功能。 Windows Defender 在默认情况下启用，经过优化，可在 Windows Server 中支持各种服务器角色。

##<a name="detecting-and-responding-to-threats"></a>检测和响应威胁
### <a name="security-threat-analysis-using-microsoft-operations-management-suitehttpschannel9msdncomeventsignite2015brk3464"></a>[使用 Microsoft Operations Management Suite 的安全威胁分析](https://channel9.msdn.com/events/ignite/2015/brk3464)
此 Ignite 演示文稿讨论如何使用操作见解执行安全威胁分析。

### <a name="microsoft-operations-management-suite-omshttpswwwmicrosoftcomen-usserver-cloudoperations-management-suiteoverviewaspx"></a>[Microsoft Operations Management Suite (OMS)](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx)
Microsoft Operations Management Suite (OMS) 安全和审核解决方案处理本地和云环境中的安全日志和防火墙事件以分析和检测恶意行为。

### <a name="oms-and-windows-serverhttpswwwyoutubecomwatchvsadw1dry2k"></a>[OMS 和 Windows Server](https://www.youtube.com/watch?v=_SaDw1dRy2k)
这段时长 3 分钟的视频演示 OMS 如何可以帮助检测 Windows Server 阻止的潜在的恶意行为。  

### <a name="microsoft-advanced-threat-analyticshttpblogstechnetcombadarchive20150722microsoft-advanced-threat-analytics-coming-next-monthaspx"></a>[Microsoft 高级威胁分析](http://blogs.technet.com/b/ad/archive/2015/07/22/microsoft-advanced-threat-analytics-coming-next-month.aspx)
这篇博客文章讨论了 Microsoft 高级威胁分析，这是一种本地产品，可使用 Active Directory 网络流量和 SIEM 数据发现潜在威胁并发出警报。

### <a name="microsoft-advanced-threat-analyticshttpswwwyoutubecomwatchv0na9fetrzfwlistpl8nfc9hageb5izgm8hvmrozethrpbdksw"></a>[Microsoft 高级威胁分析](https://www.youtube.com/watch?v=0nA9FeTRZFw&list=PL8nfc9haGeb5IZGM8HvmRozetHRpBDKSw)
这个 3 分钟的视频概述了 Microsoft 如何在 Windows Server 中添加威胁分析功能。                                                                                 |

## <a name="network-security"></a>网络安全性

### <a name="datacenter-firewall-overviewhttpstechnetmicrosoftcomlibrarydn920240aspx"></a>[数据中心防火墙概述](https://technet.microsoft.com/library/dn920240.aspx)
本概述介绍了数据中心防火墙、网络层、5 元组（协议、源端口号、目标端口号、源 IP 地址和目标 IP 地址）、有状态的多租户防火墙。

### <a name="whats-new-in-dns-in-windows-serverhttpstechnetmicrosoftcomwindows-server-docsnetworkingdnswhat-s-new-in-dns-server"></a>[什么是 Windows Server 中的 DNS 中的新增功能](https://technet.microsoft.com/windows-server-docs/networking/dns/what-s-new-in-dns-server)
本概述主题简要描述了 DNS 中的新增功能以及详细信息的链接。                                                                           

## <a name="mapping-security-features-to-compliance-regulations"></a>将安全功能映射到合规性法规

合规性是安全功能的一个重要方面。 对于如何实现合规性以及对受信任的合规性顾问而言合规性是什么，我们同意专家就此提出的建议，但我们也希望提供初始映射，以便在评估 Windows Server 时能够使用。

-   [HYPER-V 受防护的 Vm 合规性映射白皮书](https://download.microsoft.com/download/6/D/0/6D06E149-B4C1-4EED-ACD5-DF6066E93CC0/Coalfire_Branded_Hyper_V_Shielded_VMs_Whitepaper_EN_US.pdf)

-   [JEA 和 JIT 合规性映射白皮书](https://download.microsoft.com/download/2/7/A/27A2B5BB-6B52-4482-87C1-DA9D6B6D8C8D/Coalfire_Branded_Privileged_Identity_Manager_Whitepaper_EN_US.pdf)

-   [设备防护合规性映射白皮书](https://download.microsoft.com/download/6/9/D/69D9E610-D23C-4F7E-A8CC-D65B87CEB4F8/Coalfire_Branded_Device_Guard_Whitepaper_EN_US.pdf)

-   [Credential Guard 合规性映射白皮书](https://download.microsoft.com/download/8/1/2/812009C9-E4B8-4D4B-AADD-FDC373D0A076/Coalfire_Branded_Credential_Guard_Whitepaper_EN_US.pdf)

-   [Windows Defender 合规性映射白皮书](https://download.microsoft.com/download/C/7/7/C778B7BB-0783-42D7-93A9-B86DFB5A7BAD/Coalfire_Branded_Windows_Defender_Whitepaper_EN_US.pdf)

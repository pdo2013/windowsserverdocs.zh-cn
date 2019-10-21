---
title: Windows Server 疑难解答
description: 获取有关 Windows Server 问题疑难解答文章的链接
layout: LandingPage
ms.prod: windows-server
ms.service: na
manager: ''
ms.technology: server-general
ms.date: 10/17/2019
ms.topic: landing-page
author: kaushika-msft
ms.author: kaushika
ms.openlocfilehash: 0d593ab28234d5abfa0b5d2931eb7d2c958efe2a
ms.sourcegitcommit: b7f55949f166554614f581c9ddcef5a82fa00625
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2019
ms.locfileid: "72588080"
---
# <a name="troubleshooting-windows-server-components"></a>Windows Server 组件疑难解答

>- [!TIP]
> 要查找有关较旧版 Windows Server 的信息？ 查看 docs.microsoft.com 上的其他[Windows Server 库](/previous-versions/windows/)。 你还可以在[此站点上搜索](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)特定信息。

Microsoft 定期为 Windows Server 发布这两个更新。 若要确保你的服务器能够收到将来的更新（包括安全更新），请务必保持更新服务器。 有关已发布更新的完整列表，请查看- [Windows 10 和 Windows Server 2016 更新历史记录](https://support.microsoft.com/en-us/help/4000825/windows-10-windows-server-2016-update-history)。

本部分包含高级疑难解答主题和链接，以帮助你解决 Windows Server 问题。 其他主题将在可用时添加。

### <a name="troubleshoot-activation"></a>排查激活问题
- [排查 Windows 批量激活问题](https://docs.microsoft.com/windows-server/get-started/activation-troubleshooting-guide)
- [KMS 故障排除指南](https://docs.microsoft.com/windows-server/get-started/activation-troubleshoot-kms-general)
- [用于获取批量激活信息的 Slmgr.vbs 选项](https://docs.microsoft.com/windows-server/get-started/activation-slmgr-vbs-options)
- [根据 Windows 激活错误代码解决问题](https://docs.microsoft.com/windows-server/get-started/activation-error-codes)
- [KMS 激活已知问题](https://docs.microsoft.com/windows-server/get-started/activation-troubleshoot-kms-issues)
- [MAK 激活已知问题](https://docs.microsoft.com/windows-server/get-started/activation-troubleshoot-mak-issues)
- [用于排查 DNS 相关激活问题的指南](https://docs.microsoft.com/windows-server/get-started/common-troubleshooting-procedures-kms-dns)
- [重新生成 Tokens.dat 文件](https://docs.microsoft.com/windows-server/get-started/activation-rebuild-tokens-dat-file)
- [ADBA 客户端疑难解答](https://docs.microsoft.com/windows-server/get-started/activation-troubleshoot-adba-clients)

### <a name="troubleshoot-ad-forest-recovery"></a>排查 AD 林恢复问题
- [AD 林恢复 - 常见问题解答](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/ad-forest-recovery-faq)

### <a name="troubleshoot-ad-replication"></a>排查 AD 复制问题
- [Active Directory 复制问题疑难解答](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/troubleshoot/troubleshooting-active-directory-replication-problems)
- [虚拟化域控制器疑难解答](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/virtual-dc/virtualized-domain-controller-troubleshooting)
- [域控制器部署疑难解答](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/troubleshooting-domain-controller-deployment)
- [配置计算机以进行故障排除](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/troubleshoot/configuring-a-computer-for-troubleshooting)

### <a name="troubleshoot-ad-fs"></a>排查 AD FS
- [疑难解答 AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-overview)
- [AD FS 故障排除-审核事件和日志记录](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-logging)
- [AD FS 疑难解答-SQL 连接](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-sql)
- [AD FS 故障排除-声明颁发](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-claims-issuance)
- [AD FS 故障排除-循环检测](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-loop)
- [AD FS 故障排除-证书](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-certs)
- [AD FS 故障排除-Fiddler](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-fiddler)
- [AD FS 疑难解答-Fiddler-ws-federation](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-fiddler-ws-fed)
- [AD FS 故障排除-声明规则](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-claims-rules)
- [AD FS 故障排除-Windows 集成身份验证](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-iwa)
- [AD FS 故障排除-Azure AD](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-azure)
- [AD FS 常见问题](https://docs.microsoft.com/windows-server/identity/ad-fs/overview/ad-fs-faq)
- [AD FS 帮助诊断分析器](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-diagnostics-analyzer)

### <a name="troubleshoot-aovpn"></a>AoVPN 故障排除
- [始终启用 VPN 疑难解答](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy-troubleshooting)

### <a name="troubleshoot-converged-nic"></a>聚合 NIC 故障排除
- [聚合 NIC 配置疑难解答](https://docs.microsoft.com/windows-server/networking/technologies/conv-nic/cnic-app-troubleshoot)

### <a name="troubleshoot-dfsr"></a>DFSR 疑难解答
- [DFS 复制：常见问题（FAQ）](https://docs.microsoft.com/windows-server/storage/dfs-replication/dfsr-faq)

### <a name="troubleshoot-directaccess"></a>排查 DirectAccess 问题
- [DirectAccess 疑难解答](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/troubleshooting-directaccess)

### <a name="troubleshoot-disk--management"></a>磁盘管理疑难解答
- [磁盘管理疑难解答](https://docs.microsoft.com/windows-server/storage/disk-management/troubleshooting-disk-management)

### <a name="troubleshoot-dns"></a>DNS 疑难解答
- [域名系统（DNS）问题疑难解答](https://docs.microsoft.com/windows-server/networking/dns/troubleshoot/troubleshoot-dns-data-collection)
- [排查 DNS 客户端问题](https://docs.microsoft.com/windows-server/networking/dns/troubleshoot/troubleshoot-dns-client)
- [在 DNS 客户端上禁用 DNS 客户端缓存](https://docs.microsoft.com/windows-server/networking/dns/troubleshoot/disable-dns-client-side-caching)
- [DNS 服务器疑难解答](https://docs.microsoft.com/windows-server/networking/dns/troubleshoot/troubleshoot-dns-server)

### <a name="troubleshoot-failover-cluster"></a>故障转移群集疑难解答
- [使用 Windows 错误报告排查故障转移群集问题](https://docs.microsoft.com/windows-server/failover-clustering/troubleshooting-using-wer-reports)
- [群集感知更新-常见问题](https://docs.microsoft.com/windows-server/failover-clustering/cluster-aware-updating-faq)

### <a name="troubleshoot-fsrm"></a>排查 FSRM 问题
- [文件服务器资源管理器疑难解答](https://docs.microsoft.com/windows-server/storage/fsrm/troubleshooting-file-server-resource-manager)

### <a name="troubleshoot-guarded-fabric"></a>排除受保护的结构
- [使用受保护的构造诊断工具进行故障排除](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-diagnostics)
- [主机保护者服务疑难解答](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hgs)
- [主机保护者服务疑难解答](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hosts)

### <a name="troubleshoot-multi-site-ras"></a>排查多站点 RAS 问题
- [启用多站点疑难解答](https://docs.microsoft.com/windows-server/remote/remote-access/ras/multisite/troubleshoot/troubleshooting-enabling-multisite)
- [添加入口点疑难解答](https://docs.microsoft.com/windows-server/remote/remote-access/ras/multisite/troubleshoot/troubleshooting-adding-entry-points)
- [设置入口点域控制器疑难解答](https://docs.microsoft.com/windows-server/remote/remote-access/ras/multisite/troubleshoot/troubleshooting-setting-the-entry-point-domain-controller)
- [Web 探测 URL 疑难解答](https://docs.microsoft.com/windows-server/remote/remote-access/ras/multisite/troubleshoot/troubleshooting-web-probe-urls)

### <a name="troubleshoot-nano-server"></a>Nano Server 疑难解答
- [Nano Server 疑难解答](https://docs.microsoft.com/windows-server/get-started/troubleshooting-nano-server)

### <a name="troubleshoot-nic-teaming"></a>排查 NIC 组合问题
- [NIC 组合疑难解答](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/troubleshooting-nic-teaming)

### <a name="troubleshoot-otp-authentication"></a>OTP 身份验证疑难解答
- [身份验证问题疑难解答](https://docs.microsoft.com/windows-server/remote/remote-access/ras/otp/troubleshoot/troubleshooting-authentication-issues)
- [启用 OTP 疑难解答](https://docs.microsoft.com/windows-server/remote/remote-access/ras/otp/troubleshoot/troubleshooting-enabling-otp)

### <a name="troubleshoot-qos"></a>排查 QoS 问题
- [QoS 常见问题](https://docs.microsoft.com/windows-server/networking/technologies/qos/qos-policy-faq)

### <a name="troubleshoot-s2d"></a>S2D 疑难解答
- [存储空间直通疑难解答](https://docs.microsoft.com/windows-server/storage/storage-spaces/troubleshooting-storage-spaces)
- [存储空间直通-常见问题](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-faq)
- [存储空间直通运行状况和运行状态](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-states)
- [通过存储空间直通收集诊断数据](https://docs.microsoft.com/windows-server/storage/storage-spaces/data-collection)
- [Windows 中的存储类内存（NVDIMM-N）运行状况管理](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-class-memory-health)

### <a name="troubleshoot-sdn"></a>SDN 疑难解答
- [SDN 疑难解答](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-software-defined-networking)
- [排查 Windows Server 软件定义网络堆栈问题](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack)

### <a name="troubleshoot-rds-session-connectivity"></a>对 RDS 会话连接进行故障排除
- [常见远程桌面连接故障排除](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/troubleshoot/rdp-error-general-troubleshooting)
- [客户端无法连接，无法获取类未注册错误](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/troubleshoot/rdp-error-class-not-registered)
- [客户端无法连接和查看无可用许可证错误](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/troubleshoot/rdp-error-no-licenses-available)
- [用户无法完成身份验证或必须完成身份验证两次](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/troubleshoot/cannot-authenticate-or-must-authenticate-twice)
- [连接时，用户接收远程桌面服务当前正忙消息](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/troubleshoot/remote-desktop-service-currently-busy)
- [远程桌面客户端断开连接且无法重新连接到同一会话](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/troubleshoot/rdp-client-disconnects-cannot-reconnect-same-session)
- [远程笔记本电脑从无线网络断开连接](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/troubleshoot/remote-laptop-disconnects-wireless-network)
- [远程桌面连接期间出现性能低下或应用程序问题](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/troubleshoot/poor-performance-or-application-problems)

### <a name="troubleshoot-shielded-vm"></a>排查受防护的 VM
- [排查受防护的 Vm](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms)

### <a name="troubleshoot-software-restriction-policies"></a>软件限制策略疑难解答
- [软件限制策略疑难解答](https://docs.microsoft.com/windows-server/identity/software-restriction-policies/troubleshoot-software-restriction-policies)

### <a name="troubleshoot-storage-migration"></a>存储迁移故障排除
- [存储迁移服务的已知问题](https://docs.microsoft.com/windows-server/storage/storage-migration-service/known-issues)
- [存储迁移服务常见问题（FAQ）](https://docs.microsoft.com/windows-server/storage/storage-migration-service/faq)

### <a name="troubleshoot-storage-replica"></a>存储副本故障排除
- [存储副本的已知问题](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-known-issues)
- [有关存储副本的常见问题](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-frequently-asked-questions)

### <a name="troubleshoot-user-profiles"></a>用户配置文件疑难解答
- [对包含事件的用户配置文件进行故障排除](https://docs.microsoft.com/windows-server/storage/folder-redirection/troubleshoot-user-profiles-events)

### <a name="troubleshoot-vrss"></a>VRSS 疑难解答
- [vRSS 常见问题](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-faq)

### <a name="troubleshoot-webproxy"></a>WebProxy 故障排除
- [Web 应用程序代理疑难解答](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/troubleshooting-web-application-proxy)

### <a name="troubleshoot-windows-admin-center"></a>排查 Windows Admin Center 问题
- [Windows 管理中心常见的故障排除步骤](https://docs.microsoft.com/windows-server/manage/windows-admin-center/support/troubleshooting)
- [Windows 管理中心的已知问题](https://docs.microsoft.com/windows-server/manage/windows-admin-center/support/known-issues)
- [Windows 管理中心的常见问题](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/faq)

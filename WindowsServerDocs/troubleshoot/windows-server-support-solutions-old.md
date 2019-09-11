---
title: 适用于 Windows Server 的顶级支持解决方案
description: 获取适用于 Windows Server 问题的解决方案的链接
ms.prod: windows-server-threshold
ms.service: na
manager: alant
ms.technology: server-general
ms.date: 03/16/2018
ms.topic: article
author: kaushika-msft
ms.author: elizapo
ms.openlocfilehash: 29dd518ce7634a1cf8b3b7a84d8c7a4388d6027f
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866979"
---
# <a name="top-support-solutions-for-windows-server-2016"></a>适用于 Windows Server 2016 的顶级支持解决方案

Microsoft 会定期为 Windows Server 发布更新和解决方案。 若要确保你的服务器能够收到将来的更新（包括安全更新），请务必保持更新服务器。 有关已发布更新的完整列表，请查看 [Windows 10 和 Windows Server 2016 更新历史记录](https://support.microsoft.com/en-us/help/4000825/windows-10-windows-server-2016-update-history)。

以下是解决使用 Windows Server 2016 时遇到的最常见问题的顶级 Microsoft 支持解决方案。 下面的链接包含指向知识库文章、更新和库文章的链接。

>[!TIP]
> 要查找有关较旧版 Windows Server 的信息？ 在 docs.microsoft.com 上查看我们的其他 [Windows Server 库](/previous-versions/windows/)。 也可以[搜索此站点](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)了解具体信息。

## <a name="solutions-for-installing-or-upgrading-windows-server"></a>用于安装或升级 Windows Server 的解决方案

- [解决 Windows 10 升级错误：适用于 IT 专业人员的技术信息](https://docs.microsoft.com/windows/deployment/upgrade/resolve-windows-10-upgrade-errors)
- [Windows 10 版本1607和 Windows Server 2016 的服务堆栈更新：2017年8月8日](https://support.microsoft.com/en-US/help/4035631)
- [用于升级到 Windows 10 版本1607和 Windows Server 2016 的兼容性更新：2017年8月3日](https://support.microsoft.com/en-US/help/4033524)
- [基于 Windows 的 Azure Vm 不支持就地系统升级](https://support.microsoft.com/en-US/help/4014997)
- [Windows Server 2016 的升级和转换选项](../get-started/supported-upgrade-paths.md)
- [Windows Server 2016 的服务器角色升级和迁移矩阵](../get-started/server-role-upgradeability-table.md)
- [Windows Server 安装和升级](../get-started/installation-and-upgrade.md)
- [发行说明：Windows Server 2016 中的重要问题](../get-started/windows-server-2016-ga-release-notes.md)
- [有关移动到 Windows Server 2016 的建议](../get-started/recommendations-moving-to-server2016.md)

## <a name="solutions-for-volume-activation"></a>批量激活解决方案
- [Windows Server 2016 激活](../get-started/server-2016-activation.md)
- [查看并选择激活方法](https://technet.microsoft.com/library/jj134256(ws.11).aspx)
- [批量激活的激活错误代码](https://technet.microsoft.com/library/dn502528.aspx)
- [如何对密钥管理服务（KMS）进行故障排除](https://technet.microsoft.com/library/ee939272.aspx)
- [批量激活故障排除](https://technet.microsoft.com/library/ff793439.aspx)
- [激活错误代码](https://technet.microsoft.com/library/ff793399.aspx)
- [Windows 安装可能失败并出现错误 "输入的产品密钥与可用于安装的任何 Windows 映像都不匹配。输入其他产品密钥](https://support.microsoft.com/help/2796988/windows-8-or-windows-server-2012-installation-may-fail-with-error-mess)

## <a name="solutions-related-to-dcpromo-and-installing-domain-controllers"></a>与 DCPromo 和安装域控制器相关的解决方案
- [Active Directory 和 Active Directory 域服务端口要求](https://technet.microsoft.com/library/dd772723(v=ws.10).aspx)
- [Active Directory 防火墙端口–我们尝试简单地](http://blogs.msmvps.com/acefekay/2011/11/01/active-directory-firewall-ports-let-s-try-to-make-this-simple/)
- [Windows Server 2016 的 Exchange Server 支持](https://technet.microsoft.com/library/ff728623(v=exchg.150).aspx)
- [使用 Ntdsutil.exe 传输或占用 FSMO 角色到域控制器](https://support.microsoft.com/kb/255504)
- [域控制器部署疑难解答](../identity/ad-ds/deploy/troubleshooting-domain-controller-deployment.md)
- [Active Directory 安装向导问题的疑难解答](https://msdn.microsoft.com/library/bb727058.aspx)
- [用于安装和删除 AD DS 的已知问题](https://technet.microsoft.com/library/cc754463(v=ws.10).aspx)

## <a name="solutions-for-active-directory-federation-services-ad-fs"></a>Active Directory 联合身份验证服务 (AD FS) 解决方案
- [如何配置已加入 Windows 域的设备的自动注册 Azure Active Directory](/azure/active-directory/active-directory-conditional-access-automatic-device-registration-setup)
- [设置声明的颁发](/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup#step-2-setup-issuance-of-claims)
- [配置 AD FS 以对存储在 LDAP 目录中的用户进行身份验证](../identity/ad-fs/operations/configure-ad-fs-to-authenticate-users-stored-in-ldap-directories.md)
- [AD FS 对证书身份验证的备用主机名绑定的支持](../identity/ad-fs/operations/ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
- [防止密码攻击](https://blogs.technet.microsoft.com/tspring/2017/01/20/federated-to-microsoft-cloud-and-account-lockouts/)
- [使用 WID 数据库升级到 Windows Server 2016 中的 AD FS](../identity/ad-fs/deployment/upgrading-to-ad-fs-in-windows-server-2016.md)
- [Windows 10 登录–启用 AD FS 设备身份验证](../identity/ad-fs/operations/configure-device-based-conditional-access-on-premises.md)
- [在 Windows Server 2016 中的 AD FS 和 WAP 中管理 SSL 证书](../identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap-2016.md)
- [Windows Server 2016 中的访问控制策略 AD FS](../identity/ad-fs/operations/access-control-policies-in-ad-fs.md)

## <a name="solutions-related-to-active-directory-replication"></a>与 Active Directory 复制相关的解决方案

- [Active Directory 复制问题疑难解答](../identity/ad-ds/manage/troubleshoot/troubleshooting-active-directory-replication-problems.md)
- [从 Microsoft 下载中心下载 Active Directory 复制状态工具](https://www.microsoft.com/en-in/download/details.aspx?id=30005)
- [e2e如何排查常见 Active Directory 复制错误](https://support.microsoft.com/kb/3108513)
- [排查 AD 复制错误8606：提供的属性不足，无法创建对象](https://support.microsoft.com/kb/2028495)
- [在 Windows 2000 服务器和 Windows Server 2003 中 Active Directory 的入站复制期间出现事件 ID 2108 和事件 ID 1084](https://support.microsoft.com/kb/837932)
- [排查 AD 复制错误8451：复制操作遇到数据库错误](https://support.microsoft.com/kb/2645996)
- [排查 AD 复制错误1127：在访问硬盘时，磁盘操作即使在重试后也失败](https://support.microsoft.com/kb/2025726)
- [清理服务器元数据](https://technet.microsoft.com/library/cc816907.aspx)
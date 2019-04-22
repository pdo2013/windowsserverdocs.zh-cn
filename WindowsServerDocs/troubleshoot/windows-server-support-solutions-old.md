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
ms.openlocfilehash: b283eed38e991886fc72de0e8f8cdbc209972fa7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820518"
---
# <a name="top-support-solutions-for-windows-server-2016"></a>适用于 Windows Server 2016 的顶级支持解决方案

Microsoft 会定期为 Windows Server 发布更新和解决方案。 若要确保你的服务器能够收到将来的更新（包括安全更新），请务必保持更新服务器。 有关已发布更新的完整列表，请查看 [Windows 10 和 Windows Server 2016 更新历史记录](https://support.microsoft.com/en-us/help/4000825/windows-10-windows-server-2016-update-history)。

以下是解决使用 Windows Server 2016 时遇到的最常见问题的顶级 Microsoft 支持解决方案。 下面的链接包含指向知识库文章、更新和库文章的链接。

>[!TIP]
> 要查找有关较旧版 Windows Server 的信息？ 在 docs.microsoft.com 上查看我们的其他 [Windows Server 库](/previous-versions/windows/)。 也可以[搜索此站点](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)了解具体信息。

## <a name="solutions-for-installing-or-upgrading-windows-server"></a>用于安装或升级 Windows Server 的解决方案

- [解决 Windows 10 升级错误：面向 IT 专业人员的技术信息](\windows\deployment\upgrade\resolve-windows-10-upgrade-errors)
- [对于 Windows 10 版本 1607年和 Windows Server 2016 的服务堆栈更新：2017 年 8 月 8日日](https://support.microsoft.com/en-US/help/4035631)
- [升级到 Windows 10 版本 1607年和 Windows Server 2016 的兼容性更新：2017 年 8 月 3日日](https://support.microsoft.com/en-US/help/4033524)
- [在基于 Windows 的 Azure Vm 上不支持系统的就地升级](https://support.microsoft.com/en-US/help/4014997)
- [Windows Server 2016 的升级和转换选项](..\get-started\supported-upgrade-paths.md)
- [Windows Server 2016 的服务器角色升级和迁移矩阵](..\get-started\server-role-upgradeability-table.md)
- [Windows Server 安装和升级](..\get-started\installation-and-upgrade.md)
- [发行说明：Windows Server 2016 中的重要问题](..\get-started\windows-server-2016-ga-release-notes.md)
- [移动到 Windows Server 2016 的建议](..\get-started\recommendations-moving-to-server2016.md)

## <a name="solutions-for-volume-activation"></a>批量激活解决方案
- [Windows Server 2016 Activation](../get-started/server-2016-activation.md)
- [查看并选择激活方法](https://technet.microsoft.com/library/jj134256(ws.11).aspx)
- [批量激活的激活错误代码](https://technet.microsoft.com/library/dn502528.aspx)
- [如何解决密钥管理服务 (KMS)](https://technet.microsoft.com/library/ee939272.aspx)
- [卷激活故障排除](https://technet.microsoft.com/library/ff793439.aspx)
- [激活错误代码](https://technet.microsoft.com/library/ff793399.aspx)
- [Windows 安装可能会失败，错误"输入的产品密钥不匹配任何可用于安装的 Windows 映像。输入不同的产品密钥"](https://support.microsoft.com/help/2796988/windows-8-or-windows-server-2012-installation-may-fail-with-error-mess)

## <a name="solutions-related-to-dcpromo-and-installing-domain-controllers"></a>与 DCPromo 和安装域控制器相关的解决方案
- [Active Directory 和 Active Directory 域服务端口要求](https://technet.microsoft.com/library/dd772723(v=ws.10).aspx)
- [Active Directory 的防火墙端口 – 让我们试着将此简单](http://blogs.msmvps.com/acefekay/2011/11/01/active-directory-firewall-ports-let-s-try-to-make-this-simple/)
- [Windows Server 2016 的 Exchange Server 支持](https://technet.microsoft.com/library/ff728623(v=exchg.150).aspx)
- [使用 Ntdsutil.exe 转移或占用 FSMO 角色到域控制器](https://support.microsoft.com/kb/255504)
- [域控制器部署疑难解答](../identity/ad-ds/deploy/troubleshooting-domain-controller-deployment.md)
- [Active Directory 安装向导问题疑难解答](https://msdn.microsoft.com/library/bb727058.aspx)
- [安装和删除 AD DS 的已知的问题](https://technet.microsoft.com/library/cc754463(v=ws.10).aspx)

## <a name="solutions-for-active-directory-federation-services-ad-fs"></a>Active Directory 联合身份验证服务 (AD FS) 解决方案
- [如何使用 Azure Active Directory 配置的 Windows 已加入域的设备自动注册](/azure/active-directory/active-directory-conditional-access-automatic-device-registration-setup)
- [设置声明颁发](/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup#step-2-setup-issuance-of-claims)
- [配置 AD FS 进行身份验证 LDAP 目录中存储的用户](../identity/ad-fs/operations/configure-ad-fs-to-authenticate-users-stored-in-ldap-directories.md)
- [AD FS 支持的证书身份验证的备用主机名绑定](../identity/ad-fs/operations/ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
- [密码攻击](https://blogs.technet.microsoft.com/tspring/2017/01/20/federated-to-microsoft-cloud-and-account-lockouts/)
- [升级到 Windows Server 2016 使用 WID 数据库中的 AD FS](../identity/ad-fs/deployment/upgrading-to-ad-fs-in-windows-server-2016.md)
- [Windows 10 登录 – 启用设备身份验证使用 AD FS](../identity/ad-fs/operations/configure-device-based-conditional-access-on-premises.md)
- [在 AD FS 和 Windows Server 2016 中的 WAP 中管理 SSL 证书](../identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap-2016.md)
- [在 Windows Server 2016 AD FS 访问控制策略](../identity/ad-fs/operations/access-control-policies-in-ad-fs.md)

## <a name="solutions-related-to-active-directory-replication"></a>与 Active Directory 复制相关的解决方案

- [故障排除 Active Directory 复制问题](../identity/ad-ds/manage/troubleshoot/troubleshooting-active-directory-replication-problems.md)
- [从 Microsoft 下载中心下载 Active Directory 复制状态工具](https://www.microsoft.com/en-in/download/details.aspx?id=30005)
- [e2e:如何排查常见的 Active Directory 复制错误](https://support.microsoft.com/kb/3108513)
- [疑难解答 AD 复制错误 8606:提供的属性不足以创建对象](https://support.microsoft.com/kb/2028495)
- [在 Windows 2000 Server 和 Windows Server 2003 中的 Active Directory 的入站复制期间发生的事件 ID 2108 和事件 ID 1084](https://support.microsoft.com/kb/837932)
- [疑难解答 AD 复制错误 8451::复制操作遇到数据库错误](https://support.microsoft.com/kb/2645996)
- [疑难解答 AD 复制错误 1127年:访问硬盘，时磁盘操作重试后仍然失败](https://support.microsoft.com/kb/2025726)
- [清理服务器元数据](https://technet.microsoft.com/library/cc816907.aspx)
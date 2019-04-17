---
title: "Windows Server Essentials 最佳做法分析器 (BPA) 工具使用规则"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37e1dae7-586c-4dd7-bf83-7e14a9567c8f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c205bc8ff75bf64d4a13a7d799988c9d1ebe1a22
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="rules-used-by-the-windows-server-essentials-best-practices-analyzer-bpa-tool"></a>Windows Server Essentials 最佳做法分析器 (BPA) 工具使用规则

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本文介绍了由第 Essentials Windows Server 最佳做法分析器 (BPA) 的使用规则。 BPA 检查运行的 Windows Server Essentials 和展示了报告，描述问题并提供有关解决这些建议的服务器。 这些建议由 Windows Server Essentials 产品支持组织开发。  
  
## <a name="using-the-tool"></a>使用该工具  
 它时标准练习中，从 Windows Server 2011 Essentials、Windows 小型企业服务器 2011 年软件包或运行 BPA 目标服务器上之后你完成迁移的设置和数据, Windows Home Server 2011，于 Windows Server Essentials 迁移。 你可以随时从仪表板运行该工具。  
  
#### <a name="to-run-the--windows-server-essentials-bpa-on-the-server"></a>在服务器上运行 Windows Server Essentials BPA  
  
1.  登录到以 administrator 身份，服务器上，然后打开仪表板。  
  
2.  在仪表板中，单击**设备**选项卡。  
  
3.  在**服务器任务**窗格中，单击**最佳实践分析**。  
  
4.  检查每个 BPA 消息，并按照说明进行操作来解决问题。  
  
## <a name="rules-used-by-the-best-practices-analyzer"></a>最佳实践分析的使用规则  
  
### <a name="disable-ip-filtering"></a>禁用 IP 筛选  
 **问题：** IP 筛选当前启用服务器上。 必须禁用 IP 筛选。  
  
 **影响：**网络通信如果 IP 筛选处于启用状态，可能会被阻止。  
  
 **解决方法：**  
  
##### <a name="to-disable-ip-filtering"></a>若要禁用 IP 筛选  
  
1.  打开 regedit.exe 服务器上。  
  
2.  导航到 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters。  
  
3.  右键单击**EnableSecurityFilters**，然后单击**修改**。  
  
4.  在**编辑 DWORD（32 位）值**窗口中，更改**值数据**为零，字段，然后单击**确定**。  
  
5.  若要将更改应用，请重启服务器。  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-set-to-start-automatically-by-default"></a>分布式事务处理协调器 (MSDTC) 服务应设置为默认情况下自动启动。  
 **问题：** MSDTC 服务未配置为自动启动。  
  
 **影响：** MSDTC 服务可能会无法启动时自动启动的服务器。 如果停止该服务，SQL Server 或 COM 的某些功能可能会失败。 因此，使用 Microsoft SQL Server 或 COM 功能的应用程序可能无法正常工作。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-msdtc-service-to-start-automatically"></a>若要配置 MSDTC 服务，以便自动启动  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**分布式事务处理协调器**服务，，然后单击**属性**。  
  
3.  在**常规**选项卡上，更改**启动类型**到**（延迟启动）的自动**，然后单击**确定**。  
  
### <a name="the-netlogon-service-should-be-configured-to-start-automatically-by-default"></a>应配置 Netlogon 服务，默认情况下自动启动  
 **问题：** Netlogon 服务未配置为自动启动。  
  
 **影响：** Netlogon 服务可能会无法启动时自动启动的服务器。 如果停止该服务，服务器可能未进行身份验证用户和服务。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-netlogon-service-to-start-automatically"></a>若要配置 Netlogon 服务，以便自动启动  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**Netlogon**服务，，然后单击**属性**。  
  
3.  在**常规**选项卡上，更改**启动类型**到**自动**，然后单击**确定**。  
  
### <a name="the-dns-client-service-should-be-configured-to-start-automatically-by-default"></a>应配置 DNS 客户服务，默认情况下自动启动  
 **问题：** DNS 客户端服务未配置为自动启动。  
  
 **影响：** DNS 客户端服务可能会无法启动时自动启动的服务器。 如果停止该服务，服务器可能不能解决 DNS 名称。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-dns-client-service-to-start-automatically"></a>若要配置 DNS 客户端服务，以便自动启动  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**DNS 客户端**服务，，然后单击**属性**。  
  
3.  在**常规**选项卡上，更改**启动类型**到**自动**，然后单击**确定**。  
  
### <a name="the-dns-server-service-should-be-configured-to-start-automatically-by-default"></a>应配置 DNS 服务器服务，默认情况下自动启动  
 **问题：** DNS 服务器服务未配置为自动启动。  
  
 **影响：** DNS 服务器服务可能会无法启动时自动启动的服务器。 如果停止该服务，将不会 DNS 更新。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-dns-server-service-to-start-automatically"></a>若要配置 DNS 服务器服务自动启动  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**DNS 服务器**服务，，然后单击**属性**。  
  
3.  在**常规**选项卡上，更改**启动类型**到**自动**，然后单击**确定**。  
  
### <a name="active-directory-web-services-is-not-set-to-the-default-start-mode"></a>Active DirectoryWeb 服务未设置为默认开始模式  
 **问题：** Active Directory Web 服务未设置为自动默认开始模式。  
  
 **影响：** Active Directory Web 服务 (ADWS) 未设置为自动默认开始模式。 如果在服务器上的 ADWS 停止或禁用，如 Active Directory 模块的 Windows PowerShell 的客户端应用程序或不会访问或管理此服务器运行的目录服务实例 Active Directory 管理中心。 有关详细信息，请参阅[什么是广告 DS 中的新增功能：Active Directory Web 服务](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx)(https://technet.microsoft.com/library/dd391908(WS.10).aspx) 在 Windows Server Technical 媒体库中。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-active-directory-web-services-service-to-start-automatically"></a>若要配置了 Active Directory Web 服务服务，以便自动启动  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**Active Directory Web 服务**服务，，然后单击**属性**。  
  
3.  在**常规**选项卡上，更改**启动类型**到**自动**，然后单击**确定**。  
  
### <a name="the-dhcp-client-service-should-be-configured-to-start-automatically-by-default"></a>应配置 DHCP 客户服务，默认情况下自动启动  
 **问题：** DHCP 客户服务未配置为自动启动。  
  
 **影响：** DHCP 客户服务将不启动时自动启动的服务器。 如果停止该服务的客户端无法从服务器接收 IP 地址。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-dhcp-client-service-to-start-automatically"></a>若要配置为自动启动 DHCP 客户服务  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**DHCP 客户端**服务，，然后单击**属性**。  
  
3.  在**常规**选项卡上，更改**启动类型**到**自动**，然后单击**确定**。  
  
### <a name="the-iis-admin-service-should-be-configured-to-start-automatically-by-default"></a>应配置 IIS 管理员服务，默认情况下自动启动  
 **问题：** IIS 管理员服务未配置为自动启动。  
  
 **影响：** IIS 管理员服务将不启动时自动启动的服务器。 如果停止该服务，你可能会无法访问运行的在服务器上，如远程网站访问的网站。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-iis-admin-service-to-start-automatically"></a>若要配置 IIS 管理员服务，以便自动启动  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**IIS 管理员服务**，然后单击**属性**。  
  
3.  在**常规**选项卡上，更改**启动类型**到**自动**，然后单击**确定**。  
  
### <a name="the-world-wide-web-publishing-service-should-be-configured-to-start-automatically-by-default"></a>应配置 World Wide Web 发布服务为默认情况下自动启动。  
 **问题：** World Wide Web 发布服务不配置为自动启动。  
  
 **影响：** World Wide Web 发布服务可能会无法启动时自动启动的服务器。 如果停止该服务，你可能会无法访问运行的在服务器上，如远程网站访问的网站。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-world-wide-web-publishing-service-to-start-automatically"></a>若要配置 World Wide Web 发布服务，以便自动启动  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**World Wide Web 发布服务**，然后单击**属性**。  
  
3.  在**常规**选项卡上，更改**启动类型**到**自动**，然后单击**确定**。  
  
### <a name="the-remote-registry-service-should-be-configured-to-start-automatically-by-default"></a>应配置远程注册表服务为默认情况下自动启动。  
 **问题：**远程注册表服务未配置为自动启动。  
  
 **影响：**  
  
 在服务器启动远程注册表服务可能不会自动启动。 如果停止该服务，你可能无法远程执行一些网络操作。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-remote-registry-service-to-start-automatically"></a>若要配置远程注册表服务，以便自动启动  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**远程注册表**服务，，然后单击**属性**。  
  
3.  在**常规**选项卡上，更改**启动类型**到**自动**，然后单击**确定**。  
  
### <a name="the-remote-desktop-gateway-service-should-be-configured-to-start-automatically-by-default"></a>远程桌面网关服务应将配置为默认情况下自动启动  
 **问题：**远程桌面网关服务未配置为自动启动。  
  
 **影响：**停止该服务，如果用户可能会无法访问计算机使用远程 Web 访问。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-remote-desktop-gateway-service-to-start-automatically"></a>若要将自动启动远程桌面网关服务配置  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**远程桌面网关**服务，，然后单击**属性**。  
  
3.  在**常规**选项卡上，更改**启动类型**到**（延迟启动）的自动**，然后单击**确定**。  
  
### <a name="the-windows-time-service-should-be-configured-to-start-automatically-by-default"></a>应配置 Windows 时间服务，默认情况下自动启动  
 **问题：** Windows 时间服务未配置为自动启动。  
  
 **影响：**停止该服务，如果时间和数据同步不可用。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-windows-time-service-to-start-automatically"></a>若要将自动启动 Windows 时服务配置为  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**Windows 时间**服务，，然后单击**属性**。  
  
3.  在**常规**选项卡上，更改**启动类型**到**自动**，然后单击**确定**。  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-started"></a>应开始分布式事务处理协调器 (MSDTC) 服务  
 **问题：** MSDTC 服务未运行的服务器上。  
  
 **影响：**停止该服务，如果 SQL Server 或 COM 的某些功能可能会失败。 因此，使用 Microsoft SQL Server 或 COM 功能的应用程序可能无法正常工作。  
  
 **解决方法：**  
  
##### <a name="to-start-the-distributed-transaction-coordinator-service"></a>若要开始分布式事务处理协调器服务  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**分布式事务处理协调器**服务，，然后单击**开始**。  
  
### <a name="the-netlogon-service-should-be-started"></a>应开始 Netlogon 服务  
 **问题：** Netlogon 服务未运行的服务器上。  
  
 **影响：**如果没有启动该服务，服务器可能未进行身份验证用户和服务。  
  
 **解决方法：**  
  
##### <a name="to-start-the-netlogon-service"></a>若要开始 Netlogon 服务  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**Netlogon**服务，，然后单击**开始**。  
  
### <a name="the-dns-client-service-should-be-started"></a>应开始 DNS 客户服务  
 **问题：** DNS 客户端服务未运行的服务器上。  
  
 **影响：**服务器如果没有启动该服务，可能无法解决了 DNS 名称。  
  
 **解决方法：**  
  
##### <a name="to-start-the-dns-client-service"></a>若要开始 DNS 客户服务  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**DNS 客户端**服务，，然后单击**开始**。  
  
### <a name="the-dns-server-service-should-be-started"></a>应启动 DNS 服务器服务  
 **问题：** DNS 服务器服务未运行的服务器上。  
  
 **影响：**如果尚未启动 DNS 服务器服务，可能不会 DNS 更新。  
  
 **解决方法：**  
  
##### <a name="to-start-the-dns-server-service"></a>若要开始 DNS 服务器服务  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**DNS 服务器**服务，，然后单击**开始**。  
  
### <a name="active-directory-web-services-is-not-started"></a>Active Directory 不启动 web 服务  
 **问题：**不启动的 Active Directory Web 服务。  
  
 **影响：**不启动的 Active Directory Web 服务 (ADWS)。 如果在服务器上的 ADWS 停止或禁用，如 Active Directory 模块的 Windows PowerShell 的客户端应用程序或不会访问或管理此服务器运行的目录服务实例 Active Directory 管理中心。 有关详细信息，请参阅[什么是广告 DS 中的新增功能：Active Directory Web 服务](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx)(https://technet.microsoft.com/library/dd391908(WS.10).aspx) 在 Windows Server Technical 媒体库中。  
  
 **解决方法：**  
  
##### <a name="to-start-the-active-directory-web-services-service"></a>若要开始 Active Directory Web 服务  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**Active Directory Web 服务**，然后单击**开始**。  
  
### <a name="the-dhcp-client-service-should-be-started"></a>应开始 DHCP 客户服务  
 **问题：** DHCP 客户服务未运行的服务器上。  
  
 **影响：**如果停止该服务的客户端无法从服务器接收 IP 地址。  
  
 **解决方法：**  
  
##### <a name="to-start-the-dhcp-client-service"></a>若要开始 DHCP 客户服务  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**DHCP 客户端**服务，，然后单击**开始**。  
  
### <a name="the-iis-admin-service-should-be-started"></a>应开始 IIS 管理员服务  
 **问题：** IIS 管理员服务未运行的服务器上。  
  
 **影响：**停止该服务，你可能会无法访问运行的在服务器上，如远程网站访问的网站。  
  
 **解决方法：**  
  
##### <a name="to-start-the-iis-admin-service"></a>若要开始 IIS 管理员服务  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**IIS 管理员服务**，然后单击**开始**。  
  
### <a name="the-world-wide-web-publishing-service-should-be-started"></a>应启动 World Wide Web 发布服务  
 **问题：** World Wide Web 发布服务未运行的服务器上。  
  
 **影响：**停止该服务，你可能会无法访问运行的在服务器上，如远程网站访问的网站。  
  
 **解决方法：**  
  
##### <a name="to-start-the-world-wide-web-publishing-service"></a>若要开始 World Wide Web 发布服务  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**World Wide Web 发布服务**，然后单击**开始**。  
  
### <a name="the-remote-desktop-gateway-service-should-be-started"></a>应启动远程桌面网关服务  
 **问题：**远程桌面网关服务未运行的服务器上。  
  
 **影响：**停止该服务，如果用户可能会无法访问计算机使用远程 Web 访问。  
  
 **解决方法：**  
  
##### <a name="to-start-the-remote-desktop-gateway-service"></a>若要启动远程桌面网关服务  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**远程桌面网关**服务，，然后单击**开始**。  
  
### <a name="the-windows-time-service-should-be-started"></a>应启动 Windows 时间服务  
 **问题：** Windows 时间服务未运行的服务器上。  
  
 **影响：**停止该服务，如果时间和数据同步将不提供。  
  
 **解决方法：**  
  
##### <a name="to-start-the-windows-time-service"></a>若要开始 Windows 的时间服务  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**Windows 时间**服务，，然后单击**开始**。  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-logon-account-should-be-nt-authoritynetwork-service"></a>应 NT AUTHORITY\Network 服务分布式事务处理协调器 (MSDTC) service 登录帐户。  
 **问题：**分布式事务处理协调器 (MSDTC) 服务默认登录帐户被更改。  
  
 **影响：**该服务可能没有按预期方式工作所必需的权限。 因此，使用 SQL Server 或 COM 功能的应用程序可能无法正常工作。  
  
 **解决方法：**  
  
##### <a name="to-change-the-logon-account-for-the-service"></a>若要更改登录帐户的服务  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**分布式事务处理协调器**服务，，然后单击**属性**。  
  
3.  在**登录**选项卡上，选择**此帐户**，类型**NT AUTHORITY\Network 服务**，然后单击**确定**。  
  
### <a name="the-netlogon-service-should-use-the-local-system-account-as-its-logon-account"></a>Netlogon 服务应为其登录帐户来使用系统本地帐户  
 **问题：** Netlogon 服务默认登录帐户被更改。  
  
 **影响：**该服务可能没有按预期方式工作所必需的权限。 因此，服务器可能未进行身份验证用户和服务。  
  
 **解决方法：**  
  
##### <a name="to-change-the-netlogon-service-logon-account"></a>若要更改 Netlogon 服务登录帐户  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**Netlogon**服务，，然后单击**属性**。  
  
3.  在**登录**选项卡上，选择**系统本地帐户**。  
  
### <a name="the-dns-client-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>DNS 客户端服务应 NT AUTHORITY\Network 服务帐户用作其登录帐户  
 **问题：** DNS 客户端服务默认登录帐户被更改。  
  
 **影响：**该服务可能没有按预期方式工作所必需的权限。 因此，服务器可能无法解决了 DNS 名称。  
  
 **解决方法：**  
  
##### <a name="to-change-the-dns-client-service-logon-account"></a>若要更改 DNS 客户端服务登录帐户  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**DNS 客户端**服务，，然后单击**属性**。  
  
3.  在**登录**选项卡上，选择**此帐户**，然后键入**NT AUTHORITY\Network 服务**。  
  
### <a name="the-dns-server-service-should-use-the-local-system-account-as-its-logon-account"></a>DNS 服务器服务应为其登录帐户来使用系统本地帐户  
 **问题：** DNS 服务器服务默认登录帐户被更改。  
  
 **影响：**该服务可能没有按预期方式工作所必需的权限。 因此，可能不会 DNS 更新。  
  
 **解决方法：**  
  
##### <a name="to-change-the-dns-server-service-logon-account"></a>若要更改的 DNS 服务器服务登录帐户  
  
1.  打开服务器上的 services.msc  
  
2.  右键单击**DNS 服务器**服务，，然后单击**属性**。  
  
3.  在**登录**选项卡上，选择**系统本地帐户**。  
  
### <a name="active-directory-web-services-is-not-the-default-logon-account"></a>Active DirectoryWeb 服务不是默认登录帐户  
 **问题：** Active Directory Web 服务不是默认登录帐户。 默认情况下，登录帐户设置为**系统本地帐户**。  
  
 **影响：**不启动的 Active Directory Web 服务 (ADWS)。 如果在服务器上的 ADWS 停止或禁用，如 Active Directory 模块的 Windows PowerShell 的客户端应用程序或不会访问或管理此服务器运行的目录服务实例 Active Directory 管理中心。 有关详细信息，请参阅[什么是广告 DS 中的新增功能：Active Directory Web 服务](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx)(https://technet.microsoft.com/library/dd391908(WS.10).aspx) 在 Windows Server Technical 媒体库中。  
  
 **解决方法：**  
  
##### <a name="to-change-the-active-directory-web-services-logon-account"></a>若要更改的 Active Directory Web 服务登录帐户  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**Active Directory Web 服务**，然后单击**属性**。  
  
3.  更改**启动类型**到**自动**，然后单击**确定**。  
  
4.  在服务的 Active Directory Web**属性**，单击**登录**选项卡。  
  
5.  选择**系统本地帐户**选项，然后依次单击**确定**。  
  
### <a name="the-windows-update-service-should-use-the-local-system-account-as-its-logon-account"></a>Windows 更新服务应为其登录帐户来使用系统本地帐户  
 **问题：**自动更新服务默认登录帐户被更改。  
  
 **影响：**该服务可能没有按预期方式工作所必需的权限。 因此，服务器可能无法接收自动更新。  
  
 **解决方法：**  
  
##### <a name="to-change-the-windows-update-service-logon-account"></a>若要更改 Windows 更新服务登录帐户  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**Windows 更新**服务，并单击属性。  
  
3.  在**登录**选项卡上，选择**系统本地帐户**。  
  
### <a name="the-dhcp-client-service-should-use-the-nt-authoritylocalservice-account-as-its-logon-account"></a>DHCP 客户服务应为其登录帐户来使用 1 帐户  
 **问题：** DHCP 客户服务默认登录帐户被更改。  
  
 **影响：**该服务可能没有按预期方式工作所必需的权限。 这样一来，客户端计算机不会与服务器收到 IP 地址。  
  
 **解决方法：**  
  
##### <a name="to-change-the-dhcp-client-service-logon-account"></a>若要更改 DHCP 客户端服务登录帐户  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**DHCP 客户端**服务，，然后单击**属性**。  
  
3.  在**登录**选项卡上，选择**此帐户**，然后键入**NT AUTHORITY\Local 服务**。  
  
### <a name="the-iis-admin-service-should-use-the-local-system-account-as-its-logon-account"></a>IIS 管理服务应为其登录帐户来使用系统本地帐户  
 **问题：** IIS 管理服务默认登录帐户被更改。  
  
 **影响：**该服务可能未安装所需的权限所需按预期工作。 这样一来，你可能会无法访问运行的在服务器上，如远程网站访问的网站。  
  
 **解决方法：**  
  
##### <a name="to-change-the-service-logon-account"></a>若要更改的服务登录帐户  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**IIS 管理服务**，然后单击**属性**。  
  
3.  在**登录**选项卡上，选择**系统本地帐户**。  
  
### <a name="the-world-wide-web-publishing-service-should-use-the-local-system-account-as-its-logon-account"></a>World Wide Web 发布服务应为其登录帐户来使用系统本地帐户  
 **问题：** World Wide Web 发布服务默认登录帐户被更改。  
  
 **影响：**该服务可能没有按预期方式工作所必需的权限。 这样一来，你可能会无法访问运行的在服务器上，如远程网站访问的网站。  
  
 **解决方法：**  
  
##### <a name="to-change-the-world-wide-web-publishing-service-logon-account"></a>若要更改 World Wide Web 发布服务登录帐户  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**World Wide Web 发布服务**，然后单击**属性**。  
  
3.  在**登录**选项卡上，选择**系统本地帐户**。  
  
### <a name="the-remote-desktop-gateway-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>远程桌面网关服务应 NT AUTHORITY\Network 服务帐户用作其登录帐户  
 **问题：**默认登录帐户的远程桌面网关 service 发生更改。  
  
 **影响：**该服务可能没有相应权限，可按预期工作。 这样一来，用户可能会无法访问计算机使用远程 Web 访问。  
  
 **解决方法：**  
  
##### <a name="to-change-the-remote-desktop-gateway-service-logon-account"></a>若要更改远程桌面网关服务登录帐户  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**远程桌面网关**服务，，然后单击**属性**。  
  
3.  在**登录**选项卡上，选择**此帐户**，然后键入**NT AUTHORITY\Network 服务**。  
  
### <a name="the-windows-time-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>Windows 时间服务应 NT AUTHORITY\Network 服务帐户用作其登录帐户  
 **问题：** Windows 时间服务默认登录帐户被更改。  
  
 **影响：**该服务可能没有相应权限，可按预期工作。 因此，日期和时间同步可能不可用。  
  
 **解决方法：**  
  
##### <a name="to-change-the-windows-time-service-logon-account"></a>若要更改 Windows 时间 service 登录帐户  
  
1.  打开 services.msc 服务器上。  
  
2.  右键单击**Windows 时间**服务，，然后单击**属性**。  
  
3.  在**登录**选项卡上，选择**此帐户**，然后键入**NT AUTHORITY\Local 服务**。  
  
### <a name="the-built-in-administrators-group-does-not-have-the-right-to-log-on-as-batch-job"></a>内置的管理员组没有权作为批量作业登录  
 **问题：**内置管理员组没有作为批量作业登录的权利。  
  
 **影响：**管理员创建一个警报，并配置了该通知以运行管理员未登录时，如果该通知将失败，错误代码的 2147943785 并。  
  
 **解决方法：**关于如何提供内置管理员作为批量作业登录的组权限的信息，请参阅[提供内置的管理员组权作为批量作业登录](https://technet.microsoft.com/library/jj635076)(https://technet.microsoft.com/library/jj635076)。  
  
### <a name="the-windows-firewall-is-turned-off"></a>关闭 Windows 防火墙  
 **问题：**已关闭 Windows 防火墙。 默认值处于打开状态。  
  
 **影响：**具体取决于你的防火墙设置，Windows 防火墙可以帮助你服务器和网络从恶意活动保护通过阻止来自通过服务器传递的一些信息。  
  
 **解决方法：**  
  
##### <a name="to-turn-on-windows-firewall-on-the-server"></a>若要打开 Windows 防火墙在服务器上  
  
1.  在服务器上打开控制面板。  
  
2.  在控制面板中，单击**系统和安全**，然后单击**Windows 防火墙**。  
  
3.  Windows 防火墙在单击**打开或关闭 Windows 防火墙**、选择**打开 Windows 防火墙**选项，然后依次单击**确定**。  
  
### <a name="the-internal-network-adapter-is-not-configured-to-register-ip-address-in-dns"></a>内部网络适配器未配置为 DNS 注册 IP 地址  
 **问题：**内部网络适配器未配置为 DNS 注册它的 IP 地址。  
  
 **影响：**如果未在 DNS 注册内部网络适配器的 IP 地址，它可能不能使用来访问该服务器服务器的计算机名称。  
  
 **解决方法：**验证配置内部网络适配器中 DNS 注册。  
  
### <a name="dns-the-values-for-the-dns-forwardingtimeout-and-recursiontimeout-registry-key-are-identical"></a>对 DNS ForwardingTimeout 和 RecursionTimeout 的注册表项这些值正确相同的 DNS:  
 **问题：** DNS ForwardingTimeout 注册表项值不应 RecursionTimeout 注册表项值相同。  
  
 **影响：**你可能会无法访问 Internet 的资源，由名。  
  
 **解决方法：**设置为大于 ForwardingTimeout 键 RecursionTimeout 注册表项的值，则 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters 在注册表中。  
  
### <a name="the-forward-dns-zone-for-your-active-directory-domain-does-not-allow-secure-updates"></a>您的域的 Active Directory 的前进 DNS 区域不允许安全更新  
 **问题：**应该配置转发查找区域允许仅安全的动态更新。  
  
 **影响：**当您启用安全的动态更新仅授权的用户和主机可以记录进行更改。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-forward-lookup-zone-for-your-active-directory-domain"></a>若要配置了 Active Directory 域的前进查找区域  
  
1.  打开 dnsmgmt.msc 服务器上。  
  
2.  右键单击你的 Active Directory 域的前进查找区域，然后单击**属性**。  
  
3.  在**的动态更新**下拉列表中，选择**仅安全**，然后单击**确定**。  
  
### <a name="the-forward-dns-zone-does-not-allow-secure-updates"></a>前进 DNS 区域不允许安全更新  
 **问题：**应该配置转发查找区域 _msdcs.* 区域允许仅安全的动态更新。  
  
 **影响：**当您启用安全的动态更新仅授权的用户和主机可以记录 msdcs.* 区域中进行更改。  
  
 **解决方法：**  
  
##### <a name="to-allow-secure-updates-in-the-msdcs-zone"></a>若要允许 _msdcs 区域中的安全更新  
  
1.  打开 dnsmgmt.msc 服务器上。  
  
2.  转发查找区域 _msdcs 区域中，右键单击，然后单击**属性**。  
  
3.  在**的动态更新**下拉列表中，选择**仅安全**，然后单击**确定**。  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>Internet Explorer 无法启用增强的安全配置  
 **问题：**管理员组当前未启用 Internet Explorer 增强的安全配置 (IE ESC)。  
  
 **影响：**没有为管理员组启用增强的 Internet Explorer 的安全配置，如果你的服务器和 Internet Explorer 有增加曝光度恶意攻击可以通过 Web 内容和应用程序脚本复制到。  
  
 **解决方法：**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>若要启用增强的 Internet Explorer 的安全配置  
  
1.  打开**服务器管理器**服务器，，然后单击**本地服务器**。  
  
2.  在**属性**窗格中，更改的设置**IE 增强的安全配置**到**上**，然后单击**确定**。  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>Internet Explorer 无法启用增强的安全配置  
 **问题：**用户组当前未启用 Internet Explorer 增强的安全配置 (IE ESC)。  
  
 **影响：**用户组未启用增强的 Internet Explorer 的安全配置，如果你的服务器和 Internet Explorer 增加曝光度恶意攻击可以通过 Web 内容和应用程序脚本复制到。  
  
 **解决方法：**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>若要启用增强的 Internet Explorer 的安全配置  
  
1.  打开**服务器管理器**，然后单击**本地服务器**。  
  
2.  在**属性**窗格中，更改的设置**IE 增强的安全配置**到**上**，然后单击**确定**。  
  
### <a name="the-source-server-remains-in-active-directory-sites-and-services"></a>源服务器保持 Active Directory 的站点和服务中  
 **问题：** Active Directory 的站点和服务中仍然存在运行 Windows 小型企业服务器源服务器 Default-First-Site-Name 中。  
  
 **影响：**源服务器保留在主动主管站点和服务，如果客户端计算机遇到连接问题：s。  
  
 **解决方法：**你应降级源服务器、从域中, 删除它，然后源服务器从删除 Active Directory 的站点和服务以及 Active Directory 用户和计算机。  
  
### <a name="source-server-remains-in-sbscomputer-ou"></a>源服务器保留在 SBSComputer OU  
 **问题：** Active Directory 用户和计算机中仍然存在运行 Windows 小型企业服务器源服务器。  
  
 **影响：**源服务器保留在主动主管用户和计算机，如果客户端计算机遇到连接问题：s。  
  
 **解决方法：**你应降级源服务器、从域中, 删除它，然后源服务器从删除 Active Directory 的站点和服务以及 Active Directory 用户和计算机。  
  
### <a name="a-group-policy-is-missing"></a>组策略为缺少  
 **问题：**默认域策略组策略是丢失。  
  
 **影响：**默认域策略，才能进行正确的域的函数。  
  
 **解决方法：**  
  
##### <a name="to-restore-a-missing-group-policy"></a>若要还原丢失的组策略  
  
1.  打开 gpmc.msc 服务器上。  
  
2.  在组策略管理器中，展开域森林中，并搜索的控制台树**默认域策略**组策略对象。  
  
3.  如果该策略树中未显示，该从备份中还原系统的状态。  
  
### <a name="no-dns-name-server-resource-records"></a>没有 DNS 服务器进行命名资源记录  
 **问题：**在服务器的前进查找区域没有 DNS 名称记录 (NS) 服务器资源。  
  
 **影响：**没有 DNS 名称 (NS) 服务器资源记录位于域的 Active Directory 的前进查找区域，如果用户可能会无法访问该网络或 Internet 上的资源。  
  
 **解决方法：**  
  
##### <a name="to-restore-missing-dns-name-server-resource-records"></a>若要还原缺少 DNS 服务器命名资源记录  
  
1.  打开 dnsmgmt.msc 服务器上。  
  
2.  在 DNS 管理器中，右键单击 Active Directory 的域中，查找前进区域，然后单击**属性**。  
  
3.  在**名称服务器**选项卡上，验证的设置是否正确。  
  
4.  进行任何需要的更改，然后单击**确定**以保存设置。  
  
### <a name="no-dns-name-server-records"></a>没有 DNS 名称服务器记录  
 **问题：**有没有 DNS 名称服务器 (NS) 资源记录你服务器 _msdcs 区域中 (例如：_msdcs.contoso.local)。  
  
 **影响：**没有 DNS 名称 (NS) 服务器资源记录位于域的 Active Directory _msdcs 区域，如果用户可能会无法访问该网络或 Internet 上的资源。  
  
 **解决方法：**  
  
##### <a name="to-restore-missing-dns-name-server-records"></a>若要还原丢失的 DNS 服务器记录  
  
1.  打开 dnsmgmt.msc 服务器上。  
  
2.  在 DNS 管理器中，右键单击该 _msdcs 区域的前进查找区域，然后单击**属性**。  
  
3.  在**名称服务器**选项卡上，验证的设置是否正确。  
  
4.  进行任何需要的更改，然后单击**确定**以保存设置。  
  
### <a name="no-dns-name-server-records"></a>没有 DNS 名称服务器记录  
 **问题：**有没有 DNS 名称服务器 (NS) 资源记录的委派的 _msdcs 向前查找区域。  
  
 **影响：** DNS 服务器服务不 DNS 名称 (NS) 服务器资源存在记录的委派的 _msdcs 向前查找区域，如果不能解决域 DNS 资源的记录，并将无法启动。  
  
 **解决方法：**  
  
##### <a name="to-reconfigure-missing-dns-name-server-records"></a>若要重新配置缺少的 DNS 服务器记录  
  
1.  打开 dnsmgmt.msc 服务器上。  
  
2.  在 DNS 管理器中，展开你服务器的名称，然后展开**前进查找区域**。  
  
3.  单击你的 Active Directory 的域的前进查找区域 (例如：contoso.local)。  
  
4.  委派的 _msdcs 区域显示为灰色文件夹。 右键单击 _msdcs 的区域，然后单击**属性**。  
  
5.  在**名称服务器**选项卡上，验证的设置是否正确。  
  
6.  进行任何需要的更改，然后单击**确定**以保存设置。  
  
### <a name="authenticated-users-not-a-member-of-the-pre-windows-2000-compatible-access-group"></a>身份验证的用户不 windows 2000 兼容访问的组成员  
 **问题：**不 windows 2000 兼容访问的组成员的用户身份验证组。  
  
 **影响：**内置的用户身份验证组不是 windows 2000 兼容访问的组成员，如果网络用户可能会遇到"拒绝访问"错误。  
  
 **解决方法：**  
  
##### <a name="to-add-authenticated-users-to-the-pre-windows-2000-compatible-access-group"></a>若要添加到 windows 2000 兼容访问组的用户身份验证  
  
1.  打开 dsa.msc 服务器上。  
  
2.  在**内置**文件夹、右键单击**windows 2000 兼容访问**，然后单击**属性**。  
  
3.  单击**添加**，类型**验证用户**，然后单击**确定**两次。  
  
### <a name="dns-client-not-configured"></a>无法配置 DNS 客户端  
 **问题：** DNS 客户端无法配置仅指向服务器的内部 IP 地址。  
  
 **影响：** DNS 名称分辨率如果 DNS 客户端无法配置仅指向服务器的内部 IP 地址，可能会失败。  
  
 **解决方法：**  
  
##### <a name="to-configure-dns-to-point-only-to-the-server-s-internal-ip-address"></a>若要配置 DNS 仅指向服务器 s 内部 IP 地址  
  
1.  在客户端计算机上，打开**属性**网络连接的页面。  
  
2.  请确保配置 DNS 仅指向服务器的内部 IP 地址。  
  
### <a name="default-application-pool-value-changed"></a>更改默认应用程序池值  
 **问题：**池中应用程序池的最大工作进程数未设置为默认值为 1。  
  
 **影响：**用户可能无法连接到 Windows 小型企业服务器基于 web 的服务。  
  
 **解决方法：**  
  
##### <a name="to-reset-maximum-worker-processes-for-the-default-application-pool"></a>重置为默认应用程序池最大的工作进程  
  
1.  打开 Internet 信息服务 (IIS) 管理器在服务器上。  
  
2.  在 IIS 管理器中，展开你服务器的名称，然后单击**应用程序池**。  
  
3.  在**应用程序池**，右键单击**池中**，然后单击**高级设置**。  
  
4.  在**高级设置**，更改的值**最大的工作进程**到 1，然后单击**确定**。  
  
5.  关闭**高级设置**，右键单击**池中**，，然后停止，并重新启动应用程序池。  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-account"></a>应用程序池远程 Web 访问不使用默认的帐户  
 **问题：**未使用的默认帐户运行 RemoteAppPool 应用程序池。  
  
 **影响：**网络用户可能会无法访问该远程网站访问的网站。  
  
 **解决方法：**  
  
##### <a name="to-reset-the-remote-application-pool-to-use-the-default-account"></a>重置要使用默认帐户远程应用程序池  
  
1.  打开 Internet 信息服务 (IIS) 管理器在服务器上。  
  
2.  在 IIS 管理器中，展开你服务器的名称，然后单击**应用程序池**。  
  
3.  在**应用程序池**，右键单击**RemoteAppPool**，然后单击**高级设置**。  
  
4.  在**高级设置**，更改**身份**到**网络服务**，然后单击**确定**。  
  
5.  关闭**高级设置**，右键单击**RemoteAppPool**，，然后停止，并重新启动应用程序池。  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-net-framework-version"></a>应用程序池远程 Web 访问不使用的默认.NET Framework 版本  
 **问题：** RemoteAppPool 应用程序池未运行与 Microsoft.NET Framework 的默认版本。  
  
 **影响：**网络用户可能会无法访问该远程网站访问的网站。  
  
 **解决方法：**  
  
##### <a name="to-change-the-net-framework-version-used-by-the-remoteapppool"></a>若要更改 RemoteAppPool 使用.NET Framework 版本  
  
1.  打开 Internet 信息服务 (IIS) 管理器在服务器上。  
  
2.  在 IIS 管理器中，展开你服务器的名称，然后单击**应用程序池**。  
  
3.  在**应用程序池**，右键单击**RemoteAppPool**，然后单击**高级设置**。  
  
4.  在**高级设置**，更改**.NET Framework 版本**到 4.0 版，然后单击**确定**。  
  
5.  关闭**高级设置**，右键单击**RemoteAppPool**，，然后停止，并重新启动应用程序池。  
  
### <a name="the-remoteaccesslog-file-is-larger-than-1-gb-in-size"></a>1 GB 的大小大于 RemoteAccess.log 文件使用了  
 **问题：** Remoteaccess.log 文件的大小超过 1 GB，如果你遇到低磁盘空间错误，系统驱动器上的。  
  
 **影响：**太大 Remoteaccess.log 文件时，它可能会导致可用空间的问题：在驱动器 c: s。  
  
 **解决方法：**服务器进行备份后，你可以删除位于 %ProgramData%\ Microsoft Remoteaccess.log 文件 \ Windows Server \Logs\WebApps 文件夹。  
  
### <a name="default-web-sites-log-directory-is-over-1-gb-in-size"></a>默认网站日志目录是超过 1 GB 的大小  
 **问题：**默认网站的日志文件夹的大小超过 1 GB，如果你遇到低磁盘空间错误，系统驱动器上的。  
  
 **影响：**太大的默认网站日志文件夹是否，它可能会导致可用空间的问题：上驱动器 c:  
  
 **解决方法：**备份服务器，并同时默认网站会停止，你可以删除日志文件 C:\inetpub\logs\LogFiles\W3SVC1 文件夹中的后。 然后开始默认网站。  
  
### <a name="no-binding-for-ssl-on-all-ip-addresses"></a>针对所有 IP 地址 ssl 没有绑定  
 **问题：**没有任何绑定安全套接字层 (SSL) 上的服务器上的所有 IP 地址。  
  
 **影响：** SSL 未与所有 IP 地址绑定服务器上，如果某些网站将不会向用户提供。  
  
 **解决方法：**  
  
##### <a name="to-bind-ssl-to-all-ip-addresses-on-the-server"></a>若要绑定 SSL 服务器上的所有 IP 地址  
  
1.  打开 Internet 信息服务 (IIS) 管理器在服务器上。  
  
2.  IIS 管理器中上**连接**窗格中，依次展开服务器、**站点**，右键单击**默认网站**，然后单击**编辑绑定**。  
  
3.  在**站点绑定**，单击**添加**，然后选择以下设置：  
  
    -   **键入** = **https**  
  
    -   **IP 地址** = **全部未分配**  
  
    -   **端口**= 443  
  
4.  选择 SSL 证书，，然后单击**确定**以保存所做的更改。  
  
### <a name="no-binding-for-ssl-on-the-default-web-site"></a>没有默认网站上 ssl 绑定  
 **问题：**不没有 ssl 默认网站上的任何绑定。  
  
 **影响：**如果 SSL 不绑定到默认网站时，某些网站可能不可向用户提供。  
  
 **解决方法：**  
  
##### <a name="to-bind-ssl-to-the-default-website"></a>若要将 SSL 绑定到默认的网站  
  
1.  打开 Internet 信息服务 (IIS) 管理器在服务器上。  
  
2.  IIS 管理器中上**连接**窗格中，依次展开服务器、**站点**，右键单击**默认网站**，然后单击**编辑绑定**。  
  
3.  在**站点绑定**，单击**添加**，然后选择以下选项：  
  
    -   **键入** = **https**  
  
    -   **IP 地址** = **全部未分配**  
  
    -   **端口**= 443  
  
    > [!NOTE]
    >  如果端口 443 HTTPS 绑定存在特定的 IP 地址，则更改**IP 地址**该绑定到特性**所有未分配**。 为此例外情况是 127.0.0.1 的 IP 地址。 请勿更改为 127.0.0.1 绑定。  
  
4.  选择 SSL 证书，，然后单击**确定**以保存所做的更改。  
  
### <a name="a-certificate-expires-within-30-days"></a>在 30 天内到期证书  
 **问题：**服务器证书将在 30 天内后过期。  
  
 **影响：**服务器不能使用过期的证书。 如果证书已过期，用户可能无法使用任何地方访问函数。  
  
 **解决方法：**若要防止过期的证书，续订与你信任的证书颁发机构证书。  
  
### <a name="certificate-subject-does-not-match-the-name-configured-by-domain-name-wizard"></a>证书主题域名向导配置名称不匹配  
 **问题：**证书主题不符域名向导配置的名称。  
  
 **影响：**如果证书主题不符域名向导配置的名称，将不会初始化某些网站。 其他站点会显示错误"没有此网站的安全证书有问题。"  
  
 **解决方法：**要解决此问题:，重新运行设置任何地方访问向导和提供证书，正确域名或购买新的证书相匹配你想要使用的域名。  
  
### <a name="one-or-more-user-accounts-have-duplicate-cn-names"></a>一个或多个用户帐户已重复 CN 名称  
 **问题：**一个或多个用户帐户已重复 CN 名称：{0}。  
  
 **影响：**如果用户帐户重复 CN 名称，用户可能无法登录到该网络。 此外，对于用户的 Active Directory 搜索可以返回错误值。  
  
 **解决方法：**要解决此问题:，确保网络用户帐户不具有重复"CN ="名称。 若要更容易实现此，请考虑导出到文本文件查看 Active Directory 内容。 有关如何执行此操作的信息，请参阅[使用 LDIFDE 导入和导出到 Active Directory（知识库文章 237677）directory 对象](https://support.microsoft.com/kb/237677)(https://support.microsoft.com/kb/237677)。  
  
### <a name="nt-backup-is-installed"></a>安装 NT 备份  
 **问题：** Windows NT 备份程序安装在服务器上。  
  
 **影响：** Windows Server Essentials 使用 Windows Server 备份。 如果还已安装 Windows NT 备份计划，可以在两个备份程序之间存在冲突。 这可能导致失败的 Windows Server 备份过程。 冲突还可能会阻止你使用的备份中还原该服务器从。  
  
 **解决方法：**要解决此问题:，从服务器卸载 NT 备份计划。  
  
### <a name="iis-does-not-own-port-80-000080-or-port-443-0000443"></a>端口 80 (0.0.0.0:80) 或端口 (0.0.0.0:443) 443 IIS 不拥有  
 **问题：** Internet 信息服务 (IIS) 不拥有端口 80 (0.0.0.0:80) 或 443 端口。 这些端口当前受其他应用程序的影响。  
  
 **影响：** Windows Server Essentials web 应用程序需要端口 80 使用和端口 443 以向用户提供服务。 端口 80 或端口 443 已使用其他进程或应用程序，如果不能运行 Windows Server Essentials web 应用程序。 如果发生这种情况，远程网站访问和其他应用程序不可向用户提供。  
  
 **解决方法：**要解决此问题:，请卸载的应用程序的已使用端口 80 或端口 443，或指定其他端口到该应用程序。  
  
### <a name="the-default-website-is-not-running"></a>默认网站没有运行  
 **问题：**默认网站未在你的 Windows Server Essentials 环境中运行。  
  
 **影响：** Windows Server Essentials web 应用程序需要使用默认网站。 如果未运行的默认的网站，远程网站访问和其他应用程序不可向用户提供。  
  
 **解决方法：**  
  
##### <a name="to-start-the-default-website"></a>若要开始默认网站  
  
1.  打开 Internet 信息服务 (IIS) 管理器在服务器上。  
  
2.  在 IIS 管理器中，展开你服务器的名称，然后单击**站点**。  
  
3.  右键单击**默认网站**，指向**管理网站**，然后单击**开始**。  
  
### <a name="read-and-script-permissions-for-the-remote-virtual-directory-are-incorrect"></a>阅读和脚本权限 /Remote 虚拟目录不正确  
 **问题：**阅读和脚本权限未分配给 /Remote 虚拟目录。  
  
 **影响：**如果 /Remote 虚拟目录阅读和脚本权限均不正确，用户无法使用远程 Web 访问。 当用户尝试使用远程访问 Web 浏览 Internet 时，它们可能会遇到错误"HTTP 错误 403.1 禁止。"  
  
 **解决方法：**  
  
##### <a name="to-assign-read-and-script-permissions-to-the-remote-directory"></a>若要为 /Remote 目录分配阅读和脚本权限  
  
1.  打开 Internet 信息服务 (IIS) 管理器在服务器上。  
  
2.  在 IIS 管理器中，展开你服务器的名称，然后单击**站点**。  
  
3.  展开**默认网站**，然后展开**远程**。  
  
4.  在**功能视图**，双击**处理程序映射**。  
  
5.  在**操作**窗格中，单击**编辑的功能权限**。  
  
6.  选择**阅读**和**脚本**复选框，然后单击**确定**。  
  
### <a name="http-redirect-is-either-set-or-inherited-on-the-remote-virtual-directory"></a>设置或在虚拟目录 /Remote 继承 HTTP 重定向  
 **问题：**意外设置或在虚拟目录 /Remote 继承 HTTP 重定向属性。  
  
 **影响：** HTTP 重定向属性设置 /Remote 虚拟目录上，如果工作远程网站无法正常工作。  
  
 **解决方法：**  
  
##### <a name="to-remove-the-http-redirect-attribute"></a>若要删除 HTTP 重定向属性  
  
1.  打开 Internet 信息服务 (IIS) 管理器在服务器上。  
  
2.  在 IIS 管理器中，展开你服务器的名称，然后单击**站点**。  
  
3.  展开**默认网站**，然后展开**远程**。  
  
4.  在**功能视图**，双击**HTTP 重定向**。  
  
5.  清除**请求重定向到此目标**复选框，然后依次单击**应用**上**操作**窗格。  
  
### <a name="a-host-name-exists-for-port-80-on-the-default-website"></a>端口 80 默认网站上存在主机名称  
 **问题：**主机名端口 80 默认网站上的分配。  
  
 **影响：**如果主机名分配端口 80 默认网站上，你可能无法连接到某些 Windows Server Essentials web 应用程序。 主机名称并不需要在此情况下不建议  
  
 **解决方法：**  
  
##### <a name="to-clear-the-host-name-entry-for-port-80-on-the-default-website"></a>若要清除端口 80 默认网站上的主条目  
  
1.  打开 Internet 信息服务 (IIS) 管理器在服务器上。  
  
2.  在 IIS 管理器中，展开你服务器的名称，然后单击**站点**。  
  
3.  在**功能视图**，右键单击**默认网站**，然后单击**绑定**。  
  
4.  在**站点绑定**、选择**端口 80 http**设置，然后依次单击**编辑**。  
  
5.  在**编辑站点绑定**，清除**主机名**条目，，然后单击**确定**。  
  
### <a name="backup-does-not-succeed-because-of-a-hidden-partition"></a>备份不会隐藏分区由于成功  
 **问题：**非 NTFS 分区计划备份由 Windows Server 备份。  
  
 **影响：** Windows Server 备份只能备份 NTFS 格式的分区。  
  
 **解决方法：**无法配置 Windows Server 备份来备份非 NTFS 分区。 有关详细信息，请参阅[当系统状态备份失败时，Windows Server 2008 基于计算机（知识库文章 968128）上记录事件 Id 12290 和 16387](https://support.microsoft.com/kb/968128) (https://support.microsoft.com/kb/968128)。  
  
### <a name="the-most-recent-backup-did-not-succeed"></a>未成功的最新的备份  
 **问题：**未成功完成的最新的备份尝试。  
  
 **影响：**备份状态的系统不正确。  
  
 **解决方法：**查看事件日志和最新备份期间发生的错误备份日志。  
  
### <a name="the-startup-type-for-the-file-replication-service-is-not-set-to-automatic"></a>未设置为自动启动类型的文件复制服务  
 **问题：**如果未设置为默认值的自动启动类型文件复制服务 (FRS) 可能不会启动。  
  
 **影响：**域控制器如果未运行该文件复制服务，可能会停止广告其服务。 这可能导致登录错误和组策略的错误等的其他问题。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-file-replication-service-for-automatic-startup"></a>若要配置为自动启动文件复制服务  
  
1.  打开服务主机。  
  
2.  在服务列表中，双击**文件复制**。  
  
3.  对于**启动类型**、选择**自动**，然后单击**应用**。  
  
### <a name="the-file-replication-service-is-not-running"></a>未运行该文件复制服务  
 **问题：**未运行该文件复制服务。  
  
 **影响：**域控制器如果未运行该文件复制服务，可能会停止广告其服务。 此问题可能会导致登录错误和组策略的错误等的其他问题。  
  
 **解决方法：**  
  
##### <a name="to-start-the-file-replication-service"></a>若要开始文件复制服务  
  
1.  打开服务主机。  
  
2.  在服务列表中，双击**文件复制服务**。  
  
3.  单击**开始**。  
  
### <a name="the-logon-account-for-the-file-replication-service-is-not-set-to-use-the-local-system-account"></a>登录帐户的文件复制服务未设置为使用本地系统帐户  
 **问题：**未配置文件复制服务要用作默认登录帐户的系统本地帐户。  
  
 **影响：**如果文件复制服务未使用本地系统作为默认登录帐户，你可能会遇到相关权限错误。 这些错误可以触发的其他错误，并且最终可能导致域控制器广告其服务停止。  
  
 **解决方法：**  
  
##### <a name="to-configure-local-system-as-the-default-logon-account-for-file-replication"></a>作为默认登录帐户的本地系统配置文件复制  
  
1.  打开服务主机。  
  
2.  在服务列表中，双击**文件复制**。  
  
3.  在**服务属性**页上，单击**登录**选项卡。  
  
4.  选择**系统本地帐户**选项，然后依次单击**应用**。  
  
5.  重启该服务。  
  
### <a name="the-startup-type-for-the-dfs-replication-service-is-not-set-to-automatic"></a>启动类型 DFS 复制送修未设置为自动  
 **问题：** DFS 复制服务可能会无法启动，如果未设置为默认值的自动启动类型。  
  
 **影响：**域控制器如果未运行的 DFS 复制服务，可能会停止广告其服务。 这可能导致登录错误和组策略的错误等的其他问题。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-dfs-replication-service-for-automatic-startup"></a>若要配置为自动启动 DFS 复制服务  
  
1.  打开服务主机。  
  
2.  在服务列表中，双击**DFS 复制**。  
  
3.  对于**启动类型**、选择**自动**，然后单击**应用**。  
  
### <a name="the-dfs-replication-service-is-not-running"></a>DFS 复制服务没有运行  
 **问题：**当前未运行 DFS 复制服务。  
  
 **影响：**域控制器如果未运行的 DFS 复制服务，可能会停止广告其服务。 此问题可能会导致登录错误和组策略的错误等的其他问题。  
  
 **解决方法：**  
  
##### <a name="to-start-the-dfs-replication-service"></a>若要开始 DFS 复制服务  
  
1.  打开服务主机。  
  
2.  在服务列表中，双击**DFS 复制**。  
  
3.  单击**开始**。  
  
### <a name="the-dfs-replication-service-is-not-is-not-set-to-use-the-local-system-account"></a>服务不 DFS 复制未设置为使用本地系统帐户  
 **问题：** DFS 复制服务未设置为用作默认登录帐户的系统本地帐户。  
  
 **影响：**如果 DFS 复制服务未使用本地系统作为默认登录帐户，你可能会遇到相关权限错误。 这些错误可以触发的其他错误，并且最终可能导致域控制器广告其服务停止。  
  
 **解决方法：**  
  
##### <a name="to-configure-dfs-replication-to-use-local-system-as-the-default-logon-account"></a>若要配置 DFS 复制要用作默认登录帐户的本地系统  
  
1.  打开服务主机。  
  
2.  在服务列表中，双击**DFS 复制**。  
  
3.  在**服务属性**页上，单击**登录**选项卡。  
  
4.  选择**系统本地帐户**选项，然后依次单击**应用**。  
  
5.  重启该服务。  
  
### <a name="the-windows-server-office-365-integration-service-is-not-set-to-use-the-local-system-account"></a>Windows Server Office 365 集成服务未设置为使用本地系统帐户  
 **问题：** Windows Server 的 Office 365 集成服务未设置为默认登录帐户使用系统本地帐户。  
  
 **影响：**如果 Windows Server Office 365 集成服务不使用作为默认登录帐户的本地系统，Office 365 的某些功能可能无法正常运行。 你还可能会遇到相关权限错误。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-office-365-integration-service-to-use-local-system-as-the-default-logon-account"></a>若要将 Office 365 集成服务要用作默认登录帐户的本地系统配置  
  
1.  打开服务主机。  
  
2.  在服务列表中，双击**Windows Server Office 365 集成服务**。  
  
3.  在**服务属性**页上，单击**登录**选项卡。  
  
4.  选择**系统本地帐户**选项，然后依次单击**应用**。  
  
5.  重启该服务。  
  
### <a name="the-windows-server-office-365-integration-service-is-not-running"></a>Windows Server 的 Office 365 集成服务没有运行  
 **问题：**当前未运行 Windows Server 的 Office 365 集成服务。  
  
 **影响：**如果未运行的 Windows Server 的 Office 365 集成服务，Office 365 的基于云的功能将不可用。  
  
 **解决方法：**  
  
##### <a name="to-start-the-windows-server-office-365-integration-service"></a>若要启动 Windows Server 的 Office 365 集成服务  
  
1.  打开服务主机。  
  
2.  在服务列表中，双击**Windows Server Office 365 集成服务**。  
  
3.  单击**开始**。  
  
### <a name="the-startup-type-for-the-windows-server-office-365-integration-service-is-not-set-to-automatic"></a>Windows Server 的 Office 365 集成服务的启动类型未设置为自动  
 **问题：** Windows Server 的 Office 365 集成服务可能会无法启动，如果未设置为默认值的自动启动类型。  
  
 **影响：**如果未运行的 Windows Server 的 Office 365 集成服务，Office 365 的基于云的功能将不可用。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-office-365-integration-service-for-automatic-startup"></a>若要配置为自动启动的 Office 365 集成服务  
  
1.  打开服务主机。  
  
2.  在服务列表中，双击**Windows Server Office 365 集成服务**。  
  
3.  对于**启动类型**、选择**自动**，然后单击**应用**。  
  
### <a name="a-registry-value-is-missing-or-set-incorrectly"></a>注册表值已丢失或不正确设置  
 **问题：**注册表项在 HKEY_LOCAL_MACHINE \Software\ Microsoft \Rpc\RpcProxy 包含错误值，或不存在。  
  
 **影响：**如果 RPCProxy 注册表项设置不正确，你可能会收到一条错误消息，如下所示:"你的计算机无法连接到远程计算机因为远程桌面网关服务器不可暂时用。 请稍后重新连接或与网络管理员联系。"  
  
 **解决方法：**  
  
##### <a name="to-correct-the-registry-setting"></a>若要更正的注册表设置  
  
1.  注册表编辑器中打开。  
  
2.  导航至以下注册表项：  
  
     HKEY_LOCAL_MACHINE\Software\ Microsoft \Rpc\RpcProxy  
  
3.  确保名为"网站"字符串具有默认网站数据值：  
  
    -   如果数据值不正确，则修改字符串使用正确的值。  
  
    -   如果不存在，创建一个名为"网站"的新字符串和数据值设置为默认的网站。"  
  
### <a name="the-startup-type-for-the-block-level-backup-engine-service-is-not-set-to-manual"></a>未设置为手动阻止级别备份引擎服务的启动类型  
 **问题：**阻止级别备份引擎服务不在使用手动默认启动类型。  
  
 **影响：**如果启动类型未设置为手动阻止级别备份引擎服务可能不会启动。 此问题：可能会导致失败的 Windows Server 备份作业。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-for-manual-startup"></a>若要配置手动启动阻止级别备份引擎服务  
  
1.  打开服务主机。  
  
2.  在服务列表中，双击**阻止级别备份引擎服务**。  
  
3.  对于**启动类型**、选择**手册**，然后单击**应用**。  
  
### <a name="the-logon-account-for-the-block-level-backup-engine-service-is-not-set-to-use-the-local-system-account"></a>登录帐户的阻止级别备份引擎服务未设置为使用本地系统帐户  
 **问题：**阻止级别备份引擎服务未设置为默认登录帐户使用系统本地帐户。  
  
 **影响：**如果阻止级别备份引擎服务未使用本地系统作为默认登录帐户，你可能会遇到相关权限错误。 这些错误可以成功完成阻止 Windows Server 备份作业。  
  
 **解决方法：**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-to-use-local-system-as-the-default-logon-account"></a>若要将阻止级别备份引擎服务要用作默认登录帐户的本地系统配置  
  
1.  打开服务主机。  
  
2.  在服务列表中，双击**阻止级别备份引擎服务**。  
  
3.  在**服务属性**页上，单击**登录**选项卡。  
  
4.  选择**系统本地帐户**选项，然后依次单击**应用**。  
  
5.  重启该服务。  
  
### <a name="the-common-name-on-the-certificate-that-is-bound-to-the-wss-certificate-web-service-website-does-not-match-the-server-name"></a>上绑定到 WSS 证书 Web 服务的网站的证书的常见名称不符服务器名称  
 **问题：**非有效证书绑定 WSS 证书 Web 服务网站 IIS 中。 在该证书的常见名称不符服务器的名称。  
  
 **影响：**非有效证书绑定到 WSS 证书 Web 服务的网站时，如果连接向导可能无法正常工作。  
  
 **解决方法：**  
  
##### <a name="to-configure-a-valid-certificate-for-the-wss-certificate-web-service"></a>若要配置有效证书 WSS 证书 Web 服务  
  
1.  打开 Internet 信息服务 (IIS) 管理器在服务器上。  
  
2.  在 IIS 管理器中，展开你服务器的名称，然后单击**站点**。  
  
3.  右键单击**WSS 证书 Web 服务**，然后单击**编辑绑定**。  
  
4.  在**站点绑定**，单击**HTTPS**，然后单击**编辑**。  
  
5.  在**编辑站点绑定**，对于**SSL 证书**，选择你服务器同名的证书。  
  
6.  如果多个证书条目有与同名服务器，请单击**视图**以确定哪些证书是否有效，然后选择相应的证书。  
  
### <a name="there-appears-to-be-a-problem-with-the-certificate-binding-for-the-remote-desktop-gateway-service"></a>似乎那里问题的远程桌面网关服务的证书绑定  
 **问题：**远程桌面网关送修证书似乎绑定不正确。  
  
 **影响：**如果未正确配置的远程桌面网关服务证书，用户无法连接到远程访问 Web。  
  
 **解决方法：**  
  
##### <a name="to-fix-the-binding-for-the-remote-desktop-gateway-service"></a>若要修复的远程桌面网关服务绑定  
  
-   打开 command prompt 下以 administrator 身份，然后输入以下命令：  
  
    ```  
    REG ADD HKLM\SYSTEM\CurrentControlSet\services\HTTP\Parameters\SslBindingInfo\0.0.0.0:443  /v DefaultFlags /t REG_DWORD /d 1 /f  
    net stop tsgateway  
    net start tsgateway  
    ```  
  
     有关详细信息，请参阅[如何管理 Windows Server Essentials（知识库文章 2472211）中的远程桌面网关服务](https://support.microsoft.com/kb/2472211)(https://support.microsoft.com/kb/2472211)。

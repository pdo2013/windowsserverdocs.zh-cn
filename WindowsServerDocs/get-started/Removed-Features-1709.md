---
title: 从 Windows Server（版本 1709）开始删除或计划替换的功能
description: 各版本中已删除或计划删除的特性和功能。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/05/2018
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 37970f3bee2070cffc77bff855a8f28641196b24
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452866"
---
# <a name="features-removed-or-planned-for-replacement-starting-with-windows-server-version-1709"></a>从 Windows Server 版本 1709 开始删除或计划替换的功能

>适用于：Windows Server 版本 1709

以下是 Windows Server 版本 1709 中已从该版本的产品中删除或开始考虑在后续版本中可能替换的特性和功能列表。 本文档面向在商业环境中更新操作系统的 IT 专业人员。 **此列表可能会发生在后续版本中的更改，可能不包括每个受影响的功能。** 

## <a name="features-removed-from-windows-server-version-1709"></a>从 Windows Server 版本 1709 中删除的功能
Windows Server 版本 1709 包含 Windows Server 2016 中存在的相同功能。 但是，此版本提供的安装选项确实不同于 Windows Server 2016 提供的安装选项：

- 作为半年频道版本，Windows Server 版本 1709 仅提供服务器核心安装选项。 有关详细信息，请参阅[比较的维护服务频道](../get-started-19/servicing-channels-19.md)。
- 从该版本开始，Nano Server 不可用作可安装的主机操作系统。 相反，Nano Server 可用作容器操作系统。 请参阅 [Windows Server 版本 1709 中对 Nano Server 所做的更改](nano-in-semi-annual-channel.md)。
- 从此版本开始，服务器消息块 (SMB) 版本 1 不能再安装默认情况下。 有关详细信息，请参阅[SMBv1 未安装在 Windows 10 Fall Creators Update 和 Windows Server，版本 1709年及更高版本中默认](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows)。


## <a name="features-being-considered-for-replacement-starting-with-subsequent-releases"></a>考虑从后续版本开始替换的功能

正在考虑从 Windows Server 版本 1709 之后的版本开始替换以下特性和功能。 最终，它们可能会从已安装的产品映像中完全删除，并被其他特性或功能（或来自其他来源的可安装项）所替换，但它们在此版本中仍然可用，有时此版本中会删除某些功能。 现在，你应该开始计划对依赖于这些功能的任何应用程序、代码或用法使用其他方法或替代功能。

如果你有关于建议替换其中任何一项功能方面的反馈要与大家共享，可以使用[反馈中心应用](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)。 即使此应用在 Windows 10 上运行，你也可以使用它向我们发送关于 Windows Server 产品（和文档）的反馈。

### <a name="iis-6-management-compatibility"></a>IIS 6 管理兼容性
正在考虑替换的特定功能如下：

- IIS 6 元数据库兼容性（Web 元数据库）
- IIS 6 管理控制台（Web 旧管理控制台）
- IIS 6 脚本工具（Web 旧脚本）
- IIS 6 WMI 兼容性 (Web-WMI)

你应该直接或通过使用 Microsoft.Web.Administration 命名空间等工具开始将管理脚本迁移到基于文件的 IIS 目标配置，而不是迁移 IIS 6 元数据库兼容性（它充当基于 IIS 6 的元数据库脚本与 IIS 7 或更高版本所使用的基于文件的配置之间的模拟层）。

你还应该从 IIS 6.0 或更早版本中启动迁移，并移到最新版本的 IIS，此最新版本始终可在最新版本的 Windows Server 中获得。


### <a name="iis-digest-authentication"></a>IIS 摘要式身份验证
已计划替换此身份验证方法。 相反，你应该开始使用其他身份验证方法，如客户端证书映射（请参阅[配置一对一客户端证书映射](https://docs.microsoft.com/iis/manage/configuring-security/configuring-one-to-one-client-certificate-mappings)）或 Windows 身份验证（请参阅[应用程序设置](https://docs.microsoft.com/iis-administration/configuration/appsettings.json)）。

### <a name="internet-storage-name-service-isns"></a>Internet 存储名称服务 (iSNS)
正在考虑替换 iSNS。 服务器消息块 (SMB) 特性提供与其他特性基本相同的功能。 有关此特性的背景信息，请参阅[服务器消息块概述](https://technet.microsoft.com/library/hh831795(v=ws.11).aspx)。

### <a name="rsaaes-encryption-for-iis"></a>适用于 IIS 的 RSA/AES 加密 
此加密方法正在考虑针对替换是因为卓越的加密 API:下一代 (CNG) 方法已可用。 若要了解有关 CNG 加密的详细信息，请参阅[关于 CNG](https://msdn.microsoft.com/library/windows/desktop/aa375276(v=vs.85).aspx)。

### <a name="windows-powershell-20"></a>Windows PowerShell 2.0
此早期版本的 Windows PowerShell 已被一些较新的版本所取代。 为了获得最佳功能和性能，请迁移到 Windows PowerShell 5.0 或更高版本。 请参阅 [PowerShell 文档](https://docs.microsoft.com/powershell/index?view=powershell-5.1)以获取大量信息。


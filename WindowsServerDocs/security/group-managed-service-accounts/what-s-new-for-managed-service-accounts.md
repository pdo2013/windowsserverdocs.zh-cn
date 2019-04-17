---
title: "托管的服务帐户中的新增功能"
description: "Windows Server 安全"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2f2a8b6b-c152-4c40-b712-bfabff0e408b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: cac55d04a40c84ce160eb3883d6095a7db0ef3be
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="what39s-new-for-managed-service-accounts"></a>什么 & #39; 的新增功能托管的服务帐户的 s

>适用于：Windows Server（半年通道），Windows Server 2016

此主题以供 IT 专业人员说明的更改功能托管服务帐户引入了组托管服务帐户 (gMSA) 在 Windows Server 2012 和 Windows 8 中。

托管的服务帐户旨在提供服务和任务，如 Windows 服务和 IIS 应用程序池分享自己的域帐户，而不再需要的管理员，要手动管理这些帐户的密码。 它是提供自动密码管理托管的域帐户。

## <a name="versions"></a>在 Windows Server 2012 和 Windows 8 中托管服务帐户中的新增功能
下面介绍了在 Windows Server 2012 和 Windows 8 中的 MSA 功能哪些更改。

### <a name="group-managed-service-accounts"></a>组托管的服务帐户
当为某个域中的服务器配置域帐户，则可以进行身份验证客户端计算机并将其连接到该服务。 以前，仅有两个帐户类型而无需密码管理提供的身份。 但这些帐户类型具有限制：

-   计算机帐户仅限于一个域服务器，并且由计算机管理密码

-   托管的服务帐户仅限于一个域服务器，并且由计算机管理密码。

不能在多个系统共享这些帐户。 因此，你必须定期维护每个服务阻止不需要的密码到期每个系统上的帐户。

**此更改添加哪些值？**

组托管服务帐户解决此问题，因为帐户密码已由 Windows Server 2012 域控制器，可通过多个 Windows Server 2012 系统。 通过允许 Windows 处理为这些帐户的密码管理，最小化服务帐户的管理费用。

**内容的工作方式不同？**

在计算机上运行的 Windows Server 2012 或 Windows 8，可以创建和管理通过 Service 控制管理器中，以便通过服务器电场的日落，如部署了大量的服务，实例 MSA 一组可以管理从一台服务器。 工具和实用程序，用于管理托管服务帐户、 如 IIS 应用程序池管理器中，可以使用组托管服务帐户。 域管理员可以委派服务管理的服务管理员，可以管理托管服务帐户或一组托管服务帐户的整个生命周期。 现有的客户端计算机将能够通过不知道哪个向进行身份验证的服务实例任何此类服务身份验证。

### <a name="interoperability"></a>已删除或不建议使用功能
Windows Server 2012、 Windows PowerShell cmdlet 管理组托管服务帐户而不是服务器托管服务帐户到默认值。

## <a name="see-also"></a>请参阅

-   [组托管的服务帐户概述](group-managed-service-accounts-overview.md)

-   [Active Directory 域名服务概述](active-directory-domain-services-overview.md)

-   [了解托管的服务帐户:、 实现，最佳做法，和疑难解答](http://blogs.technet.com/b/askds/archive/20../managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)



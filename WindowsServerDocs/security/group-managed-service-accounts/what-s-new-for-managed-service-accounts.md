---
title: What's New for Managed Service Accounts
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: de4d64e3dbe4bc7c7cba32f066a696636632224d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403724"
---
# <a name="what39s-new-for-managed-service-accounts"></a>托管&#39;服务帐户的新增功能

>适用于：Windows Server（半年频道）、Windows Server 2016

面向 IT 专业人士的本主题介绍在 Windows Server 2012 和 Windows 8 中引入了组托管服务帐户（gMSA）后托管服务帐户的功能更改。

该托管服务帐户旨在提供诸如 Windows 服务和 IIS 应用程序池的服务和任务以共享其自己的域帐户，同时无需管理员手动管理这些帐户的密码。 它是一种托管的域帐户，提供了自动密码管理。

## <a name="versions"></a>Windows Server 2012 和 Windows 8 中托管服务帐户的新增功能
下面介绍了 Windows Server 2012 和 Windows 8 中的 MSA 功能更改。

### <a name="group-managed-service-accounts"></a>组托管服务帐户
当在域中为服务器配置域帐户时，客户端计算机可以对服务进行身份验证并连接到该服务。 以前，只有两种帐户类型提供了无需密码管理的身份。 但是，这些帐户类型具有以下一些限制：

-   计算机帐户将仅限于一个域服务器，并且密码由该计算机管理。

-   托管服务帐户将仅限于一个域服务器，并且密码由该计算机管理。

这些帐户将无法在多个系统之间进行共享。 因此，你必须定期在每个系统上维护各服务的帐户，以避免出现意料之外的密码过期。

**这一更改增添了什么价值？**

组托管服务帐户可解决此问题，因为帐户密码由 Windows Server 2012 域控制器进行管理，并且可以由多个 Windows Server 2012 系统进行检索。 通过允许 Windows 处理这些帐户的密码管理，将最大程度降低对服务帐户的管理开销。

**工作原理的不同之处是什么？**

在运行 Windows Server 2012 或 Windows 8 的计算机上，可以通过服务控制管理器创建和管理组 MSA，以便可从一台服务器管理服务的众多实例，如通过服务器场部署。 用于管理托管服务帐户的工具和实用程序（如 IIS 应用程序池管理器）可与组托管服务帐户配合使用。 域管理员可以将服务管理委派给服务管理员，或者可以管理托管服务帐户或组托管服务帐户的整个生命周期。 现有客户端计算机将能对任何此类服务进行身份验证，且无需了解进行身份验证的是哪个服务实例。

### <a name="interoperability"></a>删除或弃用的功能
对于 Windows Server 2012，Windows PowerShell cmdlet 默认为管理组托管服务帐户，而不是服务器托管服务帐户。

## <a name="see-also"></a>请参阅

-   [组托管服务帐户概述](group-managed-service-accounts-overview.md)

-   [Active Directory 域服务概述](active-directory-domain-services-overview.md)

-   @no__t 0Managed 服务帐户：了解、实现、最佳实践和疑难解答 @ no__t-0



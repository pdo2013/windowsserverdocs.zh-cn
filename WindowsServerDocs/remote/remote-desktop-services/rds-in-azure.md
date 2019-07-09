---
title: 使用 ARM 和 Azure 市场无缝部署 RDS
description: 了解如何使用 ARM 模板和 Azure 市场在 Azure 中创建小型 RDS 部署。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f72ceb6-6f90-48f6-bfc3-bdad63984ce7
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 02/10/2017
ms.openlocfilehash: 218e61e5ebe110502ebe139b27607bfeff104fde
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "63712624"
---
# <a name="seamlessly-deploy-rds-with-arm-and-azure-marketplace"></a>使用 ARM 和 Azure 市场无缝部署 RDS

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2

远程桌面服务 (RDS) 是经济高效地托管 Windows 桌面和应用程序的首选平台。 可以使用 [Azure 市场产品/服务](#basic-rds-through-the-azure-marketplace)或[快速启动模板](#customized-rds-using-quickstart-templates)在 Azure IaaS 部署中快速创建 RDS。 Azure 市场可为你创建测试域，从而成为用于测试和概念证明的简单易用机制。 另一方面，快速启动模板使你可以使用现有域，从而成为用于构建生产环境的卓越工具。 设置后，可以使用适用于 Windows、Mac、iOS 和 Android 的 Microsoft 远程桌面应用，从各种平台和设备连接到已发布的桌面和应用程序。

## <a name="basic-rds-through-the-azure-marketplace"></a>通过 Azure 市场实现的基本 RDS

通过 Azure 市场创建部署是启动并运行的最快方式。 完成所有内容后，环境会类似于[基本 RDS 体系结构](desktop-hosting-logical-architecture.md#basic-deployment)。 产品/服务可创建所需的所有 RDS 组件（你只需提供一些信息）。 

在部署市场产品/服务时需要提供以下信息：
- 管理员用户名和密码。 这是将管理部署的新用户。
- DNS 名称和 AD 域名。 这些是创建的新资源。 确保名称有意义。
- VM 大小。 可以选择要用于 RDSH 终结点的 VM 大小。 还可以在初始部署之后手动更改大小，以帮助针对工作负载和成本优化 VM。

使用以下这些步骤可从 Azure 市场创建资源占用量较少的 RDS 部署： 

1. 启动 Azure 市场 RDS 部署：
   1. 登录 [Azure 门户](https://portal.azure.com)。
   2. 单击“新建”  以添加部署。
   3. 在搜索字段中输入“RDS”，并按 Enter。
   4. 单击“远程桌面服务(RDS) - 基本 - 开发/测试”  ，然后单击“创建”  。
   5. 按照门户中的步骤创建和部署 RDS。 会添加关键配置详细信息，如上面所列的信息。 
2. 连接到部署。 部署完成后，检查输出部分以完成最后的步骤并连接到部署。
   1. 在测试设备上下载并运行[此 PowerShell 脚本](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328)以安装连接到 RDS 部署所需的任何证书。 
   
      仅在测试阶段才需要此步骤。 处于生产期间在 Azure 中部署 RDS 部署时，确保遵循最佳做法，如购买公开受信任的 SSL 证书并在 Web 服务器上使用。

   2. 出现提示时，登录到 Azure 帐户。 选择为此新部署创建的 Azure 订阅、资源组和公用 IP 地址。
   3. 脚本完成后，RD Web 页面将在默认浏览器中启动。 可以通过将页面的 URL 与部署过程中提供的 DNS 地址进行比较，来仔细检查 RD Web 页面。 
   
      使用在部署过程中创建的管理员凭据登录以查看为你发布的默认桌面。 还可以向用户发送 RD Web 站点以测试其桌面和应用程序。

      > [!TIP]
      > 忘记域名或管理员用户？ 可以返回到门户中的新资源组，单击“部署”  ，然后查看输入的参数。

现在具有一个 RDS 部署，便可以[添加和管理用户](rds-user-management.md)。

## <a name="customized-rds-using-quickstart-templates"></a>使用快速启动模板的自定义 RDS

可以使用 Azure 资源管理器模板在 Azure 中部署 RDS。 如果需要基本 RDS 部署，但是具有要使用的现有组件（如 AD），则这特别有用。 与市场产品/服务不同，可以进行进一步自定义，如在虚拟网络上使用现有 AD、将自定义操作系统映像用于 RDSH VM 以及针对 RDS 组件对高可用性进行分层。 向每个组件添加高可用性之后，环境会类似于[高度可用的 RDS 体系结构](desktop-hosting-logical-architecture.md#highly-available-deployment)。

使用以下这些步骤可通过 Azure RDS 模板创建资源占用量较少的 RDS 部署： 

1. 挑选 Azure 快速启动模板：
   1. 转到 [RDS Azure 快速启动模板](https://aka.ms/rdautomation)站点。
   2. 选择与尝试执行的操作匹配的模板。 确保满足该特定模板的任何先决条件。 （例如，如果要将自定义映像用于 VM，请确保已将该映像上传到 Azure 存储帐户。）
   3. 单击“部署到 Azure”  。
   4. 需要在 Azure 门户中提供一些详细信息（如管理员用户名、AD 域名）。 这因所选模板而异。
   5. 单击“购买”  。
2. 连接到部署。 
   1. 在测试设备上下载并运行[此 PowerShell 脚本](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328)以安装连接到 RDS 部署所需的任何证书。 
   
      仅在测试阶段才需要此步骤。 处于生产期间在 Azure 中部署 RDS 部署时，确保遵循最佳做法，如购买公开受信任的 SSL 证书并在 Web 服务器上使用。

   2. 出现提示时，登录到 Azure 帐户。 选择为此新部署创建的 Azure 订阅、资源组和公用 IP 地址。
   3. 脚本完成后，RD Web 页面将在默认浏览器中启动。 可以通过将页面的 URL 与部署过程中提供的 DNS 地址进行比较，来仔细检查 RD Web 页面。 
   
      使用在部署过程中创建的管理员凭据登录以查看为你发布的默认桌面。 还可以向用户发送 RD Web 站点以测试其桌面和应用程序。

      > [!TIP]
      > 忘记域名或管理员用户？ 可以返回到门户中的新资源组，单击“部署”  ，然后查看输入的参数。

现在具有一个 RDS 部署，便可以[添加和管理用户](rds-user-management.md)。

---
title: 使用 ARM 和 Azure 应用商店无缝部署 RDS
description: 了解如何在 Azure 中使用 ARM 模板和 Azure Marketplace 创建小型的 RDS 部署。
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
ms.openlocfilehash: e4de3d4ac14a0dbc5500fd7ab8bd8f1568f3da53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823078"
---
# <a name="seamlessly-deploy-rds-with-arm-and-azure-marketplace"></a>使用 ARM 和 Azure 应用商店无缝部署 RDS

>适用于：Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2

远程桌面服务 (RDS) 是经济高效地托管 Windows 桌面和应用程序所选择的平台。 可以使用[Azure Marketplace 产品/服务](#basic-rds-through-the-azure-marketplace)或[快速入门模板](#Customized-RDS-using-Quickstart-templates)快速在 Azure IaaS 部署中创建 RDS。 Azure 应用商店测试域为用户创建，使其简单易用机制用于测试和概念证明。 快速入门模板，但是，可以使用现有域，使其强大的工具来构建出生产环境。 设置完成后，你可以连接到已发布的桌面和应用程序从各种平台和设备，使用适用于 Windows、 Mac、 iOS 和 Android 的 Microsoft 远程桌面应用。

## <a name="basic-rds-through-the-azure-marketplace"></a>通过 Azure Marketplace 的基本 RDS

创建通过 Azure Marketplace 部署是最快的方法来启动和运行。 完成的所有内容后，你的环境将如下所示[基本 RDS 体系结构](desktop-hosting-logical-architecture.md#basic-deployment)。 该产品/服务创建所有 RDS 组件的所需-你需要做的就是为一些信息。 

你将需要在将 Marketplace 产品/服务时提供以下信息：
- 管理员用户名和密码。 这是将管理部署的新用户。
- DNS 名称和 AD 域名。 以下是新创建的资源。 请确保名称是有意义。
- VM 大小。 您可以选择要使用 RDSH 终结点的 Vm 的大小。 如何帮助您优化工作负荷和成本的 Vm 在初始部署后，可以手动更改大小。

使用以下步骤从 Azure Marketplace 创建资源占用量少的 RDS 部署： 

1. 启动 Azure Marketplace RDS 部署：
   1. 登录到 [Azure 门户](https://portal.azure.com)。
   2. 单击**新建**添加你的部署。
   3. 在搜索字段中键入"RDS"，然后按 Enter。
   4. 单击**远程桌面服务 (RDS)-基本-开发/测试**，然后单击**创建**。
   5. 请按照步骤在门户中创建和部署 rds。 你将添加密钥配置的详细信息，如上面所列的信息。 
2. 连接到你的部署。 部署完成后，检查最后几个步骤完成，然后连接到你的部署的输出部分。
   1. 下载并运行[此 PowerShell 脚本](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328)上测试设备以安装所需连接到 RDS 部署任何证书。 
   
      在测试阶段才需要此步骤。 在 Azure 中的 RDS 部署在生产环境中时，请确保按照最佳做法，例如采购和 web 服务器上使用公开受信任的 SSL 证书。

   2. 出现提示时，登录到 Azure 帐户。 选择 Azure 订阅、 资源组和为此新的部署创建的公共 IP 地址。
   3. 完成脚本后，RD Web 页将在默认浏览器中启动。 可以通过比较在部署期间提供的 DNS 地址到页的 URL，请仔细检查 RD Web 页。 
   
      若要查看默认桌面发布，以便你的部署期间创建的管理员凭据登录。 您还可以将用户发送 RD 网站来测试其台式计算机和应用程序。

      > [!TIP]
      > 忘记了域的名称或管理员用户？ 你可以转回在门户中的新资源组，单击**部署**，然后查看你输入的参数。

现在，你已有的 RDS 部署，你可以[添加和管理用户](rds-user-management.md)。

## <a name="customized-rds-using-quickstart-templates"></a>使用快速启动模板的自定义的 RDS

Azure 资源管理器模板可用于部署在 Azure 中的 RDS。 如果您想的基本 RDS 部署，但具有你想要使用的现有组件 （例如 AD)，这是特别有用。 与不同的 Marketplace 产品/服务，您可以进行进一步自定义项，如在虚拟网络上，使用现有 AD RDSH 虚拟机和高可用性 RDS 组件上的分层使用自定义 OS 映像。 后向每个组件添加的高可用性，你的环境看起来类似于[高度可用 RDS 体系结构](desktop-hosting-logical-architecture.md#highly-available-deployment)。

使用以下步骤以使用 Azure RDS 模板创建资源占用量少的 RDS 部署： 

1. 选择 Azure 快速入门模板：
   1. 转到[RDS Azure 快速入门模板](https://aka.ms/rdautomation)站点。
   2. 选择与要执行匹配的模板。 请确保满足该特定模板的任何系统必备组件。 （例如，如果要为 Vm 使用的自定义映像，请确保您具有已将该映像上载到 Azure 存储帐户。）
   3. 单击**部署到 Azure**。
   4. 你将需要提供一些详细信息 （例如管理员用户名，AD 域名），在 Azure 门户中。 此值变化根据所选的模板。
   5. 单击**采购**。
2. 连接到你的部署。 
   1. 下载并运行[此 PowerShell 脚本](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328)上测试设备以安装所需连接到 RDS 部署任何证书。 
   
      在测试阶段才需要此步骤。 在 Azure 中的 RDS 部署在生产环境中时，请确保按照最佳做法，例如采购和 web 服务器上使用公开受信任的 SSL 证书。

   2. 出现提示时，登录到 Azure 帐户。 选择 Azure 订阅、 资源组和为此新的部署创建的公共 IP 地址。
   3. 完成脚本后，RD Web 页将在默认浏览器中启动。 可以通过比较在部署期间提供的 DNS 地址到页的 URL，请仔细检查 RD Web 页。 
   
      若要查看默认桌面发布，以便你的部署期间创建的管理员凭据登录。 您还可以将用户发送 RD 网站来测试其台式计算机和应用程序。

      > [!TIP]
      > 忘记了域的名称或管理员用户？ 你可以转回在门户中的新资源组，单击**部署**，然后查看你输入的参数。

现在，你已有的 RDS 部署，你可以[添加和管理用户](rds-user-management.md)。

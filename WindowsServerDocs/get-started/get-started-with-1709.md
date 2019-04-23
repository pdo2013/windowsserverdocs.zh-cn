---
title: Windows Server 版本 1709 简介
description: 如何获取、安装和激活
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: high
ms.date: 12/5/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9cf87597-b15d-4f43-8aa1-91e60367f011
ms.openlocfilehash: 96f098457b9bac4541421c7889aefb030e3d8804
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868798"
---
# <a name="introducing-windows-server-version-1709"></a>Windows Server 版本 1709 简介

>适用于：Windows Server（半年频道）

**Windows Server 版本 1709年是新的半年频道中的第一个版本。** 

## <a name="what-the-semi-annual-channel-is--and-isnt"></a>有关半年频道的说明
作为以此新频道发布的第一个版本，Windows Server 版本 1709 *不*是 Windows Server 2016 的“更新”或“Service Pack”， 而是**新版本频道**中每年发布两次的服务器版本的第一版，该版本面向以“云节奏”移动的客户（如对快速开发周期有要求的客户），或者紧跟最新 Hyper-V 投资的托管商。 每个在该频道中发布的版本都享受 18 个月的支持服务（自初始发布开始）。 若要详细了解半年频道并获取**有关决定加入哪个频道的提示**（或留在哪个频道的提示），请参阅[半年频道概述](semi-annual-channel-overview.md)。


**当前 Long-Term Servicing Channel (LTSC) 产品是 Windows Server 2016**。 如果你需要服务器操作系统具有长期稳定性和可预测性，以支持传统的工作负载和应用程序，则 LTSC 最适合。 如果你想坚持使用 LTSC，则应该安装（或继续使用）Windows Server 2016，该软件可在服务器核心模式下或在带桌面体验的服务器模式下安装。 有关详细信息，请参阅 [Windows Server 2016 入门](https://docs.microsoft.com/windows-server/get-started/server-basics)。


## <a name="whats-different-about-1709"></a>1709 有哪些不同之处？

Windows Server 版本 1709 在服务器核心模式下运行。 这意味着没有图形用户界面，以便你进行远程管理。 但是，它可提供强大的优势，如硬件要求较低、攻击面更小并且减小了更新需求。 如果你未使用过服务器核心，则[管理服务器核心服务器](../administration/server-core/server-core-manage.md)可帮助你适应此环境。 [管理 Windows Server 2016](../administration/manage-windows-server.md) 介绍了用于远程管理服务器的各种选项。

[Windows Server 版本 1709 中的新增功能](whats-new-in-windows-server-1709.md)介绍了 Windows Server 版本 1709 中添加的新特性和功能。

### <a name="why-does-windows-server-version-1709-offer-only-the-server-core-installation-option"></a>为什么 Windows Server 版本 1709 仅提供服务器核心安装选项？
我们在规划 Windows Server 的每个版本时所采取的最重要步骤之一就是聆听客户的反馈意见 - 你如何使用 Windows Server？ 哪些新功能对你的 Windows Server 部署影响最大，扩展后，哪些对你的日常业务影响最大？ 你的反馈告诉我们，当务之急是尽快并且尽可能高效地提供创新。 同时，对于以最快速度进行创新的那些客户，你已经告诉我们，你主要将命令行脚本与 PowerShell 配合使用来管理数据中心，因此，对于桌面 GUI 的需求并不强烈（在安装具有桌面体验的 Windows Server 时提供）。 通过专注于服务器核心安装选项，我们可以专门为这些创新投入更多资源，同时还保留传统的 Windows Server 平台功能和应用程序兼容性。 如果你有关于这方面的反馈或与 Windows Server 和我们的未来版本相关的其他问题，可以通过[反馈中心](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)提出建议和意见。


### <a name="what-about-nano-server"></a>Nano Server 的情况如何？
Nano Server 可用作容器操作系统。 有关详细信息，请参阅[在 Windows Server 半年频道中对 Nano Server 所做的更改](nano-in-semi-annual-channel.md)。

## <a name="additional-information-about-this-release"></a>有关该版本的其他信息
若要全面了解有关 Windows Server 版本 1709 的重要事宜，你还应该在安装之前先查看以下主题：

- 运行它需要什么硬件？ 请参阅[系统要求](system-requirements.md)；此版本的系统要求与 Windows Server 2016 的系统要求相同。
- 已添加了哪些新的特性和功能？ 请参阅 [Windows Server 版本 1709 中的新增功能](whats-new-in-windows-server-1709.md)。
- 删除了哪些内容？ 请参阅[从 Windows Server（版本 1709）开始删除或计划替换的功能](Removed-Features-1709.md)
- 需要解决此版本特有的哪些问题？ 请参阅[发行说明 - Windows Server 版本 1709 中的重要问题](server-1709-relnotes.md)


## <a name="where-to-obtain-windows-server-version-1709"></a>在哪里获取 Windows Server 版本 1709

此版本应以干净安装的方式进行安装。

- VLSC:具有批量许可客户[软件保障](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx)可以获取此版本通过转到[批量许可服务中心](https://www.microsoft.com/Licensing/servicecenter/default.aspx)，然后单击**登录**。 然后，单击**下载和密钥**并搜索此版本。 

- [Microsoft Azure](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview) 中也提供了 Windows Server 版本 1709。

- 中的参与者**Visual Studio 订阅：** 如果您已参与 Visual Studio 订阅，则可以获取 Windows Server，通过转到版本 1709年[Visual Studio 订阅者下载页](https://my.visualstudio.com/downloads?pid=2347)和那里完成的下载可用。 如果你还不是订阅者，请转到 [Visual Studio 订阅](https://www.visualstudio.com/subscriptions/)进行注册，然后访问上方所述的 [Visual Studio 订阅者下载页面](https://my.visualstudio.com/downloads?pid=2347)。 通过 Visual Studio 订阅获得的版本仅用于开发和测试。




## <a name="activating-windows-server-version-1709"></a>激活 Windows Server 版本 1709

- 如果你从批量许可服务中心获取了此版本，则可以使用你的 Windows Server 2016 激活密钥激活它。
- 如果你使用的是 Microsoft Azure，则此版本应该会自动激活。
- 如果你通过 Visual Studio 订阅获取此版本，则它应该会自动激活。
---
title: QoS 策略方案
description: 本主题提供了服务质量 (QoS) 策略方案，演示如何使用组策略为优先处理的特定应用程序和 Windows Server 2016 中的服务的网络流量。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2a24ce3213b1aa1c66a438d278f7499b6d53bf6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834348"
---
# <a name="qos-policy-scenarios"></a>QoS 策略方案

>适用于：Windows 服务器 （半年频道），Windows Server 2016

你可以使用本主题以查看假设方案，展示了如何，何时以及为何要使用 QoS 策略。

本主题中的两个方案是：

1. 确定网络流量的优先级为业务线应用程序
2. 确定网络流量的优先级为 HTTP 服务器应用程序

>[!NOTE]
>本主题的某些部分包含可用于执行所述的操作的常规步骤。 有关更多详细管理 QoS 策略的说明，请参阅[管理 QoS 策略](qos-policy-manage.md)。

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>方案 1：确定网络流量的优先级为业务线应用程序

在此方案中，IT 部门具有他们可以使用 QoS 策略来完成的几个目标：

- 提供更好的网络性能的关键任务\-关键应用程序。
- 在使用特定的应用程序时提供更好用户的一组重要的网络性能。
- 确保公司\-宽数据备份应用程序不会通过一次使用太多带宽影响网络性能。

IT 部门决定配置 QoS 策略，以确定特定的应用程序通过使用区分服务代码点的优先级\(DSCP\)值分类网络流量，并配置其路由器以提供优先更高优先级的流量的处理方式。 

>[!NOTE]
>DSCP 的详细信息，请参阅明**定义 QoS 优先级通过区分服务代码点**主题中的[服务质量 (QoS) 策略](qos-policy-top.md)。

除了 DSCP 值，QoS 策略可以指定中止值等级。 限制起的所有匹配项 QoS 策略为特定发送速率的出站流量限制。

### <a name="qos-policy-configuration"></a>QoS 策略配置

若要完成的三个单独目标，IT 管理员决定创建三个不同的 QoS 策略。

#### <a name="qos-policy-for-lob-app-servers"></a>LOB 应用程序服务器的 QoS 策略

第一个任务关键型\-IT 部门为其创建 QoS 策略的关键应用程序是一家公司\-宽企业资源规划\(ERP\)应用程序。 在多台计算机都在运行 Windows Server 2016 的托管 ERP 应用程序。 在 Active Directory 域服务，这些计算机是组织单位的成员\(OU\)创建的业务线\(LOB\)应用程序服务器。 客户端\-ERP 应用程序的端组件安装在运行 Windows 10 和 Windows 8.1 的计算机上。

IT 管理员在组策略中，选择组策略对象\(GPO\)时应用的 QoS 策略。 IT 管理员通过使用 QoS 策略向导，创建名为的 QoS 策略"LOB 服务器策略"，它指定为高\-优先级 DSCP 值 44 的所有应用程序、 任何 IP 地址、 TCP 和 UDP 和端口号。

QoS 策略仅应用于 LOB 服务器通过将 GPO 链接到包含仅这些服务器，通过组策略管理控制台的 OU \(GPMC\)工具。 此初始服务器 LOB 策略适用高\-优先级 DSCP 值时计算机发送网络流量。 此 QoS 策略更高版本可以进行编辑\(中的组策略对象编辑器工具\)包括 ERP 应用程序的端口号，这可以限制策略来仅在使用指定的端口号时才适用。

#### <a name="qos-policy-for-the-finance-group"></a>将财务组的 QoS 策略

在公司内的多个组访问 ERP 应用程序，同时财务组取决于此应用程序的客户，在处理时，组需要从应用的性能一贯较高。

若要确保财务组可以支持客户，QoS 策略必须将这些用户的流量分类为高优先级。 但是，财务组的成员使用 ERP 应用程序以外的应用程序时，不应该应用该策略。 

因此，IT 部门定义名为的第二个 QoS 策略"客户端 LOB 策略"组策略对象编辑器工具适用 DSCP 值为 60，当财务用户组运行 ERP 应用程序中。

#### <a name="qos-policy-for-a-backup-app"></a>备份应用的 QoS 策略

单独的备份应用程序的所有计算机上运行。 若要确保备份应用程序的流量不使用所有可用的网络资源，IT 部门创建的备份数据策略。 此备份策略指定 DSCP 值为 1 可执行文件名称可查看备份应用，这是基于**backup.exe**。 

第三个 GPO 创建并部署为域中的所有客户端计算机。 备份应用程序发送的数据，只要被应用的低优先级 DSCP 值，即使它来自财务部门中的计算机。
  
>[!NOTE]
>不使用 QoS 策略的网络流量将发送 DSCP 值为 0。

### <a name="scenario-policies"></a>方案策略

下表总结了此方案中的 QoS 策略。
  
|策略名称|DSCP 值|速率限制|应用到组织单位|描述|  
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[没有策略]|0|无|[没有部署]|最佳工作量 （默认值） 的未分类的通信量的处理方式。|  
|备份数据|1|无|所有客户端|应用此大容量数据的低优先级 DSCP 值。|  
|LOB 服务器|44|无|计算机 OU 的 ERP 服务器|应用优先级较高 DSCP 的 ERP 服务器流量|  
|客户端 LOB|60|无|财务用户组|适用 ERP 客户端流量的优先级较高的 DSCP|  

>[!NOTE]
>DSCP 值以十进制格式表示。

定义并应用通过使用组策略的 QoS 策略，使用出站网络流量接收策略指定 DSCP 值。 然后，路由器提供差异使用队列来基于这些 DSCP 值的处理方式。 为此 IT 部门的路由器配置四个队列： 优先级较高、 中间优先级、 尽力传送和低优先级。

当流量在到达路由器使用 DSCP 值从"服务器 LOB 策略"和"客户端 LOB 策略，"的数据放入高优先级的队列。 DSCP 值为 0 的流量会收到服务的最大程度级别。 DSCP 值为 1 （从备份应用程序） 的数据包接收低优先级的处理方式。  
  
### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>确定优先级的业务线应用程序的先决条件

若要完成此任务，请确保满足以下要求：

- 所涉及的计算机运行的 QoS\-兼容的操作系统。

- 所涉及的计算机是 Active Directory 域服务的成员\(AD DS\)域，以便可以使用组策略配置。

- TCP/IP 网络设置配置为 DSCP 的路由器\(RFC 2474\)。 有关详细信息，请参阅[RFC 2474](https://www.ietf.org/rfc/rfc2474.txt)。

- 满足管理凭据的要求。

#### <a name="administrative-credentials"></a>管理凭据

若要完成此任务，您必须能够创建和部署组策略对象。
  
#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>设置用于确定优先级的业务线应用程序的测试环境

若要设置测试环境，请完成以下任务。

- 使用客户端和用户分组到组织单位中创建一个 AD DS 域。 有关部署 AD DS 的说明，请参阅[核心网络指南](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide)。

- 配置到差异队列 DSCP 值为基础的路由器。 例如，DSCP 值 44 进入"Platinum"队列和所有其他用户所实施的加权公平排队。

>[!NOTE]
> 可以通过网络捕获使用网络监视器之类的工具查看 DSCP 值。 执行网络捕获后，您可以观察 TOS 字段中捕获的数据。

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>确定优先级的业务线应用程序的步骤

若要确定的业务线应用程序的优先级，完成以下任务：

1. 创建并链接组策略对象\(GPO\)了 QoS 策略。

2. 配置路由器差异将处理的业务应用程序 （通过使用队列） 基于所选的 DSCP 值。 此任务的过程将会因你具有的路由器的类型而异。

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>方案 2：确定网络流量的优先级为 HTTP 服务器应用程序

在 Windows Server 2016 中，基于策略的 QoS 包含功能基于 URL 的策略。 URL 策略让你管理带宽的 HTTP 服务器。

许多企业应用程序是为开发和 Internet Information Services 上托管\(IIS\)从客户端计算机上的浏览器访问 web 服务器和 Web 应用。

在此方案中，假定，你管理的一组 IIS 服务器的主机培训视频的组织的所有员工。 您的目标是确保来自这些视频的服务器的流量不会严重影响您的网络，并确保从网络上的语音和数据流量差分的视频的流量。 

任务是类似于在方案 1 中的任务。 将设计和配置的流量管理设置，例如视频流量的 DSCP 值并根据需要对业务线应用程序限制速率相同。 但在指定的流量，而不是提供应用程序名称，仅输入 HTTP 服务器应用程序将响应的 URL： 例如， https://hrweb/training。
  
> [!NOTE]
>不能使用基于 URL 的 QoS 策略为优先处理运行在 Windows 7 和 Windows Server 2008 R2 之前发布的 Windows 操作系统的计算机的网络通信。

### <a name="precedence-rules-for-url-based-policies"></a>基于 URL 的策略的优先顺序规则

所有以下 Url 有效，可以将 QoS 策略中指定和同时应用于计算机或用户：

- https://video

- https://training.hr.mycompany.com

- https://10.1.10.249:8080/tech  

- https://*/ebooks

但哪一种将收到优先级？ 规则非常简单。 基于 URL 的策略的优先级顺序从左到右读取。 因此，从最高优先级到最低优先级，URL 字段是：
  
[1.URL 方案](#bkmk_QoS_UrlScheme)

[2.URL 主机](#bkmk_QoS_UrlHost)

[3.URL 端口](#bkmk_QoS_UrlPort)

[4.URL 路径](#bkmk_QoS_UrlPath)

详细信息如下所示：

####  <a name="bkmk_QoS_UrlScheme"></a> 1.URL 方案

 `https://` 优先级高于`https://`。

####  <a name="bkmk_QoS_UrlHost"></a> 2.URL 主机

 从最高优先级到最低，它们是：

1. 主机名

2. IPv6 地址

3. IPv4 地址

4. 通配符

对于主机名，更以点分隔的元素 （更深入地） 使用的主机名具有优先级高于具有较少以点分隔的元素的主机名。 例如，在以下主机名：

- video.internal.training.hr.mycompany.com (depth = 6)
  
- selfguide.training.mycompany.com (depth = 4)
  
- 定型 (深度 = 1)
  
- 库 (深度 = 1)
  
 **video.internal.training.hr.mycompany.com**具有最高优先级，并**selfguide.training.mycompany.com**具有下一步最高优先级。 **定型**并**库**共享相同的优先级最低。  
  
####  <a name="bkmk_QoS_UrlPort"></a> 3.URL 端口

特定或一个隐式的端口号的优先级高于通配符端口。

####  <a name="bkmk_QoS_UrlPath"></a> 4.URL 路径

主机名，例如 URL 路径可能包含多个元素。 具有多个元素始终具有更高的优先级比更少。 例如，按优先级别列出以下路径：  

1.  /ebooks/tech/windows/networking/qos

2.  /ebooks/tech/windows/

3.  /ebooks

4.  /

如果用户选择以包含所有子目录和文件遵循的 URL 路径，此 URL 路径将具有较低的优先级比时不进行选择。

用户还可以选择基于 URL 的策略中指定的目标 IP 地址。 目标 IP 地址具有比前面所述的四个 URL 任何的字段较低优先级。
  
### <a name="quintuple-policy"></a>Quintuple 策略

由协议 ID、 源 IP 地址、 源端口、 目标 IP 地址和目标端口指定 Quintuple 策略。 Quintuple 策略始终具有优先权要高于任何基于 URL 的策略。 

如果用户已应用 Quintuple 策略时，新的基于 URL 的策略不会导致冲突上该用户的客户端的任何计算机。

本指南的下一个主题，请参阅[管理 QoS 策略](qos-policy-manage.md)。

本指南中的第一个主题，请参阅[服务质量 (QoS) 策略](qos-policy-top.md)。

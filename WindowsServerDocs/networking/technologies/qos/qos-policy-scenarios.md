---
title: QoS 策略方案
description: 本主题提供服务质量（QoS）策略方案，其中演示了如何使用组策略来确定 Windows Server 2016 中特定应用程序和服务的网络流量的优先级。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0968157532c0b3bd926acbaff4291e27a71de31
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871871"
---
# <a name="qos-policy-scenarios"></a>QoS 策略方案

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用本主题来查看演示使用 QoS 策略的方式、时间和原因的假设方案。

本主题中的两个方案为：

1. 为业务线应用程序的网络流量设置优先级
2. 为 HTTP 服务器应用程序的网络流量设置优先级

>[!NOTE]
>本主题中的某些部分包含执行所述操作时可以执行的常规步骤。 有关管理 QoS 策略的更多详细说明，请参阅[管理 Qos 策略](qos-policy-manage.md)。

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>方案 1：为业务线应用程序的网络流量设置优先级

在这种情况下，IT 部门可以通过使用 QoS 策略来实现多个目标：

- 为任务\-关键型应用程序提供更好的网络性能。
- 使用特定的应用程序时，为一组关键用户提供更好的网络性能。
- 确保公司\-范围数据备份应用程序一次使用过多的带宽不会阻碍网络性能。

IT 部门决定将 QoS 策略配置为设置特定应用程序的优先级，方法是使用\(区分\)服务代码点 DSCP 值来分类网络流量，并将其路由器配置为提供特惠处理更高优先级的流量。 

>[!NOTE]
>有关 DSCP 的详细信息，请参阅主题[服务质量（QoS）策略](qos-policy-top.md)中的 "**通过差分服务码位定义 QoS 优先级**" 部分。

除了 DSCP 值，QoS 策略还可以指定限制速率。 限制的效果是将符合 QoS 策略的所有出站流量限制为特定发送速率。

### <a name="qos-policy-configuration"></a>QoS 策略配置

通过三个不同的目标来完成，IT 管理员决定创建三个不同的 QoS 策略。

#### <a name="qos-policy-for-lob-app-servers"></a>LOB 应用服务器的 QoS 策略

IT 部门创建\-QoS 策略的第一个任务关键应用程序是公司\-范围内的企业资源规划\(ERP\)应用程序。 ERP 应用程序托管在运行 Windows Server 2016 的多台计算机上。 在 Active Directory 域服务中，这些计算机是\(为业务\(线 LOB\)应用\)程序服务器创建的组织单位 OU 的成员。 ERP 应用\-程序的客户端组件安装在运行 Windows 10 并 Windows 8.1 的计算机上。

在组策略中，IT 管理员选择将应用 QoS \(策略\)的组策略对象 GPO。 通过使用 QoS 策略向导，IT 管理员将创建一个名为 "服务器 LOB 策略" 的 QoS 策略，该策略\-指定所有应用程序的高优先级 DSCP 值44、任何 IP 地址、TCP 和 UDP 以及端口号。

QoS 策略仅适用于 LOB 服务器，只是通过组策略管理控制台\(GPMC\)工具将 GPO 链接到仅包含这些服务器的 OU。 当计算机发送网络流量时，此\-初始服务器 LOB 策略会应用高优先级 DSCP 值。 稍后可以在组策略对象编辑器工具\(\)中编辑此 QoS 策略，以包含 ERP 应用程序的端口号，这会将该策略限制为仅在使用指定的端口号时应用。

#### <a name="qos-policy-for-the-finance-group"></a>财务组的 QoS 策略

虽然公司内的多个组访问 ERP 应用程序，但财务小组依赖于此应用程序来处理客户，并且组要求应用程序的性能始终高。

为了确保财务组可以支持其客户，QoS 策略必须将这些用户的流量归类为高优先级。 但是，当财务组的成员使用 ERP 应用程序以外的其他应用程序时，该策略不应应用。 

因此，IT 部门在组策略对象编辑器工具中定义了名为 "客户端 LOB 策略" 的另一个 QoS 策略，该策略在财务用户组运行 ERP 应用程序时应用 DSCP 值60。

#### <a name="qos-policy-for-a-backup-app"></a>备份应用的 QoS 策略

在所有计算机上运行单独的备份应用程序。 为了确保备份应用程序的流量不会使用所有可用的网络资源，IT 部门会创建备份数据策略。 此备份策略根据备份应用程序的可执行文件名称（即**update.exe**）指定 DSCP 值1。 

将为域中的所有客户端计算机创建和部署第三个 GPO。 只要备份应用程序发送数据，就会应用低优先级 DSCP 值，即使该应用程序源自财务部门的计算机。
  
>[!NOTE]
>没有 QoS 策略的网络流量将发送 DSCP 值0。

### <a name="scenario-policies"></a>方案策略

下表汇总了此方案的 QoS 策略。
  
|策略名称|DSCP 值|中止速率|应用于组织单位|描述|  
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[无策略]|0|无|[无部署]|对未分类流量的最佳工作量（默认）处理。|  
|备份数据|1|无|所有客户端|为此大容量数据应用低优先级的 DSCP 值。|  
|服务器 LOB|44|无|适用于 ERP 服务器的计算机 OU|对 ERP 服务器流量应用高优先级 DSCP|  
|客户端 LOB|60|无|财务用户组|对 ERP 客户端流量应用高优先级 DSCP|  

>[!NOTE]
>DSCP 值用十进制格式表示。

使用组策略定义和应用 QoS 策略后，出站网络流量将接收策略指定的 DSCP 值。 然后，路由器将使用队列根据这些 DSCP 值提供差异处理。 对于此 IT 部门，路由器配置了四个队列：高优先级、中间优先级、最大努力和低优先级。

当流量到达带有 DSCP 值来自 "服务器 LOB 策略" 和 "客户端 LOB 策略" 的路由器时，数据将被放入高优先级队列。 DSCP 值为0的流量接收到最大努力级别的服务。 DSCP 值为1（来自备份应用程序）的数据包接收低优先级处理。  
  
### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>对业务线应用程序进行排序的先决条件

若要完成此任务，请确保满足以下要求：

- 涉及的计算机运行的是\-兼容 QoS 的操作系统。

- 涉及的计算机是 Active Directory 域服务\(AD DS\)域的成员，以便可以使用组策略来配置它们。

- Tcp/ip 网络与配置了 DSCP \(RFC 2474\)的路由器一起设置。 有关详细信息，请参阅[RFC 2474](https://www.ietf.org/rfc/rfc2474.txt)。

- 满足管理凭据要求。

#### <a name="administrative-credentials"></a>管理凭据

若要完成此任务，必须能够创建并部署组策略对象。
  
#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>设置测试环境以确定业务线应用程序的优先级

若要设置测试环境，请完成以下任务。

- 创建一个 AD DS 域，其中包含客户端，并将用户分组为组织单位。 有关部署 AD DS 的说明，请参阅[核心网络指南](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide)。

- 根据 DSCP 值将路由器配置为差异 queue。 例如，DSCP 值44进入 "白金" 队列，所有其他队列均为 "加权" 队列。

>[!NOTE]
> 可以通过将网络捕获与网络监视器等工具结合使用来查看 DSCP 值。 执行网络捕获后，可以观察捕获的数据中的 TOS 字段。

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>对业务线应用程序进行排序的步骤

若要确定业务线应用程序的优先级，请完成以下任务：

1. 使用 QoS 策略创建和链接\(组策略\)对象 GPO。

2. 根据所选的 DSCP 值将路由器配置为差异将业务线应用程序（通过使用队列）处理。 此任务的过程取决于你拥有的路由器类型。

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>方案 2：为 HTTP 服务器应用程序的网络流量设置优先级

在 Windows Server 2016 中，基于策略的 QoS 包含基于功能 URL 的策略。 使用 URL 策略可以管理 HTTP 服务器的带宽。

许多企业应用程序是针对 Internet Information Services \(IIS\) web 服务器开发和托管的，而 web 应用则从客户端计算机上的浏览器进行访问。

在此方案中，假设你要管理一组 IIS 服务器，这些服务器为组织的所有员工托管培训视频。 您的目标是确保来自这些视频服务器的流量不会严重影响您的网络，并确保视频流量与网络上的语音和数据流量区分开来。 

该任务类似于方案1中的任务。 你将设计和配置流量管理设置（如视频流量的 DSCP 值）以及与业务线应用程序相同的限制速率。 但在指定流量时，你只需输入 HTTP 服务器应用程序将响应的 URL （例如，） https://hrweb/training ，而不是提供应用程序名称。
  
> [!NOTE]
>对于运行 windows 7 和 Windows Server 2008 R2 之前发布的 Windows 操作系统的计算机，不能使用基于 URL 的 QoS 策略来确定网络流量的优先级。

### <a name="precedence-rules-for-url-based-policies"></a>基于 URL 的策略的优先规则

以下所有 Url 都是有效的，可以在 QoS 策略中指定并同时应用于计算机或用户：

- https://video

- https://training.hr.mycompany.com

- https://10.1.10.249:8080/tech  

- https：//*/ebooks

但哪一个将获得优先级？ 规则非常简单。 基于 URL 的策略按从左到右的读取顺序排列。 因此，从最高优先级到最低优先级，URL 字段是：
  
[1.URL 方案](#bkmk_QoS_UrlScheme)

[2.URL 主机](#bkmk_QoS_UrlHost)

[3.URL 端口](#bkmk_QoS_UrlPort)

[4.URL 路径](#bkmk_QoS_UrlPath)

详细信息如下所示：

####  <a name="bkmk_QoS_UrlScheme"></a> 1.URL 方案

 `https://`的优先级`https://`高于。

####  <a name="bkmk_QoS_UrlHost"></a> 2.URL 主机

 从最高优先级到最低优先级，它们是：

1. 主机名

2. IPv6 地址

3. IPv4 地址

4. 通配符

对于主机名，具有更大点元素（更深层）的主机名的优先级高于具有较少点元素的主机名。 例如，在以下主机名中：

- video.internal.training.hr.mycompany.com （深度 = 6）
  
- selfguide.training.mycompany.com （深度 = 4）
  
- 定型（深度 = 1）
  
- 库（深度 = 1）
  
  **video.internal.training.hr.mycompany.com**具有最高优先级， **selfguide.training.mycompany.com**具有下一个最高优先级。 **定型**和**库**共享的优先级相同。  
  
####  <a name="bkmk_QoS_UrlPort"></a> 3.URL 端口

特定或隐式端口号的优先级高于通配符端口。

####  <a name="bkmk_QoS_UrlPath"></a> 4.URL 路径

与主机名一样，URL 路径可能包含多个元素。 具有多个元素的元素的优先级始终高于较小的一个。 例如，以下路径按优先级列出：  

1.  /ebooks/tech/windows/networking/qos

2.  /ebooks/tech/windows/

3.  /ebooks

4.  /

如果用户选择包含 URL 路径后面的所有子目录和文件，则此 URL 路径的优先级要低于所选内容。

用户还可以选择在基于 URL 的策略中指定目标 IP 地址。 目标 IP 地址的优先级低于前面所述的四个 URL 字段中的任何一个。
  
### <a name="quintuple-policy"></a>五元组策略

五元组策略由协议 ID、源 IP 地址、源端口、目标 IP 地址和目标端口指定。 五元组策略的优先级始终高于基于 URL 的任何策略。 

如果已为用户应用了五元组策略，则基于 URL 的新策略将不会导致该用户的任何客户端计算机冲突。

有关本指南的下一个主题，请参阅[管理 QoS 策略](qos-policy-manage.md)。

有关本指南的第一个主题，请参阅[服务质量（QoS）策略](qos-policy-top.md)。

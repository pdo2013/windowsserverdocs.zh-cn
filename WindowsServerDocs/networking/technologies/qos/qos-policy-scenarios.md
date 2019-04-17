---
title: QoS 策略方案
description: 本主题提供质量服务 (QoS) 策略的情况下，展示如何使用组策略设置优先级网络通信的特定应用程序和 Windows Server 2016 服务。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b635f73cc25552f4c1a05f8db6303f636eda3c54
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-scenarios"></a>QoS 策略方案

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以查看假设方案演示如何操作，请时，以及为何使用 QoS 策略。

本主题中的两个情况是：

1. 优先网络通信的业务线应用程序
2. 优先网络通信应用程序 HTTP 服务器

>[!NOTE]
>某些部分的本主题包含常规步骤可能需要执行的说明的操作。 有关详细说明了管理 QoS 策略，请参阅[管理 QoS 策略](qos-policy-manage.md)。

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>方案 1：优先网络通信的业务线应用程序

在此情况下，IT 部门具有多个目标，也可以实现使用 QoS 策略：

- 提供更好的网络性能 mission\ 关键应用程序。
- 他们在使用特定的应用程序时，可提供更好的一组键的用户的网络性能。
- 确保 company\ 范围数据备份应用程序不会妨碍一次使用过多的带宽的网络性能。

IT 部门决定配置 QoS 策略以进行分类网络通信，并配置提供较高优先级交通优先处理其路由器使用与众不同服务代码点 \(DSCP\) 值优先特定应用程序。 

>[!NOTE]
>DSCP 的详细信息，请参阅的部分**定义 QoS 优先级通过区分服务代码点**主题中[质量服务 (QoS) 策略](qos-policy-top.md)。

除了 DSCP 值 QoS 策略可以指定调节率。 限制的效果的限制匹配 QoS 的特定策略发送率所有站的交通。

### <a name="qos-policy-configuration"></a>QoS 策略配置

用三个单独目标可以实现，IT 管理员决定创建三种不同 QoS 策略。

#### <a name="qos-policy-for-lob-app-servers"></a>LOB 应用服务器 QoS 策略

为其 IT 部门创建 QoS 策略的第一个 mission\ 关键应用是规划 \(ERP\) 应用程序的范围内 company\ 企业资源。 在正在运行 Windows Server 2016 的几个计算机上托管 ERP 应用程序。 Active Directory 域服务，在这些计算机属于组织单位 \(OU\) 创建业务线 \(LOB\) 应用程序的服务器。 在正在运行 Windows 10 和 Windows 8.1 的计算机上安装 ERP 应用程序 client\ 侧组件。

在组策略 IT 管理员选择将在其应用 QoS 策略组策略对象 \(GPO\)。 通过使用 QoS 策略向导，IT 管理员创建 QoS 策略称为"服务器 LOB 策略"指定 high\ 优先级 DSCP 值的 44 所有应用、任何 IP 地址、TCP 并 UDP 和端口号。

QoS 策略应用到 LOB 服务器仅通过链接到包含仅这些服务器，通过组策略管理控制台 \(GPMC\) 工具 OU GPO。 每当计算机发送网络通信，此初始服务器 LOB 策略适用 high\ 优先级 DSCP 值。 稍后可以编辑该 QoS 策略 \（在组策略对象编辑器 tool\) 包含 ERP 应用程序端口号，限制将仅在使用指定的端口号时的策略。

#### <a name="qos-policy-for-the-finance-group"></a>财经组策略 QoS

而在公司内的多个组访问 ERP 应用程序，财经组取决于该应用程序时处理客户，以及组需要从应用一致的高性能。

若要确保财经组可支持其客户，QoS 策略必须为高的优先级分类这些用户的通信。 但是，该策略应应用时的财经组成员使用 ERP 应用以外的应用程序。 

出于此原因，IT 部门定义第二个 QoS 策略称为"客户端 LOB 策略"财经用户组运行 ERP 应用程序时，将应用 DSCP 值为 60 组策略对象编辑器工具中。

#### <a name="qos-policy-for-a-backup-app"></a>备份应用 QoS 策略

单独备份的应用程序在所有计算机上运行。 若要确保备份的应用程序交通不使用的所有可用网络资源，IT 部门创建了一个备份数据策略。 此备份策略指定 DSCP 值为 1 基于备份应用，这是可执行文件名称**backup.exe**。 

第三 GPO 创建和部署的所有客户端计算机在域中。 备份的应用程序发送数据，只要低优先级 DSCP 值应用，即使源自财经商业部中的计算机。
  
>[!NOTE]
>没有 QoS 策略网络交通发送 DSCP 值为 0。

### <a name="scenario-policies"></a>方案策略

下表总结此项 scenario QoS 的策略。
  
|策略名称|DSCP 值|调节率|应用到组织设备|描述|  
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[没有策略]|0|无|[没有部署]|未分类通信的最佳努力（默认）处理。|  
|备份数据|1|无|所有客户端|适用于此批量数据的优先级低 DSCP 值。|  
|服务器 LOB|44|无|计算机 OU ERP 服务器|将应用 ERP 服务器交通高优先级 DSCP|  
|客户端 LOB|60|无|财经用户组|将应用 ERP 客户端交通高优先级 DSCP|  

>[!NOTE]
>在小数点形式表示 DSCP 值。

使用 QoS 策略定义和应用通过使用组策略，站网络通信接收的策略指定 DSCP 值。 路由器然后提供差异处理使用队列基于这些 DSCP 值。 用四个队列路由器配置为此 IT 部门，：高优先级中间优先级，最大努力，低优先级。

当交通到达 DSCP 中的值路由器"服务器 LOB 策略"和"客户端 LOB 策略，"数据将被放入高优先级队列。 路况 DSCP 值为 0 接收服务最大努力的级别。 （从备份的应用程序）的 1 的值 DSCP 数据包得到低优先级处理。  
  
### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>先决条件排列优先顺序业务线应用程序

若要完成此任务中，请确保你满足以下要求：

- 所涉及的计算机运行 QoS\ 兼容操作系统。

- 所涉及的计算机是 Active Directory 域服务 \(AD DS\) 域中的成员，以便可以使用组策略配置。

- TCP/IP 网络配置为 DSCP \(RFC 2474\) 路由器设置。 有关详细信息，请参阅[RFC 2474](http://www.ietf.org/rfc/rfc2474.txt)。

- 满足管理凭据要求。

#### <a name="administrative-credentials"></a>管理凭据

若要完成此任务中，你必须可以创建并将其部署组策略对象。
  
#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>设置排列优先顺序业务线应用程序的测试环境

若要设置测试环境，完成以下任务。

- 客户端和分为组织设备的用户创建广告 DS 域。 有关部署广告 DS 的说明，请参阅[Core 网络指南](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide)。

- 配置 differentially 队列基于 DSCP 值到路由器。 例如，DSCP 值 44 进入"白金卡"队列和所有其他是加权公平排队。

>[!NOTE]
> 你可以通过使用网络监视器之类的工具的捕获网络查看 DSCP 值。 执行网络捕获后，你可以观察 TOS 字段中捕获的数据。

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>步骤设置优先级业务线应用程序

优先业务线应用程序，完成以下任务：

1. 创建并链接 QoS 策略使用组策略对象 \(GPO\)。

2. 配置路由器 differentially 将业务线（通过使用队列）的应用程序根据所选 DSCP 值。 此任务中的步骤会有所不同取决于你的路由器的类型。

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>方案 2：优先网络通信应用程序 HTTP 服务器

在 Windows Server 2016，基于策略的 QoS 包括 URL 基于策略的功能。 URL 策略允许您管理 HTTP 服务器的带宽。

对于开发和托管 Internet 信息服务 \(IIS\) web 服务器，在很多企业应用程序和 Web 应用可以访问来自客户端计算机上的浏览器。

在此情况下，假设，您管理的一组 IIS 服务器该主机培训视频适用于所有你的组织的员工。 你的目标是确保来自这些视频服务器交通无法严重影响你的网络，并确保与网络上的语音和数据流量区别在于视频通信。 

此任务是类似于方案 1 中的任务。 在将设计和配置流量管理设置的 DSCP 值的视频通信，例如和像你一样业务线应用程序限制评价相同。 但当指定通信，而不是提供的应用名称，仅输入 HTTP 服务器应用程序将响应的 URL：例如，https://hrweb/training。
  
> [!NOTE]
>你无法使用 URL 基于 QoS 策略优先考虑网络的计算机运行的 Windows 7 和 Windows Server 2008 R2 之前所发布的 Windows 操作系统的交通。

### <a name="precedence-rules-for-url-based-policies"></a>URL 基于策略的优先顺序规则

所有以下 Url 有效，并且可以指定 QoS 策略中并应用于一台计算机或的用户的同时：

- http://video

- https://training.hr.mycompany.com

- http://10.1.10.249:8080/tech  

- https://*/ebooks

但哪个会收到优先？ 规则很简单。 从左到右阅读顺序优先 URL 基于策略。 因此，从优先级最低最高的优先级，URL 字段是：
  
[1.URL 方案](#bkmk_QoS_UrlScheme)

[2.URL 主机](#bkmk_QoS_UrlHost)

[3.URL 端口](#bkmk_QoS_UrlPort)

[4.URL 路径](#bkmk_QoS_UrlPath)

详细信息，如下所示：

####  <a name="bkmk_QoS_UrlScheme"></a>1.URL 方案

 `https://` 有超过较高优先级`http://`。

####  <a name="bkmk_QoS_UrlHost"></a>2.URL 主机

 从至最低高的优先级，其中包括：

1. 主机

2. IPv6 地址

3. IPv4 地址

4. 通配符

在主机名的情况下使用更虚线元素（更多深度）主机具有优先级高于使用较少虚线元素主机。 例如，之间的以下主机：

- video.internal.training.hr.mycompany.com (深度 = 6)
  
- selfguide.training.mycompany.com (深度 = 4)
  
- 训练 (深度 = 1)
  
- 库 (深度 = 1)
  
 **video.internal.training.hr.mycompany.com**具有最高的优先级和**selfguide.training.mycompany.com**具有下一步高的优先级。 **训练**和**库**共享相同的最低优先级。  
  
####  <a name="bkmk_QoS_UrlPort"></a>3.URL 端口

特定或隐式端口号具有较高优先级比通配符端口。

####  <a name="bkmk_QoS_UrlPath"></a>4.URL 路径

主机名称，例如多个元素可能包括 URL 路径。 更多元素带有始终有比较小的一个较高优先级。 例如，按优先级列出以下路径：  

1.  /ebooks/tech/windows/networking/qos

2.  / ebooks/技术/windows 月

3.  /ebooks

4.  /

如果用户选择包含所有子目录和按照 URL 路径文件，则此 URL 路径将有较低比不得不如果选择不所做的优先级。

用户也可以选择 URL 基于策略中指定目的地的 IP 地址。 目标 IP 地址优先级低于任何前面所述的四个 URL 字段。
  
### <a name="quintuple-policy"></a>Quintuple 策略

协议 ID、源 IP 地址、源端口、目的地的 IP 地址和目的地端口由指定 Quintuple 策略。 Quintuple 策略始终优先级高于任何基于 URL 的策略。 

如果 Quintuple 策略已经应用的用户，新的 URL 基于策略不会导致冲突该用户的客户端的任何计算机。

本指南中的下一步主题，请参阅[管理 QoS 策略](qos-policy-manage.md)。

本指南中的第一个主题，请参阅[质量服务 (QoS) 策略](qos-policy-top.md)。

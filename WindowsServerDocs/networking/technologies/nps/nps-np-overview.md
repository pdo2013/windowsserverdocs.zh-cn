---
title: 网络策略
description: 本主题提供网络策略概述了网络策略服务器，在 Windows Server 2016，并包含指向有关 NPS 其他指南。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7e1604f4839bd955e5ea10d9eafea5ef0c978977
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="network-policies"></a>网络策略

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题进行 NPS 中的网络策略的概述。

>[!NOTE]
>本主题中，除了以下网络策略文档才可用。
> - [访问权限](nps-np-access.md)
> - [配置网络策略](nps-np-configure.md)

网络策略是一组的条件约束，，允许你将有权连接到该网络，情况下，他们可以或无法连接的设置。

在处理作为远程身份验证拨入用户服务 (RADIUS) 服务器时连接请求，NPS 连接请求为执行身份验证和授权。 身份验证过程 NPS 验证计算机连接到该网络的用户的身份。 授权过程 NPS 确定用户或计算机是否可以访问该网络。

若要使这些决定，NPS 使用配置 NPS 主机中的网络策略。 NPS 还会检查 Active Directory 中的用户帐户拨号中属性&reg;域服务 \(AD DS\) 执行授权。

## <a name="network-policies---an-ordered-set-of-rules"></a>网络策略的一组有序规则

网络策略视为规则。 每个规则具有一组条件和设置。 NPS 比较连接请求属性规则的条件。 如果之间规则，连接要求发生匹配项，规则中定义的设置将应用的连接。

当多个网络策略配置 NPS 中时，它们是一组规则有序。 NPS 检查直到找到匹配项的列表，然后秒钟，并等等的第一个规则针对每个连接请求。

每个网络策略有**策略状态**设置允许你启用或禁用的策略。 禁用网络策略，当 NPS 不评估在该策略时授权连接请求。

>[!NOTE]
>如果你想 NPS 评估网络策略执行授权连接请求时，你必须配置**策略状态**设置，方法是依次选择该策略启用复选框。

## <a name="network-policy-properties"></a>网络策略属性

有属性每个网络策略的四个类别：

### <a name="overview"></a>概述

 这些属性，使您指定策略是否已启用、策略授予还是拒绝访问，以及的特定网络连接的方法或网络的访问权限服务器 (NAS) 的类型是才能进行连接请求。 概述属性还允许您可以指定是否忽略广告 DS 中的用户帐户拨号中属性。 如果您选择此选项时，仅在网络策略设置用于 nps 确定是否连接已授权。


### <a name="conditions"></a>条件

 这些属性，允许你指定的条件，以便与网络策略; 匹配必须连接请求如果在策略配置的条件匹配连接请求，NPS 适用命名在连接到网络策略设置。 例如，如果你的网络策略的条件指定 NAS IPv4 地址，NPS 具有指定的 IP 地址 NAS 从接收连接请求的策略中条件匹配连接请求。 


### <a name="constraints"></a>约束

 约束是匹配连接请求所需的网络策略的其他参数。 如果在连接请求不匹配约束，NPS 自动拒绝该请求。 与不同 NPS 响应无与伦比条件中的网络策略，如果约束不匹配，NPS 拒绝连接请求，不评估在其他网络策略的情况下。

### <a name="settings"></a>设置

 这些属性，使您指定 NPS 适用于连接请求中，如果所有网络策略条件策略都匹配的设置。

当你添加新的网络策略使用 NPS 主机时，你必须使用新的网络策略向导。 你创建网络策略使用向导后，你可以通过双击 NPS 主机，以获取策略属性策略自策略。

有关示例模式匹配语法指定网络策略的属性，请参阅[NPS 中使用常规表情](nps-crp-reg-expressions.md)。

有关 NPS 的详细信息，请参阅[网络策略 Server (NPS)](nps-top.md)。

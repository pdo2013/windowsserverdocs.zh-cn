---
title: 网络策略
description: 本主题提供用于在 Windows Server 2016 中，网络策略服务器的网络策略的概述，并包括有关 NPS 的附加指导的链接。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 60ab80bb6cf26578430b76806405a65a0f596ef2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848948"
---
# <a name="network-policies"></a>网络策略

>适用于：Windows 服务器 （半年频道），Windows Server 2016

有关在 NPS 网络策略的概述，可以使用本主题。

>[!NOTE]
>除了本主题提供了以下网络策略文档。
> - [访问权限](nps-np-access.md)
> - [配置网络策略](nps-np-configure.md)

网络策略是一组条件、 约束和设置，允许您指定谁有权连接到网络以及其下可以或无法连接的环境。

在处理连接请求与远程身份验证拨入用户服务 (RADIUS) 服务器时，NPS 连接请求执行身份验证和授权。 在身份验证过程中，NPS 验证的用户或连接到网络的计算机的标识。 在授权过程中，NPS 确定是否允许用户或计算机是访问网络。

若要确定这些数据，NPS 使用在 NPS 控制台中配置的网络策略。 NPS 还检查 Active Directory 中的用户帐户的拨入属性&reg;域服务\(AD DS\)执行授权。

## <a name="network-policies---an-ordered-set-of-rules"></a>网络策略-一组有序规则

可以将网络策略视为规则。 每个规则都具有一组条件和设置。 NPS 将规则应用于的连接请求的属性的条件进行比较。 如果规则和连接请求之间出现匹配项，该规则中定义的设置被应用于连接。

当在 NPS 中配置多个网络策略时，它们是一组有序的规则。 NPS 检查列表中，则第二个，依次类推中的第一个规则针对每个连接请求，直到找到匹配项。

每个网络策略都有**策略状态**设置，让你能够启用或禁用的策略。 如果禁用了网络策略，NPS 不评估策略，授权连接请求时。

>[!NOTE]
>如果您希望 NPS 评估网络策略对连接请求执行授权时，必须配置**策略状态**通过选择该策略设置启用复选框。

## <a name="network-policy-properties"></a>网络策略属性

有四个类别的每个网络策略属性：

### <a name="overview"></a>概述

 这些属性可以指定是否启用了策略、 策略是否授予或拒绝访问，以及特定网络连接方法或类型的网络访问服务器 (NAS) 是否需要对连接请求。 概述属性还允许您指定是否忽略 AD DS 中的用户帐户的拨入属性。 如果选择此选项，仅在网络策略中的设置用于由 NPS 确定是否授权连接。


### <a name="conditions"></a>条件

 这些属性，可以指定的条件的连接请求必须拥有才能匹配网络策略中;如果在策略中配置的条件匹配连接请求，NPS 会应用到连接的网络策略中指定的设置。 例如，如果您的网络策略条件指定 NAS IPv4 地址，并且 NPS 从具有指定的 IP 地址的 NAS 接收连接请求，策略中的条件将匹配连接请求。 


### <a name="constraints"></a>约束

 约束是匹配连接请求所需的网络策略的其他参数。 如果约束不匹配的连接请求，则 NPS 自动拒绝该请求。 与不同的网络策略中的不匹配条件的 NPS 响应，如果约束不匹配，NPS 会拒绝连接请求而无需对其他网络策略进行评估。

### <a name="settings"></a>设置

 这些属性，指定策略的网络策略条件的所有匹配，NPS 将适用于连接的请求的设置。

当使用 NPS 控制台添加新的网络策略时，必须使用新建网络策略向导。 使用向导创建网络策略后，您可以通过双击 NPS 控制台，以获取策略属性中的策略自定义策略。

有关示例的模式匹配语法来指定网络策略属性，请参阅[在 NPS 中使用正则表达式](nps-crp-reg-expressions.md)。

有关 NPS 的详细信息，请参阅[网络策略服务器 (NPS)](nps-top.md)。

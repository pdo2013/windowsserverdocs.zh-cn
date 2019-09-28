---
title: 配置 NPS UDP 端口信息
description: 你可以使用本主题来配置网络策略服务器（NPS）用于 Windows Server 2016 中的远程身份验证拨入用户服务（RADIUS）身份验证和记帐流量的端口。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1ba6c059639b9ae7e77a9e103e7ed84f6a2032df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405306"
---
# <a name="configure-nps-udp-port-information"></a>配置 NPS UDP 端口信息

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用以下过程来配置网络策略服务器（NPS）用于远程身份验证拨入用户服务 \(RADIUS @ no__t 身份验证和记帐通信的端口。

默认情况下，NPS 在端口1812、1813、1645和1646上侦听用于所有已安装网络适配器的 Internet 协议版本 6 \(IPv6 @ no__t 和 IPv4 的 RADIUS 流量。

>[!NOTE]
>如果在网络适配器上卸载 IPv4 或 IPv6，则 NPS 不会监视已卸载协议的 RADIUS 流量。

用于身份验证的1812端口值和用于记帐的1813是由 Internet 工程任务组定义的 RADIUS 标准端口，在 Rfc 2865 和2866中 \(IETF @ no__t-1。 但是，默认情况下，许多访问服务器使用端口1645进行身份验证请求，使用1646进行记帐请求。 无论决定使用哪种端口号，请确保将 NPS 和访问服务器配置为使用相同的端口号。

>无关紧要如果不使用 RADIUS 默认端口号，则必须在防火墙上为本地计算机配置例外，以允许新端口上的 RADIUS 流量。 有关详细信息，请参阅为[RADIUS 流量配置防火墙](nps-firewalls-configure.md)。

必须至少具有 **Domain Admins** 中的成员身份或同等身份才能完成此过程。

## <a name="to-configure-nps-udp-port-information"></a>配置 NPS UDP 端口信息 

1. 打开 NPS 控制台。
2. 右键单击 "**网络策略服务器**"，然后单击 "**属性**"。
3. 单击 "**端口**" 选项卡，然后检查端口的设置。 如果 RADIUS 身份验证和 RADIUS 记帐 UDP 端口不同于提供的默认值（1812和1645（用于身份验证，以及用于记帐的1813和1646），请在 "**身份验证**和**记帐**" 中键入端口设置。
4. 若要将多个端口设置用于身份验证或记帐请求，请用逗号分隔端口号。

有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略服务器（NPS）](nps-top.md)。

---
title: 无线访问部署过程
description: 本主题是 Windows Server 2016 网络指南 "部署基于密码的 802.1 X 身份验证无线访问" 的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 10c69e1aa6157c7f088f190c0283b33b630bc25f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406260"
---
# <a name="wireless-access-deployment-process"></a>无线访问部署过程

>适用于：Windows Server（半年频道）、Windows Server 2016

用于部署无线访问的过程发生在以下阶段：

## <a name="stage-1--ap-deployment"></a>阶段 1-AP 部署

规划、部署和配置你的接入点以实现无线客户端连接，并用于 NPS。 根据你的首选项和网络依赖关系，你可以先在无线 Ap 上预先 no__t-0configure 设置，然后再将其安装到网络上，也可以在安装后远程配置这些设置。

## <a name="stage-2--adds-group-configuration"></a>阶段2– AD DS 组配置

在 AD DS 中，必须创建一个或多个无线用户安全组。

接下来，确定允许无线访问网络的用户。

最后，将用户添加到你创建的相应无线用户安全组。

>[!NOTE]
>默认情况下，用户帐户拨入属性中的**网络访问权限**设置配置为 "**通过 NPS 网络策略控制访问**" 设置。 除非有特定原因要更改此设置，否则建议保留默认值。 这允许你通过在 NPS 中配置的网络策略来控制网络访问。

## <a name="stage-3--group-policy-configuration"></a>阶段 3-组策略配置

使用组策略管理编辑器 Microsoft 管理控制台 \(MMC @ no__t 配置组策略的无线网络 \(IEEE 802.11 @ no__t 策略扩展。

若要使用无线网络策略中的设置配置域 @ no__t-0member 计算机，必须应用组策略。 首次将计算机加入域时，会自动应用组策略。 如果对组策略进行了更改，则将自动应用新设置：

- 按 no__t-0determined 间隔组策略

- 如果域用户注销然后重新登录到网络

- 通过重新启动客户端计算机并登录到域

你还可以通过在命令提示符下运行命令**gpupdate**来强制在登录到计算机时刷新组策略。

## <a name="stage-4--nps-configuration"></a>阶段4– NPS 配置

使用 NPS 中的配置向导将无线访问点添加为 RADIUS 客户端，并创建 NPS 在处理连接请求时使用的网络策略。

使用向导创建网络策略时，请将 PEAP 指定为 EAP 类型，并将在第二个阶段创建的无线用户安全组指定为。

## <a name="stage-5--deploy-wireless-clients"></a>阶段5–部署无线客户端

使用客户端计算机连接到网络。

对于可以登录到有线 LAN 的域成员计算机，刷新组策略时，会自动应用必要的无线配置设置。

如果已启用无线网络中的设置 \(IEEE 802.11 @ no__t 策略在计算机处于无线网络的广播范围内时自动连接，则无线域 @ no__t 2joined 计算机将自动尝试连接到无线 LAN。

若要连接到无线网络，用户只需在 Windows 提示时提供其域用户名和密码凭据。

若要规划无线访问部署，请参阅[无线访问部署规划](d-wireless-access-planning.md)。

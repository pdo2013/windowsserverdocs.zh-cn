---
title: 无线接入部署过程
description: 本主题介绍 Windows Server 2016 网络指南"部署密码基于 802.1 X 身份验证无线的访问"部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: shortpatti
ms.author: pashort
ms.openlocfilehash: fcdbe796e4d530604dfec448e817eab877d34c4f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment-process"></a>无线接入部署过程

>适用于：Windows Server（半年通道），Windows Server 2016

使用部署无线接入过程这些阶段：

## <a name="stage-1--ap-deployment"></a>步骤 1-接入点部署

计划、 部署和 NPS 用来配置你的无线客户端连接和使用的接入点。 根据你的偏好随意选择和网络依赖关系，你可以任一 pre\-配置你之前安装它们在你的网络上的无线接入点上的设置，或者你可以在安装后远程配置它们。

## <a name="stage-2--ad-ds-group-configuration"></a>步骤 2-广告 DS 组配置

在广告 DS 必须创建一个或多个无线用户安全组。

接下来，识别允许无线网络的访问权限的用户。

最后，添加到您创建的相应无线用户安全组的用户。

>[!NOTE]
>默认情况下，**网络访问权限**设置在用户帐户拨号属性配置的设置**控制 NPS 网络策略访问**。 除非你具有特定的原因，若要更改此设置，建议你继续默认值。 这允许你控制在 NPS 中配置网络策略通过网络访问权限。

## <a name="stage-3--group-policy-configuration"></a>步骤 3-组策略配置

配置无线网络 \ (IEEE 802.11\) 组策略使用组策略编辑器中管理 Microsoft 管理控制台 \(MMC\) 策略扩展。

若要配置 domain\ 成员计算机无线网络策略中使用的设置，你必须应用组策略。 一台计算机首次连接到域时, 组策略会自动应用。 如果对组策略进行了更改，将自动应用的新的设置：

- 由组策略的时间间隔 pre\ 确定

- 如果域用户注销，然后再到网络

- 重启客户端计算机并登录到域

你还可以强制组策略刷新运行命令将其登录到计算机时**gpupdate**在命令提示符。

## <a name="stage-4--nps-server-configuration"></a>Stage 4 – NPS server 配置

添加作为 RADIUS 客户端的无线接入点和创建 NPS 在处理连接请求时所使用的网络策略，在 NPS 使用配置向导。

当使用该向导创建网络策略，作为 EAP 类型，并在第二个阶段创建无线用户安全组中指定 PEAP。

## <a name="stage-5--deploy-wireless-clients"></a>Stage 5-无线部署的客户端

使用客户端计算机连接到该网络。

对于域成员可以登录到 lan 之外，有线的计算机，刷新组策略时自动应用必要的无线配置设置。

如果您启用了设置无线网络中的 \ (IEEE 802.11\) 策略自动连接当计算机处于的无线网络，然后你的无线、 domain\ 加入计算机将广播范围内会自动尝试连接到无线 LAN。

若要连接到无线网络，用户需要仅提供其域用户名和密码凭据时提示你通过 Windows。

若要计划你的无线接入部署，请参阅[无线访问部署规划](d-wireless-access-planning.md)。

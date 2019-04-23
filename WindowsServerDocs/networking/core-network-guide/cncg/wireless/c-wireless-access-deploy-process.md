---
title: 无线访问部署过程
description: 本主题是 Windows Server 2016 网络指南"部署基于密码的 802.1 X 身份验证无线访问"的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 6a286cf10e066043ee6f514bbf468bfb2b13f162
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879838"
---
# <a name="wireless-access-deployment-process"></a>无线访问部署过程

>适用于：Windows 服务器 （半年频道），Windows Server 2016

用于将无线访问部署过程是在这些阶段：

## <a name="stage-1--ap-deployment"></a>阶段 1-亚太部署

规划、 部署和使用 NPS 中配置您的 Ap 的无线客户端连接和使用。 可以根据首选项和网络依赖项，请预先\-之前安装在网络上无线 Ap 上配置设置，也可以配置其远程安装完成后。

## <a name="stage-2--adds-group-configuration"></a>阶段 2 – AD DS 组配置

在 AD DS 中，必须创建一个或多个无线用户安全组。

接下来，确定允许对网络的无线访问的用户。

最后，将用户添加到你创建的相应的无线用户安全组。

>[!NOTE]
>默认情况下**网络访问权限**用的设置配置用户帐户拨入属性中的设置**通过 NPS 网络策略控制访问**。 除非有特定原因需要更改此设置，因此，建议保留默认值。 这样，您可以控制在 NPS 中配置的网络策略的网络访问权限。

## <a name="stage-3--group-policy-configuration"></a>阶段 3-组策略配置

配置无线网络\(IEEE 802.11\)策略扩展的组策略通过使用组策略管理编辑器 Microsoft 管理控制台\(MMC\)。

若要配置域\-成员计算机的无线网络策略中使用的设置，你必须应用组策略。 当计算机首次加入到域中时，将自动应用组策略。 如果对组策略进行了更改，会自动应用新设置：

- 通过组策略在预先\-确定的时间间隔

- 如果域用户注销，然后返回到网络

- 通过重新启动客户端计算机并登录到域

您也可以强制组策略刷新运行命令登录到计算机时**gpupdate**在命令提示符处。

## <a name="stage-4--nps-configuration"></a>阶段 4-NPS 配置

使用配置向导在 NPS 中，将无线访问点添加为 RADIUS 客户端，并创建处理连接请求时，将使用 NPS 网络策略。

当使用向导创建网络策略，指定 PEAP 作为 EAP 类型，并在第二个阶段中创建的无线用户安全组。

## <a name="stage-5--deploy-wireless-clients"></a>阶段 5-部署无线客户端

使用客户端计算机连接到网络。

对于可以登录到有线 LAN 的域成员计算机，刷新组策略时自动应用必要的无线配置设置。

如果已启用了无线网络中的设置\(IEEE 802.11\)策略来自动连接计算机位于内广播的无线网络、 无线、 域范围\-然后将加入的计算机自动尝试连接到无线 LAN。

若要连接到无线网络，用户只需要提供其域用户名和密码凭据通过 Windows 出现提示时。

若要规划您的无线访问部署，请参阅[无线访问部署规划](d-wireless-access-planning.md)。

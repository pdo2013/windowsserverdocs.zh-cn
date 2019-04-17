---
title: 配置远程 RADIUS 服务器组
description: 本主题介绍如何在 Windows Server 2016 中的网络策略 Server 配置远程 RADIUS 服务器组。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f293fd18176115365e5e243a90a034676b3262f9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="configure-remote-radius-server-groups"></a>配置远程 RADIUS 服务器组

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题配置远程 RADIUS 服务器组，当你想要配置 NPS 用作代理服务器和前进连接到其他 NPS 服务器进行处理的请求。

## <a name="add-a-remote-radius-server-group"></a>添加遥控器 RADIUS 服务器组

你可以使用此过程网络策略 Server (NPS) 管理单元中添加新远程 RADIUS 服务器组。

NPS RADIUS 代理作为配置时，你创建新的连接要求策略 NPS 用来确定要将转发给其他 RADIUS 服务器哪个连接请求。 此外，连接要求策略配置通过指定远程 RADIUS 服务器组包含一个或多个 RADIUS 服务器，以告诉 NPS 发送匹配连接请求策略连接请求的位置。

>[!NOTE]
>你还可以过程中创建新的连接要求策略配置新远程 RADIUS 服务器组。

在会员**域管理员**，或等效的最低要求完成此过程。

### <a name="to-add-a-remote-radius-server-group"></a>若要添加远程 RADIUS 服务器组 

1. 在服务器管理器中，单击**工具**，然后单击**网络策略服务器**打开 NPS 主机。
2. 控制台树中，双击**RADIUS 客户端和服务器**，右键单击**远程 RADIUS 服务器组**，然后单击**新建**。
3. **新远程 RADIUS 服务器组**对话框中打开。 在**组的名称**，键入远程 RADIUS 服务器组的名称。
4. **在 RADIUS 服务器**，单击**添加**。 **添加 RADIUS 服务器**对话框中打开。 键入你想要添加到组中，或者 RADIUS 服务器、完全限定域名 \(FQDN\) 的键入，然后单击 RADIUS 服务器的 IP 地址**验证**。
5. 在**添加 RADIUS 服务器**，单击**身份验证/记帐**选项卡。在**共享机密**和**确认共享的机密**，键入共享的机密。 当你将在本地计算机配置为远程 RADIUS 服务器上的 RADIUS 客户端，必须使用相同的共享的机密。
6. 如果你未进行身份验证使用扩展验证协议 (EAP)，请单击**请求必须包含消息 authenticator 特性**。 EAP 使用默认邮件 Authenticator 属性。
7. 验证身份验证和帐户端口号部署无误。
8. 如果你使用用于财务，在不同的共享的机密**记帐**，清除**使用相同的共享的机密身份验证和帐户**复选框，，然后键入中的记帐共享的机密**共享机密**和**确认共享的机密**。
9. 如果你不希望网络的访问权限服务器启动和清晰停止远程 RADIUS 服务器、到邮件转发**网络的访问权限 server 启动和停止对此服务器通知转发**复选框。

有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略 Server (NPS)](nps-top.md)。


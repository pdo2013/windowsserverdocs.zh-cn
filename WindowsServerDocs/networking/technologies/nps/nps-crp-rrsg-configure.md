---
title: 配置远程 RADIUS 服务器组
description: 本主题提供有关如何在 Windows Server 2016 中的网络策略服务器中配置远程 RADIUS 服务器组的信息。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 02088a35196c0bfadeb65e8971a47fdcc741258d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818758"
---
# <a name="configure-remote-radius-server-groups"></a>配置远程 RADIUS 服务器组

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题可用于配置远程 RADIUS 服务器组，当你想要将 NPS 配置为充当代理服务器和连接请求转发到其他 NPSs 进行处理。

## <a name="add-a-remote-radius-server-group"></a>添加远程 RADIUS 服务器组

此过程可用于在网络策略服务器 (NPS) 管理单元中添加新的远程 RADIUS 服务器组。

将 NPS 配置为 RADIUS 代理时，创建新的连接请求策略，NPS 用于确定哪些连接请求转发给其他 RADIUS 服务器。 此外，通过指定的远程 RADIUS 服务器组包含一个或多个 RADIUS 服务器，以告诉 NPS 将匹配连接请求策略的连接请求发送到何处的连接请求策略进行配置。

>[!NOTE]
>此外可以在创建新的连接请求策略的过程中配置新的远程 RADIUS 服务器组。

必须至少具有 **Domain Admins** 中的成员身份或同等身份才能完成此过程。

### <a name="to-add-a-remote-radius-server-group"></a>若要添加远程 RADIUS 服务器组 

1. 在服务器管理器中，单击**工具**，然后单击**网络策略服务器**打开 NPS 控制台。
2. 在控制台树中，双击**RADIUS 客户端和服务器**，右键单击**远程 RADIUS 服务器组**，然后单击**新建**。
3. **新的远程 RADIUS 服务器组**对话框随即打开。 在中**组名称**，键入远程 RADIUS 服务器组的名称。
4. **在 RADIUS 服务器**，单击**添加**。 **添加 RADIUS 服务器**对话框随即打开。 键入你想要添加到组，RADIUS 服务器的 IP 地址或键入完全限定的域名称\(FQDN\)的 RADIUS 服务器，然后单击**验证**。
5. 在中**添加 RADIUS 服务器**，单击**身份验证/记帐**选项卡。在中**共享机密**并**确认共享的机密**，键入共享的机密。 在本地计算机配置远程 RADIUS 服务器上的 RADIUS 客户端时，必须使用相同的共享的机密。
6. 如果你不使用可扩展身份验证协议 (EAP) 进行身份验证，请单击**请求必须包含消息身份验证器属性**。 默认情况下，EAP 使用消息身份验证器属性。
7. 验证身份验证和记帐端口号适合你的部署。
8. 如果在中，为记帐，使用不同的共享的机密**记帐**，清除**相同的共享的机密用于身份验证和记帐**复选框，，然后键入中的记帐共享的机密**共享的机密**并**确认共享的机密**。
9. 如果不希望转发网络访问服务器启动和停止消息到远程 RADIUS 服务器上，清除**网络访问服务器启动和停止通知发送到此服务器转发**复选框。

有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略服务器 (NPS)](nps-top.md)。


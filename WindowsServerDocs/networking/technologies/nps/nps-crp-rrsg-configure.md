---
title: 配置远程 RADIUS 服务器组
description: 本主题提供有关如何在 Windows Server 2016 中的网络策略服务器中配置远程 RADIUS 服务器组的信息。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fe34d25d2b54b02bb56fcad99c433054a309f60b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405456"
---
# <a name="configure-remote-radius-server-groups"></a>配置远程 RADIUS 服务器组

>适用于：Windows Server（半年频道）、Windows Server 2016

如果希望将 NPS 配置为充当代理服务器，并将连接请求转发到其他 NPSs 进行处理，则可以使用本主题来配置远程 RADIUS 服务器组。

## <a name="add-a-remote-radius-server-group"></a>添加远程 RADIUS 服务器组

您可以使用此过程在网络策略服务器（NPS）管理单元中添加新的远程 RADIUS 服务器组。

将 NPS 配置为 RADIUS 代理时，会创建一个新的连接请求策略，NPS 使用该策略来确定哪些连接请求转发到其他 RADIUS 服务器。 此外，通过指定包含一个或多个 RADIUS 服务器的远程 RADIUS 服务器组来配置连接请求策略，这将告知 NPS 向其中发送与连接请求策略匹配的连接请求的位置。

>[!NOTE]
>你还可以在创建新的连接请求策略过程中配置新的远程 RADIUS 服务器组。

必须至少具有 **Domain Admins** 中的成员身份或同等身份才能完成此过程。

### <a name="to-add-a-remote-radius-server-group"></a>添加远程 RADIUS 服务器组 

1. 在服务器管理器中，单击 "**工具**"，然后单击 "**网络策略服务器**" 以打开 NPS 控制台。
2. 在控制台树中，双击 " **RADIUS 客户端和服务器**"，右键单击 "**远程 RADIUS 服务器组**"，然后单击 "**新建**"。
3. 此时将打开 "**新建远程 RADIUS 服务器组**" 对话框。 在 "**组名**" 中，键入远程 RADIUS 服务器组的名称。
4. **在 RADIUS 服务器中**，单击 "**添加**"。 此时将打开 "**添加 RADIUS 服务器**" 对话框。 键入要添加到组中的 RADIUS 服务器的 IP 地址，或者键入完全限定的域名 \(FQDN @ no__t，然后单击 "**验证**"。
5. 在 "**添加 RADIUS 服务器**" 中，单击 "**身份验证/记帐**" 选项卡。在 "**共享机密**" 和 "**确认共享密钥**" 中，键入共享机密。 在远程 RADIUS 服务器上将本地计算机配置为 RADIUS 客户端时，必须使用相同的共享机密。
6. 如果不使用可扩展的身份验证协议（EAP）进行身份验证，请单击 "**请求必须包含消息身份验证器属性"** 。 默认情况下，EAP 使用消息身份验证器属性。
7. 验证你的部署的身份验证和记帐端口号是否正确。
8. 如果你使用不同的共享机密来进行记帐，请在 "**记帐**" 中清除 "**为身份验证和记帐使用相同的共享机密**" 复选框，然后在 "**共享密钥**" 和 "**确认共享" 中键入记帐共享机密机密**。
9. 如果你不想将网络访问服务器启动和停止消息转发到远程 RADIUS 服务器，则清除 "将**网络访问服务器启动和停止通知转发到此服务器**" 复选框。

有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略服务器（NPS）](nps-top.md)。


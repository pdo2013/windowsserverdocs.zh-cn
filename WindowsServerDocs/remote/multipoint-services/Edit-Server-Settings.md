---
title: 编辑服务器设置
description: 了解 MultiPoint 服务设置
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afb64b94-9055-4703-b8ce-a8839b2718da
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 8a6a9d8e6a76a8fb3c0da59c8fb487d0311f04d7
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871706"
---
# <a name="edit-server-settings"></a>编辑服务器设置
安装 MultiPoint 服务时，你可以配置系统设置，包括选择某些程序。 本主题介绍你可以为 MultiPoint 服务系统进行的设置，并解释了如何编辑设置。  
  
## <a name="about-multipoint-services-settings"></a>关于 MultiPoint 服务设置  
下表介绍了你可以为 MultiPoint 服务系统更改的不同设置。  
  
|MultiPoint 服务设置|描述|  
|-----------------------------------------------------------------------------------------|---------------|  
|允许一个帐户具有多个会话|允许一个用户帐户同时登录到多个工作站。 对于教室中每位学生使用一个共享帐户的情况，该设置十分有用。 使用此设置，对帐户资源（例如文档文件夹或桌面）的任何更改都会提供给使用同一帐户登录的所有用户。|  
|允许远程管理此计算机|允许网络上的其他 MultiPoint 系统管理运行 MultiPoint 服务的计算机。 如果选择此选项，并且管理的计算机位于同一子网，则此计算机将出现在要进行管理的可用服务器列表中。 如果选择此选项，并且管理的计算机位于不同子网，则管理的计算机仍可管理此计算机，但必须指定此计算机的 IP 地址。|
|允许监视此计算机的桌面|允许你控制能否在 MultiPoint 服务系统上监视桌面。 如果此设置为 "关" （未选中），则连接到运行 MultiPoint 服务的计算机的工作站（本地和远程）的桌面将不会显示在 MultiPoint 管理器的 "主页" 选项卡中（包括在计算机正在运行的其他计算机上。远程管理）。|  
|总是在控制台模式中启动|启用 RemoteFX 技术，该技术通过卸载 CPU 和 GPU 进程来提高远程桌面会话的速度和效率。 如果使用支持 RemoteFX 的客户端连接到 MultiPoint 服务，则可以使用此选项获得更好的性能。 这些优势取决于你的服务器和网络的能力。 例如，部分取决于压缩数据流时执行的附加处理进程所用的时间，是否少于传输更少数据所节省的时间。|  
|用户首次登录时不显示隐私通知|用户第一次登录 MultiPoint 工作站时，会显示告知用户其工作站活动可能受监视的通知。|  
|为每个工作站分配一个唯一的 IP|为每个工作站分配唯一 IP 地址。 默认情况下，MultiPoint 服务有一个 IP 地址，该系统上运行的所有会话均共享该地址。 但是，此配置可能导致某些应用程序兼容性问题。 例如，如果某一应用程序要求唯一的 IP 地址，它可能无法在 MultiPoint 服务上正常运行。 选择此选项（也称为 IP 虚拟化）可以解决此问题。<br /><br />IP 虚拟化对于监视 MultiPoint 服务上的活动会话也非常有用。 某些监视工具根据 IP 地址报告使用情况。 若要启用会话监视，你可以使用 IP 虚拟化来为每个会话分配唯一 IP 地址。 请注意，如果选择此选项，则每个新会话都会接收一个唯一 IP 地址。 任何现有会话继续使用共享的 IP 地址，直到这些会话注销并重新登录。|  
|在此计算机上允许 MultiPoint 仪表板和用户会话之间的即时消息|在此计算机上启用 MultiPoint 管理器和用户会话之间的聊天。 有关详细信息，请参阅[使用 IM](Use-IM.md)。|  
|允许管理员和 MultiPoint 仪表板用户会话之间的业务流程|如果启用，将允许管理员使用 MultiPoint 仪表板进行会话业务流程。 这些会话显示为缩略图。|  
|允许工作站使用 GPU 硬件呈现|控制工作站是否可以使用系统的图形处理单元 (GPU)。|   
  
## <a name="editing-the-computer-settings"></a>编辑计算机设置  
  
1.  在[工作站模式下](Switch-Between-Modes.md)打开 "MultiPoint 管理器"，然后单击 "**主页**" 选项卡。  
  
2.  在 "**计算机**" 列中，单击计算机名称，然后在 "*计算机名*"**任务**下，单击 "**编辑计算机设置**"。  
  
3.  选择或清除要更改的项，然后单击 **"确定"** 。  
  
## <a name="see-also"></a>请参阅  
[使用 MultiPoint 管理器管理系统任务](Manage-System-Tasks-Using-MultiPoint-Manager.md)  
  

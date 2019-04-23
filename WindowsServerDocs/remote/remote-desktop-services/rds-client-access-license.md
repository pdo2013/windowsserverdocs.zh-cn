---
title: 使用客户端访问许可证 (Cal) 许可 RDS 部署
description: 客户端中远程桌面服务的许可的概述。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5be6546b-df16-4475-bcba-aa75aabef3e3
author: lizap
ms.author: elizapo
ms.date: 09/20/2018
manager: dongill
ms.openlocfilehash: 6648a52bb4d09725935a2197d6ce6fa6d8cc74a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853438"
---
# <a name="license-your-rds-deployment-with-client-access-licenses-cals"></a>使用客户端访问许可证 (Cal) 许可 RDS 部署

>适用于：Windows 服务器 （半年频道），Windows Server 2016

每个用户和设备连接到远程桌面会话主机需要客户端访问许可证 (CAL)。 使用 RD 授权来安装、 颁发和跟踪 RDS Cal。  

当用户或设备连接到 RD 会话主机服务器时，RD 会话主机服务器将确定是否需要 RDS CAL。 然后，RD 会话主机服务器从远程桌面许可证服务器请求 RDS CAL。 如果有适合的 RDS CAL 许可证服务器不可用，RDS CAL 颁发给客户端，并且客户端能够连接到 RD 会话主机服务器，并从台式计算机或它们正在尝试使用的应用。

虽然授权宽限期内任何许可证服务器不需要之后的宽限期结束，客户端必须具有有效的 RDS CAL 颁发的许可证服务器之前, 他们可以登录到 RD 会话主机服务器。

使用以下信息来了解有关客户端访问授权远程桌面服务中的工作方式的信息和部署和管理你的许可证：

- [了解 Cal 模型](#understanding-the-cals-model)
- [激活许可证服务器](rds-activate-license-server.md)
- [许可证服务器上安装 RDS Cal](rds-install-cals.md)
- [跟踪部署中使用的 Cal](rds-track-cals.md)

## <a name="understanding-the-cals-model"></a>了解 Cal 模型

有两种类型的 Cal:

- Rds-每设备 Cal
- Rds-每用户 Cal

下表概述了两种类型的 Cal 之间的差异：

| 每个设备                                                     | 每个用户                                                                         |
|----------------------------------------------------------------|----------------------------------------------------------------------------------|
| 以物理方式分配给每个设备 Cal。                   | Cal 分配给 Active Directory 中的用户。                                 |
| 许可证服务器会跟踪 Cal。                        | 许可证服务器会跟踪 Cal。                                          |
| 不考虑 Active Directory 成员身份，可以跟踪 Cal。 | 在工作组中不能跟踪 Cal 的跟踪。                                       |
| 可以撤消最多 20%的 Cal。                              | 你无法撤销任何 Cal。                                                      |
| 临时 Cal 的有效期为 52 – 89 天。                       | 临时 Cal 将不可用。                                                |
| 不能过度分配 Cal。                                  | Cal （在远程桌面授权协议违反） 过度分配状态。 |

如果使用每个设备型号，临时许可证颁发第一次设备连接到 RD 会话主机。 第二次该设备连接时，只要激活许可证服务器，并且有可用的 Cal，许可证服务器颁发一个永久的 rds-每设备 CAL。

使用每个用户模型时，不强制执行授权，并向每个用户授予从任意数量的设备连接到 RD 会话主机的许可证。 许可证服务器颁发许可证从可用的 CAL 池或 Over-Used CAL 池。 它是由你负责确保你的所有用户具有有效的许可证和零 Over-Used Cal — 否则，您在与远程桌面服务许可条款的冲突。

若要确保遵守远程桌面服务许可条款，跟踪 rds-每用户 Cal 数在组织中使用并确保有足够每用户 Cal 安装在你的所有用户的许可证服务器上。

可以使用远程桌面授权管理器跟踪并生成有关 rds-每用户 Cal 的报告。

## <a name="note-about-cal-versions"></a>请注意有关 CAL 版本

由用户或设备 CAL 必须对应于用户或设备连接到的 Windows Server 的版本。 您不能使用较旧的 Cal 访问更高版本的 Windows Server 版本，但可以使用较新的 Cal 来访问 Windows Server 的早期版本。

下表显示了在 RD 会话主机和 RD 虚拟化主机兼容的 Cal。

|                  |2008 R2 和更早的 CAL|2012 CAL|2016 CAL|2019 CAL|
|---------------------------------|--------|--------|--------|--------|
| **2008、 2008 R2 许可证服务器**| 是    | 否     | 否     | 否     |
| **2012 许可证服务器**         | 是    | 是    | 否     | 否     |
| **2012 R2 许可证服务器**      | 是    | 是    | 否     | 否     |
| **2016 许可证服务器**         | 是    | 是    | 是    | 否     |
| **2019 许可证服务器**         | 是    | 是    | 是    | 是    |

任何 RDS 许可证服务器可以承载来自所有以前版本的远程桌面服务和远程桌面服务的当前版本的许可证。 例如，Windows Server 2016 RDS 许可证服务器可以托管中的 RDS，所有先前版本的许可证，而 Windows Server 2012 R2 RDS 许可证服务器只能承载到 Windows Server 2012 R2 的许可证。

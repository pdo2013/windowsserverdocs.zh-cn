---
title: 使用客户端访问许可证 (CAL) 许可 RDS 部署
description: 远程桌面服务中的客户端授权的概述。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 252ee776946ba0c387d7a6cdf3dc97ffdc55a591
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387585"
---
# <a name="license-your-rds-deployment-with-client-access-licenses-cals"></a>使用客户端访问许可证 (CAL) 许可 RDS 部署

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

连接到远程桌面会话主机的每个用户和设备需要客户端访问许可证 (CAL)。 使用 RD 授权来安装、颁发和跟踪 RDS CAL。  

当用户或设备连接到 RD 会话主机服务器时，RD 会话主机服务器将确定是否需要 RDS CAL。 然后，RD 会话主机服务器从远程桌面许可证服务器请求 RDS CAL。 如果可从许可证服务器获取相应的 RDS CAL，则会将 RDS CAL 颁发给客户端，并且客户端能够连接到 RD 会话主机服务器，并从此处连接到桌面或它们正在尝试使用的应用。

虽然在一个授权宽限期内不需要任何许可证服务器，但在此宽限期过后，客户端必须先具有由许可证服务器颁发的有效 RDS CAL，然后才能登录到 RD 会话主机服务器。

使用以下信息来了解客户端访问授权在远程桌面服务中的工作方式，以及部署和管理你的许可证：

- [使用客户端访问许可证 (CAL) 许可 RDS 部署](#license-your-rds-deployment-with-client-access-licenses-cals)
  - [了解 CAL 模型](#understanding-the-cals-model)
  - [有关 CAL 版本的注释](#note-about-cal-versions)

## <a name="understanding-the-cals-model"></a>了解 CAL 模型

有两种类型的 CAL：

- RDS - 每设备 CAL
- RDS - 每用户 CAL

下表概述了两种类型的 CAL 之间的差异：

| 每设备                                                     | 每用户                                                                         |
|----------------------------------------------------------------|----------------------------------------------------------------------------------|
| CAL 以物理方式分配给每个设备。                   | CAL 分配给 Active Directory 中的用户。                                 |
| CAL 由许可证服务器跟踪。                        | CAL 由许可证服务器跟踪。                                          |
| 无论 Active Directory 成员身份为何，都可以跟踪 CAL。 | 不能在工作组中跟踪 CAL。                                       |
| 最多可以撤消 20% 的 CAL。                              | 无法撤消任何 CAL。                                                      |
| 临时 CAL 的有效期为 52–89 天。                       | 临时 CAL 不可用。                                                |
| 不能过度分配 CAL。                                  | 可以过度分配 CAL（违反远程桌面授权协议）。 |

如果使用每设备模型，则在设备首次连接到 RD 会话主机时，会颁发临时许可证。 该设备第二次连接时，只要激活许可证服务器，并且存在可用 CAL，许可证服务器就会颁发永久的 RDS - 每设备 CAL。

如果使用每用户模型，则不会强制执行授权，并向每个用户授予从任意数量的设备连接到 RD 会话主机的许可证。 许可证服务器颁发来自可用的 CAL 池或过度使用的 CAL 池的许可证。 你必须负责确保你的所有用户具有有效的许可证且没有过度使用的 CAL；否则，你违反了远程桌面服务许可条款。

若要确保遵守远程桌面服务许可条款，请跟踪组织中使用的 RDS - 每用户 CAL 数，并确保有足够的每用户 CAL 安装在所有用户的许可证服务器上。

可以使用远程桌面授权管理器跟踪并生成有关 RDS - 每用户 CAL 的报告。

## <a name="note-about-cal-versions"></a>有关 CAL 版本的注释

由用户或设备使用的 CAL 必须对应于用户或设备连接到的 Windows Server 的版本。 不能使用较旧的 CAL 访问更高版本的 Windows Server，但可以使用较新的 CAL 访问早期版本的 Windows Server。

下表显示了在 RD 会话主机和 RD 虚拟化主机上兼容的 CAL。

|                  |2008 R2 及更早的 CAL|2012 CAL|2016 CAL|2019 CAL|
|---------------------------------|--------|--------|--------|--------|
| **2008、2008 R2 许可证服务器**| 是    | 否     | 否     | 否     |
| **2012 许可证服务器**         | 是    | 是    | 否     | 否     |
| **2012 R2 许可证服务器**      | 是    | 是    | 否     | 否     |
| **2016 许可证服务器**         | 是    | 是    | 是    | 否     |
| **2019 许可证服务器**         | 是    | 是    | 是    | 是    |

任何 RDS 许可证服务器都可以托管来自所有以前版本的远程桌面服务和当前版本的远程桌面服务的许可证。 例如，Windows Server 2016 RDS 许可证服务器可以托管来自所有以前版本的 RDS 的许可证，而 Windows Server 2012 R2 RDS 许可证服务器最多只能托管来自 Windows Server 2012 R2 的许可证。

---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: 创建站点链接设计
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4e0607cf66d41e1747b108a3ecc10562120d9174
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861928"
---
# <a name="creating-a-site-link-design"></a>创建站点链接设计

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

创建使用站点链接连接您的站点的站点链接设计。 站点链接反映的站点间连接和用于传输复制通信的方法。 必须与站点链接连接站点，以便在每个站点的域控制器可以复制 Active Directory 的更改。  
  
## <a name="connecting-sites-with-site-links"></a>将站点与站点链接相连接

若要使用站点链接连接站点，请确定想要连接的站点链接、 在相应的站点间传输容器中，创建站点链接对象和将其命名为站点链接的成员站点。 创建站点链接后，你可以继续将站点链接属性设置。  
  
创建站点链接时，确保站点链接中包含的每个站点。 此外，确保所有站点都连接到彼此通过其他站点链接，以使所做的更改可以到所有其他站点复制在任何站点中的域控制器。 如果您不能执行此操作，在事件查看器指出未连接的站点拓扑中的目录服务日志中生成一条错误消息。  
  
每当将站点添加到新创建的站点链接，确定要添加的站点是否属于其他站点链接，并更改必要的站点的站点链接成员身份。 例如，如果你最初创建的网站时使以默认的第一个站点的链接的成员的站点，请务必将站点添加到新的站点链接之后从默认的第一个站点的链接中删除站点。 如果不从默认的第一个站点的链接中删除该站点，知识一致性检查器 (KCC) 将使基于这两个站点链接，可能会导致不正确的路由的成员身份的路由决策。  
  
若要确定你想要使用的站点链接连接成员站点，使用位置和链接在"地理位置和通信链接"(DSSTOPO_1.doc) 工作表中记录的位置的列表。 如果多个站点具有相同的连接和对每个其他可用性，可以使用相同的站点链接对它们进行连接。  
  
站点间传输容器提供了用于映射到的链接使用的传输的站点链接的方式。 创建站点链接对象时，你创建它的 IP 容器，将站点链接与远程过程调用 (RPC) 通过 IP 传输相关联或将站点链接与 SMTP 相关联的简单邮件传输协议 (SMTP) 容器中传输。  
  
> [!NOTE]  
> SMTP 复制将不支持在将来版本的 Active Directory 域服务 (AD DS);因此，不建议在 SMTP 容器中创建站点链接对象。  
  
在相应的站点间传输容器中创建站点链接对象时，AD DS 使用 RPC 通过 IP 站点间和站点内复制之间传输域控制器。 若要确保数据安全在传输过程中，通过 IP 复制的 RPC 使用这两个 Kerberos 身份验证协议和数据加密。  
  
时直接 IP 连接不可用，可以配置站点以使用 SMTP 之间的复制。 但是，SMTP 复制功能限制，需要企业证书颁发机构 (CA)。 SMTP 可以仅复制到配置、 架构和应用程序目录分区，并且不支持的域目录分区复制。  
  
若要命名为站点链接，请使用一致的命名方案，例如 name_of_site1 name_of_site2。 记录站点、 链接的站点和连接的工作表中这些站点的站点链接的名称的列表。 若要帮助您记录站点名称和关联的站点链接名称的工作表，请参阅[作业辅助工具为 Windows Server 2003 部署工具包](https://go.microsoft.com/fwlink/?LinkID=102558)，下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，和打开"站点和关联的站点链接"(DSSTOPO_5.doc)。  
  
## <a name="in-this-guide"></a>本指南包含的内容

[设置站点链接属性](Setting-Site-Link-Properties.md)  

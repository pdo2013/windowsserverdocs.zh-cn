---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: "创建站点链接设计"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9eb54781035424c9a5210e11fbdeafc55496c6c3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-link-design"></a>创建站点链接设计

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

创建网站的链接设计联系站点链接你的站点。 站点链接反映站点间连接和用于传输复制交通的方法。 您必须连接站点使用网站的链接，以便域控制器在每个站点可以复制 Active Directory 更改。  
  
## <a name="connecting-sites-with-site-links"></a>将站点连接与站点链接  
站点联系网站的链接，确定你想要联系站点链接在各自的站点间传输容器中, 创建站点链接对象，然后将该网站的链接成员站点。 创建的站点链接后，你可以继续进行设置链接属性该站点。  
  
创建网站链接时，请确保每个站点位于站点链接。 此外，确保所有站点已都连接到彼此通过其他站点链接以便所做的更改可以所有其他站点到复制的域控制器中的任何网站。 如果你未执行此操作，一条错误消息生成的目录服务日志中事件查看器指出站点拓扑不已连接。  
  
只要将站点添加到新建的站点链接，确定添加该网站是否成员的其他站点链接，并且更改站点链接成员资格，如果需要的站点。 例如，如果最初创建该站点时进行了成员 Default-First-Site-Link 的站点，请确保将站点添加到一个新站点链接后 Default-First-Site-Link 删除该站点。 如果没有从 Default-First-Site-Link 删除该站点，知识一致性检查 (KCC) 将根据会员的这两个网站的链接，这可能导致错误路由路由决策。  
  
要找出你想要使用某个网站的链接连接成员站点，可使用位置和记录"地理位置和通信的链接"(DSSTOPO_1.doc) 工作表中的链接的位置列表。 多个站点具有相同的连接和彼此可用性，如果你可以连接他们具有相同的站点链接。  
  
间站点传输容器提供意味着链接使用传输到映射网站的链接。 当您创建站点链接对象时，你可以在 IP 容器，通过 IP 传输将与远程过程调用 (RPC) 的网站的链接或简单邮件传输协议 (SMTP) 容器，将该站点链接 SMTP 传输与相关联中创建它。  
  
> [!NOTE]  
> SMTP 复制将不受支持的 Active Directory 域服务 (广告 DS); 未来版本中因此，不建议 SMTP 容器在创建网站链接对象。  
  
当你在各自的站点间传输容器创建站点链接对象时，广告 DS RPC 通过使用 IP 之间域控制器传输站点间和站点内复制。 若要防止数据安全在乘公交时，通过 IP 复制 RPC 使用这两个 Kerberos 身份验证协议和数据加密。  
  
当直接 IP 连接不可用时，你可以配置站点使用 SMTP 之间复制。 但是，SMTP 复制功能有限并且需要企业证书颁发机构）。 SMTP 配置、架构和应用程序目录分区只能复制和不支持的域目录分区复制。  
  
若要名称网站的链接、使用一致的命名方案，如 name_of_site1 name_of_site2。 记录的站点、链接的站点和链接名称的站点连接的工作表中的这些站点的列表。 为了帮助您录制站点和关联的网站的链接名称工作表中，请参阅 Windows Server 2003 部署工具包工作辅助 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下载 < DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip >，并打开"站点和关联网站的链接"(DSSTOPO_5.doc)。  
  
## <a name="in-this-guide"></a>本指南中  
[设置站点链接属性](Setting-Site-Link-Properties.md)  
  



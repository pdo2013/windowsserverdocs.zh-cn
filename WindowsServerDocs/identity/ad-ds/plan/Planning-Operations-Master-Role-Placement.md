---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: "计划操作母版角色位置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d271ed3a54e78106aed0d4dbfdb610cc8b9a460
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="planning-operations-master-role-placement"></a>计划操作母版角色位置

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

Active Directory 域服务(广告 DS) 支持多主机复制的目录数据，这意味着任何域控制器可以接受目录更改，并复制到其他所有域控制器的更改。 但是，某些更改，如方案修改是不切实际执行多主机的方式。 出于此原因某些域控制器，称为操作主机按角色负责接受对某些特定更改请求。  
  
> [!NOTE]  
> 必须能够某些信息写入 Active Directory 数据库操作主机角色持有者。 仅阅读域控制器 (RODC) 上的 Active Directory 数据库只读自然景观，因为 Rodc 不能充当操作主机角色持有者。  
  
在每个域中存在的三个操作主机角色（也称为灵活一个主机操作或 FSMO）：  
  
-   主域控制器 (PDC) 仿真器操作主机进程密码的所有更新。  
  
-   相对 ID (RID) 操作主机维护的域的全球 RID 池，并分配给所有域控制器，以确保在域中创建的所有安全原则所都使用的唯一标识符本地 Rid 池。  
  
-   给定的域的基础结构操作主机维护它的域中的组成员的其他域中的安全要求列表。  
  
三个级别域操作主机角色，除了在每个森林存在两项操作主的角色：  
  
-   方案操作主机对本架构进行更改。  
  
-   域名操作主机添加并到和树林中移除域和其他目录分区（如域名系统 (DNS) 应用程序分区）。  
  
将举办了网络可靠性值较高，领域这些操作主机角色域控制器，然后确保 PDC 模拟器和 RID 主机都始终可用。  
  
创建给定域中的第一个域控制器时，将自动分配操作主角色持有者。 这两个森林级别角色（架构主机和域名主机）分配给创建树林中的第一个域控制器。 此外，三域级别角色（RID 主机、母版基础结构，以及 PDC 模拟器）分配的第一个在域中创建的域控制器。  
  
> [!NOTE]  
> 仅当创建新的域，并在当前的角色所有者降级时进行自动操作主角色持有人分配。 所有其他更改角色所有者已由管理员启动。  
  
这些自动操作主机角色分配上创建森林或域中的第一个域控制器可能会导致高 CPU 使用率。 若要避免此问题，给（传输）操作主机角色森林或域中的各种域控制器。 将主机操作主机可靠网络的区域中的角色域控制器，然后操作母版可通过森林中的所有其他域控制器。  
  
你还应该指定待机（可选）操作所有操作的母版主机角色。 备用操作母版将的你可能传输操作主机角色，以防原始角色持失败的域控制器。 确保备用操作母版直接复制合作伙伴的实际操作母版。  
  
## <a name="planning-the-pdc-emulator-placement"></a>计划 PDC 仿真器位置  
PDC 模拟器处理客户端密码更改。 只有一个域控制器充当森林中的每个域 PDC 仿真器。  
  
即使域控制器升级到 Windows 2000、Windows Server 2003 和 Windows Server 2008、域运行级别 Windows 2000 本机正常工作，PDC 模拟器接收优先复制由其他域控制器在域中执行的密码更改。 如果最近更改了密码，所做的更改所需时间将复制到每个域控制器在域中。 如果登录身份验证失败在另一个域控制器，由于错误的密码，该域控制器转发身份验证请求 PDC 模拟器到之前决定是否接受或拒绝登录尝试。  
  
将 PDC 模拟器放置包含大量的用户从该域转发操作，如果需要密码的位置。 此外，请确保位置也会连接到其他位置，以最小化复制延迟。  
  
工作表中记录你计划放置 PDC 模拟器和每个便会出现在每个位置的域的用户数有关的信息向你提供帮助，请参阅 Windows Server 工作辅助 2003 部署工具包 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip) 域控制器的位置。  
  
你需要的有关你需要将 PDC 模拟器部署区域时的位置信息，请参考。 有关部署区域的域的详细信息，请参阅[部署 Windows Server 2008 区域域](https://technet.microsoft.com/library/cc755118.aspx)。  
  
## <a name="requirements-for-infrastructure-master-placement"></a>需求的基础结构主位置  

基础主机更新将添加到它自己的域中的组其他域来自安全主体的名称。 例如，如果用户某个域中的第二个域中的组成员，并且用户的名称已更改的第一个域中，第二个域不通知，必须在列表中的组成员更新用户的名称。 因为一个域中的域控制器不复制到另一个域中的域控制器安全原则，第二个域永远不会注意到在基础主机缺少更改。  
  
基础结构母版监视器不断组成员资格，寻找来自其他域安全原则。 如果找到一个，它将检查与安全主体域验证已更新的信息。 过期的信息时，基础结构母版执行此更新，并改动然后复制到其域中的其他域控制器。  
  
此规则适用两个例外。 首先，如果所有域控制器服务器全球目录，域控制器承载主基础角色是不重要因为全球目录复制更新的信息，无论他们属于的域。 其次，森林具有只有一个域，域控制器承载主基础角色不重要因为来自其他域安全主体不存在。  
  
不要放置也是一个全球目录服务器的域控制器上的基础结构母版。 如果的基础结构母版和全球目录相同的域控制器上，将无法正常基础结构母版。 基础结构母版从不查看已过期; 的数据因此，它将向其他域控制器在域中永远不会复制的任何更改。  
  
## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>操作主机使用有限连接网络的位置  
请注意，如果您的环境的功能有中心位置或中心网站，你可以在其中放置操作主机角色持有者，取决于这些操作可用性某些域控制器操作掌握的角色可能会影响持有者。  
  
例如，假设组织创建站点，B C，并且数码站点链接之间存在一个和 B，以及 C 和数码网络连接 B C，之间完全镜像的网络连接性的站点的链接。 在此示例中，主机角色放入的所有操作都网站和选项**桥所有都站点链接**未选择。  
  
尽管在所有的站点之间的成功复制结果此配置，operations 主机角色函数有以下限制：  
  
-   域控制器在站点 C 和 D 无法访问 PDC 中的模拟器站点 A 若要更新密码，或检查最近已更新的密码。  
  
-   C 和 D 站点中的域控制器无法访问网站以获取 Active Directory 安装完成后的一个初始 RID 池并刷新 RID 池，因为它们被耗尽 A RID 的主机。  
  
-   域控制器在站点 C 和 D 无法添加或删除目录、DNS 或自定义应用程序分区。  
  
-   域控制器在站点 C 和 D 能方案更改。  
  
以帮助你在规划运营主机角色位置工作表中，请参阅有关工作辅助[Windows Server 2003 部署工具包](https://go.microsoft.com/fwlink/?LinkID=102558)、下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，并打开 (DSSTOPO_4.doc) 域控制器的位置。  
  
你将需要创建森林根域和区域时，请参考此信息。 有关部署的森林根域的详细信息，请参阅部署[部署 Windows Server 2008 森林根域](https://technet.microsoft.com/library/cc731174.aspx)。 有关部署区域的域的详细信息，请参阅[部署 Windows Server 2008 区域域](https://technet.microsoft.com/library/cc755118.aspx)。  
  



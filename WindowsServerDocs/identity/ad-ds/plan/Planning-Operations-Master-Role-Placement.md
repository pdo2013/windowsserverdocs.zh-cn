---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: 规划操作主机角色放置
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a3fbe76302199888dce19f845b1c838c07facb82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870888"
---
# <a name="planning-operations-master-role-placement"></a>规划操作主机角色放置

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Active Directory 域服务 (AD DS) 支持多主机复制的目录数据，这意味着任何域控制器可以接受目录更改并将所做的更改复制到所有其他域控制器。 但是，某些更改，例如架构修改是不实际执行多主机的方式。 为此特定域控制器，名为操作主机，负责接受某些特定更改的请求充当的角色。  
  
> [!NOTE]  
> 操作主机角色持有者必须能够向 Active Directory 数据库写入某些信息。 只读域控制器 (RODC) 上的 Active Directory 数据库的只读性质**Rodc 不能充当操作主机角色持有者**。  
  
每个域中存在三个操作主机角色 （也称为灵活单主机操作或 FSMO）：  
  
- 主域控制器 (PDC) 仿真器操作主机处理所有密码更新。  

- 相对 ID (RID) 操作主机维护域的全局 RID 池，并分配到所有域控制器，以确保在域中创建的所有安全主体都具有唯一标识符的本地 Rid 池。  
- 给定域的基础结构操作主机维护从其域中的组成员的其他域的安全主体的列表。  

除了三个域级操作主机角色，在每个林中存在两个操作主机角色：  
  
- 在架构操作主机控制对架构的更改。  
- 域命名操作主机添加并到和从林中删除域和其他目录分区 （例如，域名系统 (DNS) 应用程序分区）。  
  
将托管在网络可靠性较高，几个方面这些操作主机角色的域控制器，并确保 PDC 仿真器和 RID 主机是始终可用。  
  
创建给定域中的第一个域控制器时，将自动分配操作主机角色持有者。 （架构主机和域命名主机） 的两个林级角色分配给在林中创建的第一个域控制器。 此外，（RID 主机、 基础结构主机和 PDC 仿真器） 的三个域级角色分配给域中创建第一个域控制器。  
  
> [!NOTE]  
> 仅当创建新的域和当前角色拥有者副本降级后进行自动操作主机角色持有者分配。 向角色所有者的所有其他更改必须由管理员启动。  
  
在林或域中创建的第一个域控制器上，这些自动操作主机角色分配可能会导致 CPU 使用率非常高。 若要避免此问题，（传输） 向分配操作主机角色在林或域中的不同域控制器。 将主机操作主机在网络不可靠的部分中的角色的域控制器和操作主机可以通过在林中的所有其他域控制器。  
  
您还应该指定备用 （备选） 操作主机的所有操作主机角色。 备用操作主机是域控制器，你无法将传输到其中的操作主机角色在原角色拥有者故障的情况下。 请确保备用操作主机是实际操作主机的直接复制伙伴。  
  
## <a name="planning-the-pdc-emulator-placement"></a>规划 PDC 模拟器放置

PDC 仿真器处理客户端密码更改。 只有一个域控制器将作为每个林中的域中的 PDC 仿真器。  
  
即使所有域控制器都升级到 Windows 2000，Windows Server 2003 和 Windows Server 2008 和域运行在 Windows 2000 本机功能级别，PDC 模拟器接收的执行密码更改的优先复制在域中其他域控制器。 如果密码最近已更改，所做的更改需要花费时间复制到域中每个域控制器。 如果密码错误的另一个域控制器在失败的登录身份验证，该域控制器将决定是否接受或拒绝尝试登录之前将身份验证请求转发到 PDC 仿真器上。  
  
将 PDC 模拟器放在包含大量用户密码转发操作，如果需要在这个域中的位置。 此外，请确保该位置也连接到其他位置，以便尽量减少复制延迟。  
  
工作表以帮助你记录有关你打算将 PDC 仿真器和在每个位置中的呈现每个域的用户数的信息，请参阅作业辅助工具为 Windows Server 2003 部署工具包 ([ https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，请下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，并打开域控制器放置 (DSSTOPO_4.doc)。  
  
您需要对您需要时部署地区域将 PDC 仿真器的位置有关的信息，请参阅。 有关部署地区域的详细信息，请参阅[部署 Windows Server 2008 地区域](https://technet.microsoft.com/library/cc755118.aspx)。  
  
## <a name="requirements-for-infrastructure-master-placement"></a>基础结构主机放置要求  

基础结构主机更新来自其他域添加到其自己的域中的组的安全主体的名称。 例如，如果一个域中的用户是第二个域中的组的成员，并且在第一个域中更改用户的名称，第二个域不通知用户的名称必须在组的成员身份列表进行更新。 因为一个域中的域控制器不复制到另一个域中的域控制器的安全主体，第二个域不会知道这一基础结构主机没有更改。  
  
基础结构主机连续监视组成员身份，寻找来自其他域的安全主体。 如果找到，它会检查与要验证已更新信息的安全主体的域。 如果信息已过时，基础结构主机执行更新，然后将更改复制到其域中的其他域控制器。  
  
两种例外情况应用于该规则。 首先，如果所有域控制器都是全局编录服务器，承载基础结构主机角色的域控制器并不重要由于全局编录复制更新的信息，而不考虑到其所属的域。 其次，如果林仅有一个域，承载基础结构主机角色的域控制器并不重要因为不存在来自其他域的安全主体。  
  
不要将基础结构主机放置也是全局编录服务器在域控制器上。 如果基础结构主机和全局编录位于同一个域控制器上，则基础结构主机将不会运行。 基础结构主机将永远不会查找已过时; 的数据因此，它将永远不会将任何更改复制到域中的其他域控制器。  
  
## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>操作主机网络的位置通过有限的连接

请注意如果您的环境确实有一个中心位置或可以在其中放置操作主机角色持有者的中心站点，这些操作的可用性取决于特定域控制器操作主的角色拥有者可能会受到影响。  
  
例如，假设组织创建站点 A，B，C，并且之间不存在 D.站点链接 A 和 B，B 和 C，C 和 d。 网络连接之间以及之间完全镜像站点链接的网络连接。 在此示例中，所有操作主机角色放置在都站点 A，将选项**桥接所有都站点链接**未选中。  
  
尽管此配置导致的所有站点之间成功复制，但操作主机角色函数具有以下限制：  
  
- 站点 C 和 D 中的域控制器无法访问 PDC 仿真器在站点 A 中，更新密码或检查其最近已更新的密码。  
- 站点 C 和 D 中的域控制器无法访问在站点 A 获取 Active Directory 安装后的初始的 RID 池和刷新的 RID 池，因为它们被耗尽的 rid。  
- 站点 C 和 D 中的域控制器不能添加或删除目录、 DNS 或自定义应用程序分区。  
- 站点 C 和 D 中的域控制器不能进行架构更改。  
  
若要帮助你规划操作主机角色分配的工作表，请参阅[作业辅助工具为 Windows Server 2003 部署工具包](https://go.microsoft.com/fwlink/?LinkID=102558)、 下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，并打开域控制器放置 (DSSTOPO_4.doc)。  
  
需要时创建的目录林根域和地区域，请参考此信息。 有关部署目录林根域的详细信息，请参阅 Deploying[部署 Windows Server 2008 目录林根域](https://technet.microsoft.com/library/cc731174.aspx)。 有关部署地区域的详细信息，请参阅[部署 Windows Server 2008 地区域](https://technet.microsoft.com/library/cc755118.aspx)。  

## <a name="next-steps"></a>后续步骤

有关 FSMO 角色布局的其他信息可在支持主题[FSMO 放置和 Active Directory 域控制器上的优化](https://support.microsoft.com/help/223346)

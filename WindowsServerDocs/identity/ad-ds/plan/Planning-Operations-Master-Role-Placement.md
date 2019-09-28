---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: 规划操作主机角色放置
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: eb17ed55ba7d7ba23d21162fd41f4022821948fe
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402530"
---
# <a name="planning-operations-master-role-placement"></a>规划操作主机角色放置

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory 域服务（AD DS）支持对目录数据进行多主机复制，这意味着任何域控制器都可以接受目录更改并将更改复制到所有其他域控制器。 但是，某些更改（如架构修改）对于以多主机方式执行是不切实际的。 出于此原因，某些域控制器（称为操作主机）包含负责接受特定更改请求的角色。  
  
> [!NOTE]  
> 操作主机角色持有者必须能够将一些信息写入 Active Directory 数据库。 由于只读域控制器（RODC）上 Active Directory 数据库的只读特性， **rodc 不能充当操作主机角色持有**者。  
  
每个域中都存在三个操作主机角色（也称为灵活单主机操作或 FSMO）：  
  
- 主域控制器（PDC）仿真器操作主机处理所有密码更新。  

- 相对 ID （RID）操作主机维护域的全局 RID 池，并将本地 Rid 池分配给所有域控制器，以确保域中创建的所有安全主体都具有唯一的标识符。  
- 给定域的基础结构操作主机将维护作为其域中组的成员的其他域中的安全主体的列表。  

除了三个域级操作主机角色，每个林中都有两个操作主机角色：  
  
- 架构操作主机控制对架构的更改。  
- 域命名操作主机在林中添加和删除域和其他目录分区（例如，域名系统（DNS）应用程序分区）。  
  
将托管这些操作主机角色的域控制器放置在网络可靠性较高的区域中，并确保 PDC 仿真器和 RID 主机一直可用。  
  
当创建了给定域中的第一个域控制器时，将自动分配操作主机角色持有者。 将两个林级角色（架构主机和域命名主机）分配给在林中创建的第一个域控制器。 此外，三个域级角色（RID 主机、基础结构主机和 PDC 模拟器）分配给域中创建的第一个域控制器。  
  
> [!NOTE]  
> 仅当创建新域和将当前角色持有者降级时，才会执行自动操作主机角色持有者分配。 对角色所有者的所有其他更改必须由管理员启动。  
  
这些自动操作主机角色分配可能会导致在林或域中创建的第一个域控制器上的 CPU 使用率非常高。 若要避免这种情况，请将操作主机角色分配到林或域中的各个域控制器。 将承载操作主机角色的域控制器放置在网络可靠性和林中所有其他域控制器可访问的位置。  
  
还应为所有操作主机角色指定备用（备用）操作主机。 备用操作主机是可以将操作主机角色传输到的域控制器，以防原始角色持有者失败。 确保备用操作主机是实际操作主机的直接复制伙伴。  
  
## <a name="planning-the-pdc-emulator-placement"></a>规划 PDC 模拟器位置

PDC 模拟器处理客户端密码更改。 只有一个域控制器充当林中每个域中的 PDC 仿真器。  
  
即使所有域控制器都升级到 Windows 2000、Windows Server 2003 和 Windows Server 2008，并且域在 Windows 2000 本机功能级别运行，PDC 仿真器也会接收执行的密码更改的优先复制由域中的其他域控制器。 如果最近更改了密码，则需要将该更改复制到域中的每个域控制器。 如果由于密码不正确，登录身份验证在另一个域控制器上失败，则该域控制器会将身份验证请求转发给 PDC 模拟器，然后决定是接受还是拒绝登录尝试。  
  
如果需要，请将 PDC 仿真器放置在包含大量用户的域中，以便进行密码转发操作。 此外，请确保位置正确连接到其他位置，以最大程度地减少复制延迟。  
  
要使工作表帮助你记录有关你计划在何处放置 PDC 模拟器的信息，以及每个位置中每个域的用户数的信息，请参阅 Windows Server 2003 部署工具包的作业助手（[https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)）、下载作业_Aids_Designing_and_Deploying_Directory_and_Security_Services，并打开域控制器布局（DSSTOPO_4）。  
  
你需要参考有关在部署区域性域时需要放置 PDC 模拟器的位置的信息。 有关部署地区性域的详细信息，请参阅[部署 Windows Server 2008 地区性域](https://technet.microsoft.com/library/cc755118.aspx)。  
  
## <a name="requirements-for-infrastructure-master-placement"></a>基础结构主机位置的要求  

基础结构主机将更新添加到其自身域中的组的其他域中的安全主体的名称。 例如，如果一个域中的用户是第二个域中某个组的成员，并且在第一个域中更改了用户的名称，则第二个域不会通知用户名称必须在组的成员身份列表中更新。 由于一个域中的域控制器不会将安全主体复制到另一个域中的域控制器，因此第二个域绝不会意识到缺少基础结构主机。  
  
基础结构主机会持续监视组成员身份，查找来自其他域的安全主体。 如果找到一个，它将检查安全主体的域以验证是否已更新信息。 如果信息过期，则基础结构主机会执行更新，然后将更改复制到其域中的其他域控制器。  
  
此规则有两个例外。 首先，如果所有域控制器都是全局编录服务器，则承载基础结构主机角色的域控制器是无意义的，因为无论用户所属的域为何，全局编录都会复制更新的信息。 其次，如果林中只有一个域，则承载基础结构主机角色的域控制器是无意义的，因为来自其他域的安全主体不存在。  
  
不要将基础结构主机置于也是全局编录服务器的域控制器上。 如果基础结构主机和全局编录位于同一个域控制器上，则基础结构主机将无法工作。 基础结构主机绝不会找到过期的数据;因此，它永远不会将任何更改复制到域中的其他域控制器。  
  
## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>具有受限连接的网络的操作主机位置

请注意，如果你的环境中有一个可放置操作主机角色持有者的中央位置或中心站点，则依赖于这些操作主机角色持有者的可用性的某些域控制器操作可能会受到影响。  
  
例如，假设组织创建了站点 A、B、C 和 D。站点 a、b、B 和 c 之间以及 C 和 D 之间存在站点链接。网络连接完全反映了站点链接的网络连接。 在此示例中，所有操作主机角色都放置在站点 A 中，并且未选择 "**桥接所有站点链接**" 选项。  
  
尽管此配置会导致所有站点之间成功复制，但操作主机角色功能具有以下限制：  
  
- 站点 C 和 D 中的域控制器无法访问站点 A 中的 PDC 模拟器来更新密码或检查其是否具有最近更新的密码。  
- 站点 C 和 D 中的域控制器不能访问站点 A 中的 RID 主机，从而在 Active Directory 安装后获取初始 RID 池，并在删除 RID 池时刷新它们。  
- 站点 C 和 D 中的域控制器无法添加或删除目录、DNS 或自定义应用程序分区。  
- 站点 C 和 D 中的域控制器无法进行架构更改。  
  
要使工作表帮助你规划操作主机角色放置，请参阅[Windows Server 2003 部署工具包的作业帮助](https://go.microsoft.com/fwlink/?LinkID=102558)，下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services，并打开域控制器位置（DSSTOPO_4）。  
  
创建目录林根级域和区域域时，需要引用此信息。 有关部署目录林根级域的详细信息，请参阅部署[部署 Windows Server 2008 林根级域](https://technet.microsoft.com/library/cc731174.aspx)。 有关部署地区性域的详细信息，请参阅[部署 Windows Server 2008 地区性域](https://technet.microsoft.com/library/cc755118.aspx)。  

## <a name="next-steps"></a>后续步骤

有关 FSMO 角色放置的其他信息，请参阅支持主题[fsmo 位置和优化 Active Directory 域控制器](https://support.microsoft.com/help/223346)

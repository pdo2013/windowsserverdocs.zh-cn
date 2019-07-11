---
title: dfsrmig
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e1b6a464-6a93-4e66-9969-04f175226d8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cce34684509a72912cc867b2d637477f868d289d
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67792256"
---
# <a name="dfsrmig"></a>dfsrmig

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

`dfsrmig`命令将 SYSvol 复制迁移到分布式文件系统 (DFS) 复制从文件复制服务 (FRS)，提供的迁移，进度有关的信息并会将 active directory 域服务 (AD DS) 对象修改支持迁移。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)本文档后面的部分。
## <a name="syntax"></a>语法
```
dfsrmig [/SetGlobalState <state> | /GetGlobalState | /GetMigrationState | /createGlobalObjects | 
/deleteRoNtfrsMember [<read_only_domain_controller_name>] | /deleteRoDfsrMember [<read_only_domain_controller_name>] | /?]
```
## <a name="parameters"></a>Parameters

|                         参数                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  /SetGlobalState <state>                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     域的所需的全局迁移状态设置为对应于指定的值的状态*状态*。<br /><br />若要继续执行迁移或回滚进程，使用此命令有效状态之间循环。 此选项，可启动并通过在 AD DS 中 PDC 仿真器上设置全局迁移状态来控制迁移过程。 如果 PDC 仿真器不可用，此命令会失败。<br /><br /> 只能为稳定状态设置全局迁移状态。 有效值*状态*，因此， **0**启动状态，为**1**准备就绪状态为**2**重定向状态和**3**已清除状态。<br /><br />迁移到已清除状态是不可逆，则无法进行回滚从该状态，因此请使用值为**3**有关*状态*只有当您处于完全 committd 为 SYSvol 使用 DFS 复制复制。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                      /GetGlobalState                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       从 AD DS 数据库、 在 PDC 仿真器上运行时的本地副本中检索域的当前全局迁移状态。<br /><br />使用此选项以确认正确的全局迁移状态设置。 仅稳定迁移状态可以是全局的迁移状态，因此结果的**dfsrmig**命令会报告与 **/GetGlobalState**选项对应于的状态，可以设置 **/SetGlobalState**选项。<br /><br />应运行**dfsrmig**命令 **/GetGlobalState**仅在 PDC 仿真器上的选项。 active directory 复制会将全局状态复制到域中的其他域控制器，但如果您运行，复制延迟可能会导致不一致**dfsrmig**命令与 **/GetGlobalState** PDC 仿真器的域控制器上的选项。 若要检查 PDC 仿真器的域控制器的本地迁移状态，请使用 **/GetMigrationState**选项。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|                    /GetMigrationState                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     检索域中的所有域控制器的当前本地迁移状态，并确定这些本地状态是否与当前全局迁移状态匹配。<br /><br />使用此选项以确定所有域控制器已都达到全局迁移状态。 输出**dsfrmig**命令使用时 **/GetMigrationState**选项指示是否为当前的全局状态迁移已完成，并且它任何列出的本地迁移状态未达到当前全局迁移状态的域控制器。 域控制器的本地迁移状态可以包括未达到当前全局迁移状态的域控制器的转换状态。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|                   /createGlobalObjects                    | 在 DFS 复制使用的 AD DS 中创建的全局对象和设置。<br /><br />应该不需要使用此选项在正常的迁移过程中，因为 DFS 复制服务会自动创建这些 AD DS 对象和设置，到准备就绪的状态从开始状态迁移过程。 使用此选项以手动在以下情况下创建这些对象和设置：<br /><br />-   **在新的只读域控制器被提升在迁移期间**。 DFS 复制服务自动从启动状态为准备就绪的状态在迁移期间创建的 AD DS 对象和 DFS 复制设置 如果新的只读域控制器在域中升级后这一转换，但之前迁移到已清除状态，然后创建的对象对应于新激活的只读域控制器不在导致复制 AD DS 中和迁移失败。<br />-在这种情况下，可以运行**dfsrmig**命令 wth **/createGlobalObjects**选项是否已具有其任何只读域控制器上手动创建的对象。 运行此命令不会影响已有对象和设置的 DFS 复制服务的域控制器。<br />-   **DFS 复制服务的全局设置缺失或已删除**。 如果这些设置缺少特定的域控制器，请从开始状态迁移到准备就绪状态停留在域控制器正在准备转换状态。 在这种情况下，可以使用**dfsrmig**命令 **/createGlobalObjects**选项来手动创建的设置。 **注意：** PDC 仿真器上创建只读域控制器，DFS 复制服务的全局 AD DS 设置，因为需要将复制到只读域控制器从前，DFS 复制服务在 PDC 模拟器上这些设置只读域控制器可以使用这些设置。 由于 active 目录复制延迟，这种复制可能需要一些时间发生。 |
| /deleteRoNtfrsMember [<read_only_domain_controller_name>] |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    删除全局 AD DS 设置 FRS 复制，对应于指定的只读域控制器，或如果为不指定任何值中删除所有只读域控制器的 FRS 复制的全局 AD DS 设置*read_only_domain_controller_name*。<br /><br />应该不需要使用此选项在正常的迁移过程中，因为从重定向状态为已清除状态在迁移期间，DFS 复制服务将自动删除这些 AD DS 设置。 只读域控制器不能从 AD DS 中删除这些设置，因为 PDC 仿真器执行此操作，并更改最终会复制到只读域控制器后的适用 active directory 复制延迟。<br /><br />使用此选项仅在自动删除只读域控制器无法正常工作且从重定向状态为已清除状态在迁移期间停止长 ime 的只读域控制器时手动删除 AD DS 设置。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| /deleteRoDfsrMember [<read_only_domain_controller_name>]  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          删除全局 AD DS 设置为 DFS 复制，对应于指定的只读域控制器，或如果为不指定任何值中删除所有只读域控制器的 DFS 复制的全局 AD DS 设置*read_only_domain_controller_name*。<br /><br />使用此选项仅在自动删除只读域控制器无法正常工作且长时间停留在只读域控制器时手动删除 AD DS 设置时回滚迁移从准备就绪状态为启动状态的时间。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|                            /?                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              在命令提示符下显示帮助。 等效于运行**dfsrmig**不带任何选项。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |

## <a name="remarks"></a>备注
- dfsrmig.exe，DFS 复制服务的迁移工具将安装 DFS 复制服务。
  对于新的 Windows Server 2008 server，Dcpromo.exe 安装并启动 DFS Replication 服务提升到域控制器的计算机时。 当你将服务器从 Windows Server 2003 升级到 Windows Server 2008 中，升级过程安装并启动 DFS 复制服务。 不需要安装 DFS 复制角色服务 DFS 复制服务在安装并启动。
- **Dfsrmig**工具支持只在运行在 Windows Server 2008 域功能级别的域控制器上，因为从 FRS 向 DFS 复制的 SYSvol 迁移时，才能在域控制器上运行 Windows Server 2008 域功能级别。
- 你可以运行**dfsrmig**上的任何域控制器，但创建或操作 AD DS 操作的命令对象仅允许读写支持域控制器 （不在只读域控制器） 上。
- 运行**dfsrmig**不带任何选项将在命令提示符下显示的帮助。
  ## <a name="BKMK_examples"></a>示例
  若要将全局迁移状态设置为已准备好 (**1**) 并启动迁移到或回滚从准备好的状态，类型：
  ```
  dfsrmig /SetGlobalState 1
  ```
  若要设置全局迁移状态才能启动 (**0**) 并启动回滚到启动状态，类型：
  ```
  dfsrmig /SetGlobalState 0
  ```
  若要显示全局迁移状态，请键入：
  ```
  dfsrmig /GetGlobalState
  ```
  此示例显示典型的输出**dfsrmig /GetGlobalState**命令。
  ```
  Current DFSR global state:  Prepared 
  Succeeded.
  ```
  若要显示有关所有域控制器上的本地迁移状态匹配全局迁移状态和其中的本地状态与全局状态不匹配任何域控制器的本地迁移状态的信息，请键入：
  ```
  dfsrmig /GetMigrationState
  ```
  此示例显示典型的输出**dfsrmig /GetMigrationState**命令时的所有域控制器上的本地迁移状态匹配全局迁移状态。
  ```
  All Domain Controllers have migrated successfully to Global state ( Prepared ).
  Migration has reached a consistent state on all Domain Controllers.
  Succeeded.
  ```
  此示例显示典型的输出**dfsrmig /GetMigrationState**命令时某些域控制器上的本地迁移状态与全局迁移状态不匹配。
  ```
    The following Domain Controllers are not in sync with Global state ( Prepared ):
    Domain Controller (Local Migration State)   DC type
    =========
    CONTOSO-DC2 ( start )   ReadOnly DC
    CONTOSO-DC3 ( Preparing )   Writable DC
    Migration has not yet reached a consistent state on all domain controllers
    State information might be stale due to AD latency.
  ```
若要在其中这些设置过程中未创建自动迁移的域控制器上的 AD DS 中创建 DFS 复制使用的全局对象和设置或其中的这些设置缺失，请键入：
```
dfsrmig /createGlobalObjects
```
若要删除名为 contoso dc2，如果迁移过程将自动删除未被删除这些设置的只读域控制器的 FRS 复制的全局 AD DS 设置，请键入：
```
dfsrmig /deleteRoNtfrsMember contoso-dc2
```
若要删除的所有只读域控制器的 FRS 复制的全局 AD DS 设置，如果这些设置不会自动删除的迁移过程，请键入：
```
dfsrmig /deleteRoNtfrsMember
```
若要删除名为 contoso dc2，如果这些设置不会自动删除的迁移过程的只读域控制器为 DFS 复制的全局 AD DS 设置，请键入：
```
dfsrmig /deleteRoDfsrMember contoso-dc2
```
若要删除所有只读域控制器的 DFS 复制的全局 AD DS 设置，如果这些设置不会自动删除的迁移过程，请键入：
```
dfsrmig /deleteRoDfsrMember
```
## <a name="additional-references"></a>其他参考
[命令行语法项](https://go.microsoft.com/fwlink/?LinkId=122056)

[SYSvol 迁移系列：第 2 部分 dfsrmig.exe:SYSvol 迁移工具](https://go.microsoft.com/fwlink/?LinkID=121757)

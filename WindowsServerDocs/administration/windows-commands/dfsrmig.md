---
title: dfsrmig
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b2f8e3d8840be2393ba986babc6a196226c5bb52
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378390"
---
# <a name="dfsrmig"></a>dfsrmig

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

@No__t-0 命令将 SYSvol 复制从文件复制服务（FRS）迁移到分布式文件系统（DFS）复制，提供有关迁移进度的信息，并将 active directory 域服务（AD DS）对象修改为支持迁移。
有关如何使用此命令的示例，请参阅本文档后面的 "[示例](#BKMK_examples)" 一节。
## <a name="syntax"></a>语法
```
dfsrmig [/SetGlobalState <state> | /GetGlobalState | /GetMigrationState | /createGlobalObjects | 
/deleteRoNtfrsMember [<read_only_domain_controller_name>] | /deleteRoDfsrMember [<read_only_domain_controller_name>] | /?]
```
## <a name="parameters"></a>Parameters

|                         参数                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  /SetGlobalState <state>                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     将域的所需全局迁移状态设置为与*状态*指定的值对应的状态。<br /><br />若要继续进行迁移或回滚进程，请使用此命令来遍历有效状态。 使用此选项，可以通过在 PDC 模拟器的 AD DS 中设置全局迁移状态来启动和控制迁移过程。 如果 PDC 模拟器不可用，则此命令将失败。<br /><br /> 只能将全局迁移状态设置为稳定状态。 因此，*状态*的有效值为**0** （表示开始状态）、 **1**表示已准备状态、 **2**表示重定向状态和**3**表示已消除状态。<br /><br />迁移到已删除状态的操作是不可逆的，并且不能从该状态回滚，因此，仅当你完全 committd 使用用于 SYSvol 复制的 DFS 复制时，才将值**3**用于*状态*。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                      /GetGlobalState                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       在 PDC 模拟器上运行时，从 AD DS 数据库的本地副本中检索域的当前全局迁移状态。<br /><br />使用此选项可确认设置了正确的全局迁移状态。 只有稳定的迁移状态可以是全局迁移状态，因此， **dfsrmig**命令与 **/GetGlobalState**选项一起报告的结果对应于可通过 **/SetGlobalState**选项设置的状态。<br /><br />只应在 PDC 模拟器上运行带有 **/GetGlobalState**选项的**dfsrmig**命令。 active directory 复制将全局状态复制到域中的其他域控制器，但是，如果在域控制器上使用 **/GetGlobalState**选项运行**dfsrmig**命令，复制延迟会导致不一致。而不是 PDC 模拟器。 若要检查 PDC 模拟器以外的域控制器的本地迁移状态，请改用 **/GetMigrationState**选项。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|                    /GetMigrationState                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     检索域中所有域控制器的当前本地迁移状态，并确定这些本地状态是否与当前全局迁移状态匹配。<br /><br />使用此选项确定所有域控制器是否已达到全局迁移状态。 当你使用 **/GetMigrationState**选项时， **dsfrmig**命令的输出指示是否已完成迁移到当前全局状态的情况，并列出尚未到达的任何域控制器的本地迁移状态当前全局迁移状态。 域控制器的本地迁移状态可能包括尚未达到当前全局迁移状态的域控制器的转换状态。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|                   /createGlobalObjects                    | 在 DFS 复制使用的 AD DS 中创建全局对象和设置。<br /><br />不需要在正常的迁移过程中使用此选项，因为 DFS 复制服务会在从启动状态迁移到已准备状态时自动创建这些 AD DS 对象和设置。 使用此选项可以在下列情况下手动创建这些对象和设置：<br /><br />-   **会在迁移过程中升级新的只读域控制器**。 在从开始状态迁移到已准备状态时，DFS 复制服务会自动为 DFS 复制创建 AD DS 对象和设置。 如果新的只读域控制器在此转换后升级到域中，但在迁移到已消除状态之前，不会在导致复制的 AD DS 中创建对应于新激活的只读域控制器的对象，迁移失败。<br />-在这种情况下，你可以运行**dfsrmig**命令配合 **/createGlobalObjects**选项，以在尚未包含这些对象的任何只读域控制器上手动创建这些对象。 运行此命令不会影响已经具有 DFS 复制服务的对象和设置的域控制器。<br />-   **DFS 复制服务的全局设置缺失或已被删除**。 如果特定域控制器缺少这些设置，则从启动状态到已准备状态的迁移将停止，以准备域控制器的转换状态。 在这种情况下，可以将**dfsrmig**命令与 **/createGlobalObjects**选项一起使用，以手动创建设置。 **注意：** 由于只读域控制器的 DFS 复制服务的全局 AD DS 设置是在 PDC 模拟器上创建的，因此这些设置需要从 PDC 仿真器复制到只读域控制器，然后 DFS 复制才能在只读域控制器可以使用这些设置。 由于活动 i 复制延迟，此复制可能需要一段时间。 |
| /deleteRoNtfrsMember [< read_only_domain_controller_name >] |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    删除与指定的只读域控制器对应的 FRS 复制的全局 AD DS 设置，或者，如果没有为 read_only_domain_ 指定任何值，则删除所有只读域控制器的 FRS 复制的全局 AD DS 设置。 *controller_name*。<br /><br />不需要在正常的迁移过程中使用此选项，因为在从重定向状态到已删除状态的迁移过程中，DFS 复制服务会自动删除这些 AD DS 设置。 由于只读域控制器无法从 AD DS 中删除这些设置，因此 PDC 模拟器会执行此操作，并且更改最终会在 active directory 复制适用延迟后复制到只读域控制器。<br /><br />仅当在只读域控制器上自动删除失败时，将使用此选项手动删除 AD DS 设置，并在从重定向状态到已消除状态的迁移过程中禁止显示长 ime 的只读域控制器。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| /deleteRoDfsrMember [< read_only_domain_controller_name >]  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          删除对应于指定只读域控制器的 DFS 复制的全局 AD DS 设置，如果没有为 read_only_domain_ 指定任何值，则删除所有只读域控制器的 DFS 复制的全局 AD DS 设置。 *controller_name*。<br /><br />使用此选项，仅当在只读域控制器上自动删除失败时，并且在将迁移从已准备状态回滚到启动状态时，会长时间停止只读域控制器时，才手动删除 AD DS 设置。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|                            /?                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              在命令提示符下显示帮助。 等效于不带任何选项的情况下运行**dfsrmig** 。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |

## <a name="remarks"></a>备注
- dfsrmig 是 DFS 复制服务的迁移工具，与 DFS 复制服务一起安装。
  对于新的 Windows Server 2008 服务器，当你将计算机提升为域控制器时，Dcpromo.exe 将安装并启动 DFS 复制服务。 将服务器从 Windows Server 2003 升级到 Windows Server 2008 后，升级过程将安装并启动 DFS 复制服务。 您无需安装 DFS 复制角色服务即可安装和启动 DFS 复制服务。
- 仅在 Windows Server 2008 域功能级别运行的域控制器上支持**dfsrmig**工具，因为从 FRS 到 DFS 复制的 SYSvol 迁移仅适用于在 windows server 2008 域中运行的域控制器功能级别。
- 你可以在任何域控制器上运行**dfsrmig**命令，但创建或操作 AD DS 对象的操作仅允许在可读写功能的域控制器上（不是在只读域控制器上）。
- 如果运行不带任何选项的**dfsrmig** ，将在命令提示符下显示帮助。
  ## <a name="BKMK_examples"></a>示例
  若要将全局迁移状态设置为 "已准备好（**1**）" 并启动到准备状态的迁移或回滚，请键入：
  ```
  dfsrmig /SetGlobalState 1
  ```
  若要将全局迁移状态设置为 "启动" （**0**）并启动回退到开始状态，请键入：
  ```
  dfsrmig /SetGlobalState 0
  ```
  若要显示全局迁移状态，请键入：
  ```
  dfsrmig /GetGlobalState
  ```
  此示例显示**dfsrmig/GetGlobalState**命令的典型输出。
  ```
  Current DFSR global state:  Prepared 
  Succeeded.
  ```
  若要显示有关所有域控制器上的本地迁移状态是否匹配全局迁移状态的信息，以及本地状态与全局状态不匹配的任何域控制器的本地迁移状态，请键入：
  ```
  dfsrmig /GetMigrationState
  ```
  此示例显示了在所有域控制器上的本地迁移状态与全局迁移状态匹配时， **dfsrmig/GetMigrationState**命令的典型输出。
  ```
  All Domain Controllers have migrated successfully to Global state ( Prepared ).
  Migration has reached a consistent state on all Domain Controllers.
  Succeeded.
  ```
  此示例显示了某些域控制器上的本地迁移状态与全局迁移状态不匹配时， **dfsrmig/GetMigrationState**命令的典型输出。
  ```
    The following Domain Controllers are not in sync with Global state ( Prepared ):
    Domain Controller (Local Migration State)   DC type
    =========
    CONTOSO-DC2 ( start )   ReadOnly DC
    CONTOSO-DC3 ( Preparing )   Writable DC
    Migration has not yet reached a consistent state on all domain controllers
    State information might be stale due to AD latency.
  ```
若要创建 DFS 复制在迁移过程中不会自动创建这些设置的域控制器 AD DS 中使用的全局对象和设置，请键入：
```
dfsrmig /createGlobalObjects
```
若要删除名为 contoso-dc2 的只读域控制器的 FRS 复制的全局 AD DS 设置，请在迁移过程中键入以下内容：
```
dfsrmig /deleteRoNtfrsMember contoso-dc2
```
若要删除所有只读域控制器的 FRS 复制的全局 AD DS 设置（如果迁移过程未自动删除这些设置），请键入：
```
dfsrmig /deleteRoNtfrsMember
```
若要删除名为 contoso-dc2 的只读域控制器 DFS 复制的全局 AD DS 设置，如果迁移过程未自动删除这些设置，请键入：
```
dfsrmig /deleteRoDfsrMember contoso-dc2
```
若要删除所有只读域控制器的全局 AD DS DFS 复制设置（如果迁移过程未自动删除这些设置），请键入：
```
dfsrmig /deleteRoDfsrMember
```
## <a name="additional-references"></a>其他参考
[命令行语法项](https://go.microsoft.com/fwlink/?LinkId=122056)

@no__t 0SYSvol 迁移系列：第2部分 dfsrmig：SYSvol 迁移工具 @ no__t-0

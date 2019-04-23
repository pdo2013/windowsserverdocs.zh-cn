---
ms.assetid: 73a4deba-7da6-4eae-8fdd-2a4d369f9cbb
title: 虚拟化域控制器技术参考附录
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9e3a5cc2c71455bb040f1311bdbfed1ac7e213fb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832228"
---
# <a name="virtualized-domain-controller-technical-reference-appendix"></a>虚拟化域控制器技术参考附录

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题讲解：  
  
-   [术语](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_Terms)  
  
-   [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)  
  
## <a name="BKMK_Terms"></a>术语  
  
-   **快照**-位于某个特定时间点的虚拟机的状态。 它是依赖于链前面拍摄的快照，在硬件上及虚拟化平台。  
  
-   **克隆**-完成和单独的虚拟机的副本。 它所依赖的虚拟硬件 （虚拟机监控程序）。  
  
-   **完全克隆**-完整克隆是克隆操作完成后不共享任何资源与父虚拟机的虚拟机的独立副本。 正在进行的操作的完整克隆是完全独立于父虚拟机。  
  
-   **差异磁盘**-实时的方式与父虚拟机共享的虚拟磁盘的虚拟机的副本。 这通常可以节省磁盘空间，并允许多个虚拟机要使用相同的软件安装。  
  
-   **VM 复制**-文件系统复制的所有相关文件和文件夹的虚拟机。  
  
-   **将 VHD 文件复制**-虚拟机的 VHD 的副本  
  
-   **VM 生成 ID** -提供给虚拟机的虚拟机监控程序的 128 位整数。 此 ID 是存储在内存中，每次应用快照重置。 设计使用不限的虚拟机监控程序的机制，用于公开虚拟机中的 VM 生成 ID。 HYPER-V 实现公开虚拟机的 ACPI 表中的 ID。  
  
-   **导入/导出**-允许用户保存整个虚拟机 （VM 文件、 VHD 和计算机配置） 的 HYPER-V 功能。 然后允许用户使用该集的文件重新打开同一个 VM （还原） 的同一台计算机将在计算机上另一台计算机作为同一个 VM （移动） 或新的 VM （复制）  
  
## <a name="BKMK_FixPDCPerms"></a>FixVDCPermissions.ps1  
  
```  
# Unsigned script, requires use of set-executionpolicy remotesigned -force  
# You must run the Windows PowerShell console as an elevated administrator  
  
# Load Active Directory Windows PowerShell Module and switch to AD DS drive  
import-module activedirectory  
cd ad:  
  
## Get Domain NC  
$domainNC = get-addomain  
  
## Get groups and obtain their SIDs   
$dcgroup = get-adgroup "Cloneable Domain Controllers"  
  
$sid1 = (get-adgroup $dcgroup).sid  
  
## Get the DACL of the domain  
$acl = get-acl $domainNC  
  
## The following object specific ACE grants extended right 'Allow a DC to create a clone of itself' for the CDC group to the Domain NC  
## 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e is the schemaIDGuid for 'DS-Clone-Domain-Controller"  
  
$objectguid = new-object Guid 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e  
$ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid1,"ExtendedRight","Allow",$objectguid  
  
## Add the ACE in the ACL and set the ACL on the object   
  
$acl.AddAccessRule($ace1)  
set-acl -aclobject $acl $domainNC  
write-host "Done writing new VDC permissions."  
cd c:   
```  
  



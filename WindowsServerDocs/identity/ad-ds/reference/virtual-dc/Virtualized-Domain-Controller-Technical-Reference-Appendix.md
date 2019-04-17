---
ms.assetid: 73a4deba-7da6-4eae-8fdd-2a4d369f9cbb
title: "虚拟化的域控制器技术参考附录"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2e7f264a098b6f67d98c9aa47ec5794374b8920d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="virtualized-domain-controller-technical-reference-appendix"></a>虚拟化的域控制器技术参考附录

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍：  
  
-   [术语](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_Terms)  
  
-   [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)  
  
## <a name="BKMK_Terms"></a>术语  
  
-   **快照**-在特定的时间点虚拟机的状态。 它是依赖于链拍摄以前的快照，的硬件上及虚拟化平台。  
  
-   **克隆**-完成并单独虚拟机的副本。 它是依赖虚拟硬件 （虚拟机监控程序）。  
  
-   **完整克隆**-完整克隆是独立克隆操作后，与父虚拟机共享任何资源虚拟机的副本。 正在进行的操作的完整克隆是完全保存在远离家长虚拟机。  
  
-   **差异磁盘**-实时的方式与父虚拟机共享虚拟磁盘虚拟机的副本。 通常，这可以节省磁盘空间，并允许使用相同的安装软件的多个虚拟机。  
  
-   **VM 复制**，文件系统份相关的所有文件和文件夹的虚拟机。  
  
-   **文件复制过程 VHD** -虚拟机的 VHD 一份  
  
-   **VM 代 ID** -提供给虚拟机监控程序一个 128 整数。 此 ID 每次应用快照重置并存储在内存。 设计用于虚拟机监控程序无关机制研究用户在虚拟机 VM 代 ID。 HYPER-V 实现公开在虚拟机的 ACPI table ID。  
  
-   **导入月导出**-HYPER-V 功能，使用户可以保存整个虚拟机 （VM 文件、 VHD 和计算机配置）。 然后使用该设置的文件以使同一 VM （还原），为同一台计算机上重新计算机用户允许相同 VM （移动），或新虚拟机 （复制） 的其他计算机上  
  
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
  



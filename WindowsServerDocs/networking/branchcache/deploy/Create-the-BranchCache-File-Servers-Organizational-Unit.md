---
title: 创建 BranchCache 文件服务器组织单位
description: 本主题是适用于 Windows Server 2016 的 BranchCache 部署指南的一部分，它演示了如何在分布式和托管缓存模式下部署 BranchCache，以优化分支机构中的 WAN 带宽使用情况
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f058dbec42997f22106666b014508191a8861694
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406490"
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>创建 BranchCache 文件服务器组织单位

>适用于：Windows Server（半年频道）、Windows Server 2016

可以使用此过程在 BranchCache 文件服务器 Active Directory 域服务（AD DS）中创建组织单位（OU）。  
  
“Domain Admins”中的成员身份或同等身份是执行此过程的最低要求。  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>创建 BranchCache 文件服务器组织单位  
  
1.  在安装了 AD DS 的计算机上的服务器管理器中，单击 "**工具**"，然后单击 " **Active Directory 用户和计算机**"。 此时将打开 Active Directory 用户和计算机 "控制台。  
  
2.  在 "Active Directory 用户和计算机" 控制台中，右键单击要向其添加 OU 的域。 例如，如果域的名称为 example.com，请右键单击 " **example.com**"。 指向 **“新建”** ，然后单击 **“组织单位”** 。 此时将打开 "**新建对象-组织单元**" 对话框。  
  
3.  在 "**新建对象-组织单元**" 对话框的 "**名称**" 中，键入新 OU 的名称。 例如，如果想要命名 OU BranchCache 文件服务器，请键入 " **BranchCache 文件服务器**"，然后单击 **"确定"** 。  
  



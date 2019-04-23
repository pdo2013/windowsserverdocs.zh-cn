---
title: 创建 BranchCache 文件服务器组织单位
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b7b26ec5808f5b11141e81dc5e738c83c94ef6b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874078"
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>创建 BranchCache 文件服务器组织单位

>适用于：Windows 服务器 （半年频道），Windows Server 2016

您可以使用下列步骤创建组织单位 (OU) BranchCache 文件服务器的 Active Directory 域服务 (AD DS) 中。  
  
“Domain Admins”中的成员身份或同等身份是执行此过程的最低要求。  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>若要创建的 BranchCache 文件服务器组织单位  
  
1.  在其中安装 AD DS，服务器管理器中的计算机上单击**工具**，然后单击**Active Directory 用户和计算机**。 此时将打开 Active Directory 用户和计算机控制台。  
  
2.  在 Active Directory 用户和计算机控制台中，右键单击你想要添加 OU 的域。 例如，如果你的域名为 example.com，右键单击**example.com**。 指向 **“新建”**，然后单击 **“组织单位”**。 **新建对象-组织单位**对话框随即打开。  
  
3.  在中**新建对象-组织单位**对话框中**名称**，键入新 OU 的名称。 例如，如果你想要名称 OU BranchCache 文件服务器，键入**BranchCache 文件服务器**，然后单击**确定**。  
  



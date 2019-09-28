---
title: 通过服务连接点配置客户端自动托管缓存发现
description: 本指南说明如何在运行 Windows Server 2016 和 Windows 10 的计算机上以托管缓存模式部署 BranchCache
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 60ccc8b80537da0d0b689f6c508c75ef15a339c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406398"
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>通过服务连接点配置客户端自动托管缓存发现

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

使用此过程，你可以使用组策略在运行以下 BranchCache @ no__t-1capable Windows 操作系统的域 @ no__t-0joined 计算机上启用和配置 BranchCache 托管缓存模式。

- Windows 10 企业版
- Windows 10 教育版
- Windows 8.1 企业版
- Windows 8 企业版

> [!NOTE]  
> 若要配置运行 Windows Server 2008 R2 或 Windows 7 的已加入域的计算机，请参阅 Windows Server 2008 R2 [BranchCache 部署指南](https://technet.microsoft.com/library/ee649232.aspx)。

“Domain Admins”中的成员身份或同等身份是执行此过程的最低要求。

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>使用组策略为托管缓存模式配置客户端

1. 在安装了 Active Directory 域服务服务器角色的计算机上，打开服务器管理器，选择本地服务器，单击 "**工具**"，然后单击**组策略管理**"。 此时将打开组策略管理控制台。

2. 在组策略管理控制台中，展开以下路径：**林：** *corp.contoso.com*、**域**、 *corp.contoso.com*、**组策略对象**，其中*corp.contoso.com*是要在其中进行操作的 BranchCache 客户端计算机帐户所在的域的名称。已找到配置。

3. 右 @ no__t-0click**组策略对象**"，然后单击"**新建**"。 此时将打开 "**新建 GPO** " 对话框。 在 "**名称**" 中，键入新组策略对象的名称，\(GPO @ no__t-2。 例如，如果想要为对象 BranchCache 客户端计算机命名，请键入 " **Branchcache 客户端计算机**"。 单击 **“确定”** 。

4. 在组策略管理控制台中，确保已选中 "**组策略对象**"，并在详细信息窗格中，右键 @ no__t-1click 你刚刚创建的 GPO。 例如，如果你命名了 GPO BranchCache 客户端计算机，则右 @ no__t-0click **BranchCache 客户端计算机**。 单击**编辑**。 此时将打开组策略管理编辑器控制台。

5. 在组策略管理编辑器控制台中，展开以下路径：**计算机配置**，**策略**，@no__t 2Administrative 模板：策略定义 @no__t-从本地计算机 @ no__t， **Network**， **BranchCache**检索到的文件 @ no__t-1。

6. 单击 " **BranchCache**"，然后在详细信息窗格中，**双击 "no__t-1click"** 。 此时将打开 "**打开 BranchCache** " 对话框。
  
7.  在 "**打开 BranchCache** " 对话框中，单击 "**已启用**"，然后单击 **"确定"** 。

8. 在组策略管理编辑器控制台中，确保仍选中 " **BranchCache** "，然后在详细信息窗格中，双击 "**通过服务连接点启用自动托管缓存发现**"。 此时将打开 "策略设置" 对话框。

9. 在 "**通过服务连接点启用自动托管缓存发现**" 对话框中，单击 "**已启用**"，然后单击 **"确定"** 。

10. 若要使客户端计算机能够从 BranchCache 文件服务器 @ no__t-0based 内容服务器下载和缓存内容，请执行以下操作：在组策略管理编辑器控制台中，确保仍选中 " **BranchCache** "，然后在详细信息窗格中，**为 "网络文件**" 选择 "no__t-1click branchcache"。 此时将打开 "**配置网络文件的 BranchCache** " 对话框。 
11. 在 "**配置网络文件的 BranchCache** " 对话框中，单击 "**启用**"。 在 "**选项**" 中，键入最大往返网络延迟时间的数字值（以毫秒为单位），然后单击 **"确定"** 。
  
    > [!NOTE]
    > 默认情况下，如果往返网络延迟时间超过80毫秒，客户端计算机将从文件服务器缓存内容。
  
12. 若要配置在每台客户端计算机上为 BranchCache 缓存分配的硬盘空间量，请执行以下操作：在组策略管理编辑器控制台中，确保仍选中 " **BranchCache** "，然后在详细信息窗格中，双击 "1click**设置用于客户端计算机缓存的磁盘空间百分比**"。 "**设置用于客户端计算机缓存的磁盘空间百分比**" 对话框将打开。 单击 "**已启用**"，然后在 "**选项**" 中键入一个数字值，该值表示在每台客户端计算机上为 BranchCache 缓存使用的硬盘空间百分比。 单击 **“确定”** 。

13. 指定段在客户端计算机的 BranchCache 数据缓存中有效的默认期限（天）：在组策略管理编辑器控制台中，确保仍选中 " **BranchCache** "，然后在详细信息窗格中**为数据缓存中的段设置**"no__t-1click"。 此时将打开 "在**数据缓存中设置段的生存期**" 对话框。 单击 "**已启用**"，然后在 "详细信息" 窗格中键入所需的天数。 单击 **“确定”** 。

14. 为客户端计算机配置适用于你的部署的其他 BranchCache 策略设置。

15. 通过运行 " **gpupdate/force**" 或重启客户端计算机，在分支机构客户端计算机上刷新组策略。

BranchCache 托管缓存模式部署现已完成。

有关本指南中的技术的其他信息，请参阅[其他资源](11-Bc-Hcm-additional-resources.md)。
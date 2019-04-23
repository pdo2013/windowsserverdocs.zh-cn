---
title: 通过服务连接点配置客户端自动托管缓存发现
description: 本指南说明了在运行 Windows Server 2016 和 Windows 10 的计算机上的托管的缓存模式下部署 BranchCache
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bd77fc76a999517cb8372aec8dfad25b4dd5be3b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829718"
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>通过服务连接点配置客户端自动托管缓存发现

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

使用此过程，可以使用组策略来启用和域上配置 BranchCache 托管缓存模式\-加入的计算机运行以下 BranchCache\-功能的 Windows 操作系统。

- Windows 10 企业版
- Windows 10 教育版
- Windows 8.1 企业版
- Windows 8 企业版

> [!NOTE]  
> 若要配置在运行 Windows Server 2008 R2 或 Windows 7 的已加入域的计算机，请参阅 Windows Server 2008 R2 [BranchCache 部署指南](https://technet.microsoft.com/library/ee649232.aspx)。

“Domain Admins”中的成员身份或同等身份是执行此过程的最低要求。

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>若要使用组策略配置为托管的缓存模式客户端

1. 在其安装 Active Directory 域服务服务器角色的计算机，打开服务器管理器，选择本地服务器，单击**工具**，然后单击**组策略管理**。 将打开组策略管理控制台。

2. 在组策略管理控制台中，展开以下路径：**林：** *corp.contoso.com*，**域**， *corp.contoso.com*，**组策略对象**，其中*corp.contoso.com*是你想要配置 BranchCache 客户端计算机帐户所在的域的名称。

3. 右\-单击**组策略对象**，然后单击**新建**。 **新的 GPO**对话框随即打开。 在中**名称**，键入新组策略对象的名称\(GPO\)。 例如，如果你想要命名的对象 BranchCache 客户端计算机，键入**BranchCache 客户端计算机**。 单击 **“确定”**。

4. 在组策略管理控制台中，确保**组策略对象**将被选中，并且在详细信息窗格右侧\-单击刚创建的 GPO。 例如，如果名为 GPO BranchCache 客户端计算机，右键\-单击**BranchCache 客户端计算机**。 单击**编辑**。 此时将打开组策略管理编辑器控制台。

5. 在组策略管理编辑器控制台中，展开以下路径：**计算机配置**，**策略**，**管理模板：策略定义\(ADMX 文件\)从本地计算机上检索**，**网络**， **BranchCache**。

6. 单击**BranchCache**，然后在详细信息窗格中，双击\-单击**打开 BranchCache**。 **打开 BranchCache**对话框随即打开。
  
7.  在中**打开 BranchCache**对话框中，单击**已启用**，然后单击**确定**。

8. 在组策略管理编辑器控制台中，确保**BranchCache**是仍然处于选中状态，然后在详细信息窗格双\-单击**启用自动托管缓存发现通过服务连接点**。 将打开策略设置对话框。

9. 在中**启用自动托管缓存发现通过服务连接点**对话框中，单击**已启用**，然后单击**确定**。

10. 若要启用 BranchCache 文件服务器客户端计算机下载和缓存内容\-基于内容的服务器：在组策略管理编辑器控制台中，确保**BranchCache**是仍然处于选中状态，然后在详细信息窗格双\-单击**网络文件 BranchCache**。 **配置网络文件 BranchCache**对话框随即打开。 
11. 在中**配置网络文件 BranchCache**对话框中，单击**已启用**。 在中**选项**，键入数值，以毫秒为单位，最大往返网络延迟时间，，然后单击**确定**。
  
    > [!NOTE]
    > 默认情况下，客户端计算机时间超过 80 毫秒的往返网络延迟是否缓存文件服务器中的内容。
  
12. 若要配置 BranchCache 缓存每个客户端计算机上分配的硬盘空间量：在组策略管理编辑器控制台中，确保**BranchCache**是仍然处于选中状态，然后在详细信息窗格双\-单击**设置用于客户端计算机缓存的磁盘空间的百分比**. **设置用于客户端计算机缓存的磁盘空间的百分比**对话框随即打开。 单击**已启用**，然后在**选项**键入一个数字值，表示每个客户端计算机上的 BranchCache 缓存使用的硬盘空间的百分比。 单击 **“确定”**。

13. 若要指定的默认期限，以天为单位的段为有效的客户端计算机上的 BranchCache 数据缓存中：在组策略管理编辑器控制台中，确保**BranchCache**是仍然处于选中状态，然后在详细信息窗格双\-单击**数据缓存中的设置段的存在时间**。 **数据缓存中的设置段的存在时间**对话框随即打开。 单击**已启用**，然后在详细信息窗格中键入您喜欢的天数。 单击 **“确定”**。

14. 根据你的部署需要配置客户端计算机的其他 BranchCache 策略设置。

15. 在分支机构的客户端计算机上刷新组策略，通过运行命令**gpupdate /force**，或通过重新启动客户端计算机。

BranchCache 托管缓存模式部署现已完成。

本指南中的技术的其他信息，请参阅[其他资源](11-Bc-Hcm-additional-resources.md)。
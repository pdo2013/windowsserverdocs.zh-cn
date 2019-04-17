---
title: 配置服务器证书模板
description: 本主题介绍指南部署服务器证书 802.1 X 有线和无线部署部分
manager: brianlic
ms.topic: article
ms.assetid: 8ff610e2-43ca-407f-a828-06d9366e02f0
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e747fedd74dfc55c2c4df457d7f107b618423b8e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-server-certificate-template"></a>配置服务器证书模板

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程配置证书模板该 Active Directory&reg;证书服务 （广告客户服务） 服务器证书的注册到网络上的服务器使用为基础。  
  
配置此模板，时，您可以指定的服务器按应自动接收服务器证书广告客户服务的 Active Directory 组。   
  
下面的过程中包含有关配置为服务器以下类型的所有问题证书模板说明：  
  
- 远程访问服务，包括那些属于 RAS 网关服务器运行的服务器**RAS 和 IAS 服务器**组。  
- 运行的服务器的网络策略 Server (NPS) 服务，属于**RAS 和 IAS 服务器**组。  
  
在这两会员**企业管理员**和根域**域管理员**组是的最低要求才能完成此过程。  
  
### <a name="to-configure-the-certificate-template"></a>若要配置证书模板  
  
1.  在 CA1，在服务器管理器中，单击**工具**，然后单击**证书颁发机构**。 打开证书颁发机构 Microsoft 管理控制台 (MMC)。  
  
2.  在 MMC 中，双击 CA 名称、 右键单击**证书模板**，然后单击**管理**。  
  
3.  证书模板主机将打开。 所有证书模板显示详细信息窗格中。  
  
4.  在详细信息窗格中，单击**RAS 和 IAS 服务器**模板。  
  
5.  单击**操作**菜单，然后依次单击**复制模板**。 模板**属性**对话框中打开。  
  
6.  单击**安全**选项卡。   
  
7.  在**安全**选项卡上，在**组或用户名**，单击**RAS 和 IAS 服务器**。  
  
8.  在**RAS 和 IAS 服务器的权限**下**允许**，确保**注册**是处于选中状态，然后依次选择**自动注册**复选框。 单击**确定**，并关闭证书模板 MMC。  
  
9.  在证书颁发机构 MMC，单击**证书模板**。 在**操作**菜单、 指向**新建**，然后单击**证书颁发模板**。 **启用证书模板**对话框中打开。  
  
10. 在**启用证书模板**，单击配置，你只需证书模板的名称，然后单击**确定**。 例如，如果你未更改默认证书模板名称，请单击**副本的 RAS 和 IAS 服务器**，然后单击**确定**。  
  



---
title: 配置服务器证书模板
description: 本主题是指南为 802.1x 有线和无线部署部署服务器证书的一部分
manager: brianlic
ms.topic: article
ms.assetid: 8ff610e2-43ca-407f-a828-06d9366e02f0
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 238579c945821d19e45dad7e623450d598830596
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886118"
---
# <a name="configure-the-server-certificate-template"></a>配置服务器证书模板

>适用于：Windows 服务器 （半年频道），Windows Server 2016

您可以使用此过程来配置证书模板的 Active Directory&reg;证书服务 (AD CS) 用作的基础到你的网络上的服务器注册的服务器证书。  
  
在配置此模板时，可以指定应自动接收来自 AD CS 服务器证书的 Active Directory 组策略的服务器。   
  
下面的过程包括配置向以下服务器类型的所有颁发证书模板的说明：  
  
- 运行远程访问服务，包括 RAS 网关服务器、 成员的服务器**RAS 和 IAS 服务器**组。  
- 服务器正在运行的网络策略服务器 (NPS) 服务，是的成员**RAS 和 IAS 服务器**组。  
  
在这种成员资格**Enterprise Admins**和根域**Domain Admins**组是完成此过程所需的最低。  
  
### <a name="to-configure-the-certificate-template"></a>若要配置证书模板  
  
1.  在 CA1，在服务器管理器中，单击**工具**，然后单击**证书颁发机构**。 此时将打开证书颁发机构 Microsoft 管理控制台 (MMC)。  
  
2.  在 MMC 中，双击 CA 名称，右键单击**证书模板**，然后单击**管理**。  
  
3.  将打开证书模板控制台。 在详细信息窗格中将显示所有的证书模板。  
  
4.  在详细信息窗格中，单击**RAS 和 IAS 服务器**模板。  
  
5.  单击**操作**菜单，并单击**复制模板**。 该模板**属性**对话框随即打开。  
  
6.  单击 **“安全”** 选项卡。   
  
7.  上**安全**选项卡上，在**组或用户名**，单击**RAS 和 IAS 服务器**。  
  
8.  在中**RAS 和 IAS 服务器的权限**下**允许**，，请确保**注册**是否选择，然后选择**自动注册**检查框。 单击**确定**，并关闭证书模板 MMC。  
  
9.  在证书颁发机构 MMC 中，单击**证书模板**。 上**操作**菜单，依次指向**新建**，然后单击**颁发的证书模板**。 **启用证书模板**对话框随即打开。  
  
10. 在中**启用证书模板**，单击配置，您只需的证书模板的名称，然后单击**确定**。 例如，如果未更改默认证书模板名称，请单击**复制的 RAS 和 IAS 服务器**，然后单击**确定**。  
  



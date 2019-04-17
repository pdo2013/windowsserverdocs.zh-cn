---
ms.assetid: 67d8a8d7-2fbd-4ed7-bb41-75769f942024
title: "配置性能监视"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6a7602cddcaee274d42213cd9365f6d1722dab79
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="configure-performance-monitoring"></a>配置性能监视

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012
  
## <a name="bkmk_ConfigurePerfMon"></a>  
广告 FS 包含自己专用的性能计数器以帮助你监视联合身份验证的服务器和联合身份验证的服务器代理计算机的性能。 若要使用 Performance Monitor 监视广告 FS 服务器的性能，它可创建新的数据收集设置并将广告 FS 计数器添加至该视图。 以下过程介绍了如何配置广告 fs 监视的性能。  
  
#### <a name="to-configure-performance-monitoring-for-ad-fs-using-performance-monitor"></a>配置使用 Performance Monitor 广告 fs 监视性能  
  
1.  在**开始**屏幕上，键入**Performance Monitor**，然后按 ENTER。  
  
2.  控制台树中，展开**收集的数据集**，right\ 单击**用户定义**，指向**新建**，然后单击**数据收集设置**。  
  
    显示创建新的数据收集设置向导。  
  
3.  在**新数据收集设置**，为**名称**新的数据收集设置为键入一个名称 \ (如"广告 FS 性能"\)，单击**手动创建 \(Advanced\)**，然后单击**下一步**。  
  
4.  对于要包含的数据类型，请验证**创建数据日志**选中，则，然后单击下面的数据类型对应的复选框：**性能计数器**，**事件跟踪数据**，**系统配置信息**。  
  
5.  对于性能计数器展开**广告 FS**中**可用计数器**列表，然后依次单击**添加**。  
  
    广告 FS 性能计数器应该会显示在**计数器添加**列表。  
  
6.  当提示你添加事件跟踪提供程序时，请单击**添加**、 选择**广告 FS 事件**和**广告 FS 跟踪**从列表中提供商。  
  
7.  当提示你添加注册表项来监视器上，单击**下一步**。  
  
8.  当提示你要指定的性能数据保存的位置时，你可以接受默认位置 \ (**%systemdrive%\\PerfLogs\\Admin\\***< data\_collector\_set >*，然后单击**下一步**。  
  
9. 当提示你创建的数据收集设置时，请选择**保存并关闭**，然后单击**完成**。  
  
    新的数据收集设置将显示在下的控制台树**用户定义**节点。  
  
10. 使用以下步骤来处理广告 FS 性能计数器：  
  
    -   若要开始使用广告 FS\ 相关计数器性能监视，right\ 单击数据收集设置，你可以添加 \ (如"广告 FS 性能"\)，然后单击**开始**。  
  
    -   若要创建一个报告以查看监视结果的性能，right\ 单击数据收集设置，你可以添加 \ (如"广告 FS 性能"\)，然后单击**最新的报告**。  
  
    -   结束捕获的性能数据，以便你可以查看最新的报告，right\ 单击数据收集设置，你可以添加 \ (如"广告 FS 性能"\)，然后单击**停止**。  
  
    添加并自动编号最新的报告 \(starting at 000001\) 下**Report\\User 定义***\\ < data\_collector\_set >*控制台树中的节点。  
  
## <a name="ad-fs-performance-counters"></a>广告 FS 性能计数器  
下表列出了广告 FS 性能计数器，并介绍了如何很有用监视与联盟服务器或联合身份验证的服务器代理的活动。  
  
|计数器|描述|可以使用上： 
|-----------|---------------|------------------- 
|标记请求|监视发送到包括 SSOAuth 令牌请求联盟服务器令牌请求数。|联合身份验证的服务器 
|每个秒令牌 Requests\|监视发送到包括 SSOAuth 每秒令牌请求联盟服务器令牌请求数。|联合身份验证的服务器  
|联盟元数据的请求|监视联合身份验证的服务器发送传入联盟元数据请求数。|联合身份验证的服务器  
|每秒联盟元数据 Requests\|监视数传入联盟元数据的请求每秒发给联合身份验证的服务器。|联合身份验证的服务器  
|项目分辨率请求|监视数传入联盟元数据的请求每秒发给联合身份验证的服务器。|联合身份验证的服务器  
|项目分辨率 Requests\/秒|监视联合身份验证的服务器发送到每秒项目分辨率终点请求数。|联合身份验证的服务器  
|代理请求|监视发送给联合身份验证的服务器代理传入请求数。|联合身份验证的服务器代理服务器  
|代理 Requests\/秒|监视数发给联合身份验证的服务器代理每秒传入的请求。|联合身份验证的服务器代理服务器  
|代理 MEX 请求|监视发给联合身份验证的服务器代理传入 WS\ 元数据 Exchange \(MEX\) 请求数。|联合身份验证的服务器代理服务器 
|代理 MEX Requests\/秒|监视每秒发给联合身份验证的服务器代理的传入 MEX 请求数。|联合身份验证的服务器代理服务器  
  


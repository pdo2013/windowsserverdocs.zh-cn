---
title: 发行说明-Windows Server 2019 中的重要问题
description: 总结了需要解决方法，以避免崩溃、 挂起、 安装失败和数据丢失的重要问题
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d55-87ef-9e5fd098371f
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: d5d84bb1cd204a5419271cc3668343f8ac9c9a54
ms.sourcegitcommit: dbb4738fdac3b7911952ff11f1eaed9649d6567a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/07/2019
ms.locfileid: "9149887"
---
# 发行说明-Windows Server 2019 中的重要问题

>适用于：Windows Server 2019

这些发行说明汇总了 Windows Server 中最关键的问题&reg;2019年操作系统，包括避免或解决问题，方法，如果已知。 有关通过设计更改、 新功能，并在此版本中的修补程序的信息，请参阅[什么是 Windows Server 2019 中的新增功能](whats-new-19.md)和从特定功能团队的公告。 除非另行指定，每个报告的问题适用于所有版本和 Windows Server 2019 的安装选项。  

本文档将持续更新。 发现需要解决的严重问题，以及有可用的新的解决方法和修补程序时，它们将被添加到此文档中。  
  
## 发行说明
Windows Server 2019 中存在以下已知的问题。 
<table border="1" rules="rows">
  <thead align="left" valign="middle">
    <tr>
      <th>标题</th>
      <th>描述</th>
    </tr>
  </thead>
  <tbody align="left" valign="middle">
    <tr>
      <td>在服务器设置期间安装选项菜单已截断德语文本</td>
      <td>运行安装程序从德国服务器媒体上操作系统选择窗口标题为"选择你想要安装，操作系统"桌面体验安装选项的说明将具有缺少和不正确字符的最末尾句子。 下面是完整德语文本，它应该显示。  
      <br/>
      <p><i>Durch diese 选项 wird 片 vollständige grafische Umgebung von Windows installiert，wodurch zusätzlicher Speicherplatz verbraucht wird。 Sie kann hilfreich sein，wenn Sie den Windows 桌面 verwenden möchten 含有 über eine 应用 verfügen、 die 片 grafische Umgebung benötigt。</i> </p>
      <p>这只会影响在公共可用性的各种 Windows Server 2019、 Windows Server 版本 1809 和 Microsoft HYPER-V Server 2019 释放德语媒体。</p></td>
    </tr>
    <tr>
      <td>在 Windows Server 版本 1809年的安装过程不正确的 Windows Server 品牌图像  </td>
      <td>Windows Server 版本 1809，在设置体验期间的背景图像的一些初始屏幕上显示"Windows Server 2019"。  为 Windows server 版本 1709年和 1803，这应该只需显示"Windows 服务器"。  有其他影响的产品，其他任何地方，并且没有对 Windows Server 2019 产品没有影响。  该问题的 Windows Server 版本 1809，仅适用于批量许可客户访问批量许可服务中心设置期间被限制为此图像。  
      </td>
    </tr>
  </tbody>
</table>


### 版权  
本文档按“原样”提供。 本文档中表达的信息和视图（包括 URL 和其他 Internet 网站引用）如有更改，恕不另行通知。  

本文档未向你提供针对任何 Microsoft 产品的任何知识产权的任何法律权限。 可以复制本文档并将其用于进行内部参考。  

&copy;2019 Microsoft Corporation。 保留所有权利。  

Microsoft、Active Directory、Hyper-V、Windows 和 WindowsServer 是 Microsoft Corporation 在美国和/或其他国家/地区的注册商标或商标。  

本产品包含图形过滤器软件；该软件在一定程度上是以独立 JPEG 小组所做工作为基础的。  


1.0  

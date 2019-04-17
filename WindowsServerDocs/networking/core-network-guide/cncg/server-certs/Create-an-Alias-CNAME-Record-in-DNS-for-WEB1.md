---
title: 在 DNS 为 WEB1 创建别名 (CNAME) 记录
description: 本主题介绍指南部署服务器证书 802.1 X 有线和无线部署部分
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 65f10efe1cfd2866fb99406bf031197f6268b05b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>在 DNS 为 WEB1 创建别名 \(CNAME\) 记录

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程区域中添加别名 canonical 名称记录 \(CNAME\) 资源的 Web 服务器中 DNS 域控制器上。 借助 CNAME 记录，可以使用多个名称指向一个主机，使其可用于轻松执行诸如主机文件传输协议 \(FTP\) 服务器和 Web 服务器同一台计算机上。   
  
出于此原因，你可以随意使用你的 Web 服务器存放证书吊销列出你的证书颁发机构 \(CRL\) \(CA\) 以及执行其他服务，如 FTP 或 Web 服务器。  
  
当你执行此过程时，替换**别名**和适用于你的部署的值的其他变量。  
  
若要执行此过程，你必须**域管理员**。  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>若要向区域中添加别名 \(CNAME\) 资源记录  
  
>[!NOTE]  
>若要执行此过程，方法是使用 Windows PowerShell，请参阅[添加 DnsServerResourceRecordCName](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx)。  
  
1.  在 DC1，在服务器管理器中，单击**工具**，然后单击**DNS**。 打开 DNS 经理 Microsoft 管理控制台 (MMC)。  
  
2.  控制台树中，双击**前进查找区域**，右键单击你想要添加别名资源录制，然后单击转发查找区域**新别名 \(CNAME\)**。 **新资源记录**对话框中打开。  
  
3.  在**别名**，键入别名**pki**。  
  
4.  当你键入的值**别名**、**完整域名 \(FQDN\)**自动-填充的对话框中。 例如，如果你的别名是"pki"以及你域是 corp.contoso.com，则这**pki.corp.contoso.com**是为你自动填写。  
  
5.  在**完整域名 \(FQDN\) 目标主机**，键入你的 Web 服务器 FQDN。 例如，如果 WEB1 名为你的 Web 服务器，并且你域 corp.contoso.com，键入**WEB1.corp.contoso.com**。  
  
6.  单击**确定**向区域中添加新的记录。  
  


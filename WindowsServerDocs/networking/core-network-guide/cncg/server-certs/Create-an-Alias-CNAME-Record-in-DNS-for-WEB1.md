---
title: 为 WEB1 在 DNS 中创建别名 (CNAME) 记录
description: 本主题是指导 802.1 X 有线和无线部署的 "部署服务器证书" 的一部分
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 77b8e464d2d8fab8803477e59826c3e715c0a6d2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406294"
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>在 DNS 中为 WEB1 创建别名 @no__t 0CNAME @ no__t 记录

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用此过程将 Web 服务器的别名规范名称 \(CNAME @ no__t 资源记录添加到域控制器上 DNS 中的某个区域。 对于 CNAME 记录，你可以使用多个名称来指向单个主机，这样就可以轻松地将文件传输协议 \(FTP @ no__t 服务器和同一台计算机上的 Web 服务器作为宿主。   
  
因此，你可以随时使用你的 Web 服务器来承载证书吊销列表，\(CRL @ no__t-1 用于证书颁发机构 \(CA @ no__t，以及用于执行其他服务，如 FTP 或 Web 服务器。  
  
执行此过程时，请将**Alias 名称**和其他变量替换为适用于你的部署的值。  
  
若要执行此过程，您必须是**Domain Admins**的成员。  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>将别名 \(CNAME @ no__t 资源记录添加到区域  
  
>[!NOTE]  
>若要使用 Windows PowerShell 执行此过程，请参阅[DnsServerResourceRecordCName](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx)。  
  
1.  在 DC1 上的服务器管理器中，单击 "**工具**"，然后单击 " **DNS**"。 此时将打开 DNS 管理器 Microsoft 管理控制台（MMC）。  
  
2.  在控制台树中，双击 "**正向查找区域**"，右键单击要在其中添加别名资源记录的正向查找区域，然后单击 "**新建别名 \(CNAME @ no__t-3**"。 此时将打开 "**新建资源记录**" 对话框。  
  
3.  在 "**别名**" 中，键入别名 **。**  
  
4.  为 "**别名**" 键入值时，该对话框中的**完全限定域名 \(FQDN @ no__t**自动填充。 例如，如果你的别名为 "pki"，而你的域为 corp.contoso.com，则会自动为你填充值**pki.corp.contoso.com** 。  
  
5.  在 "**完全限定的域名 \(FQDN @ no__t-2 对于目标主机**" 中，键入 Web 服务器的 FQDN。 例如，如果你的 Web 服务器名为 WEB1，并且你的域为 corp.contoso.com，则键入**WEB1.corp.contoso.com**。  
  
6.  单击 **"确定"** 将新记录添加到区域。  
  


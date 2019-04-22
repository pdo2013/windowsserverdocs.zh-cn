---
title: 为 WEB1 在 DNS 中创建别名 (CNAME) 记录
description: 本主题是指南为 802.1x 有线和无线部署部署服务器证书的一部分
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 442ee8b70311eaad3f0b3f263003786b6beab8bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813158"
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>创建一个别名\(CNAME\)记录在 DNS 中为 WEB1

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用此过程以添加别名规范名称\(CNAME\) Web 服务器放到 DNS 中域控制器上的区域的资源记录。 与 CNAME 记录，可以使用多个名称，以指向一台主机，从而轻松实现以下功能： 为这两个主机文件传输协议\(FTP\)服务器和在同一台计算机上的 Web 服务器。   
  
因此，你可以随意使用你的 Web 服务器托管证书吊销列表\(CRL\)的证书颁发机构\(CA\)并执行其他服务，例如 FTP 或 Web 服务器。  
  
当你执行此过程时，将为**别名**和适用于你的部署的值与其他变量。  
  
若要执行此过程，您必须**Domain Admins**。  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>添加别名\(CNAME\)资源记录到区域  
  
>[!NOTE]  
>若要使用 Windows PowerShell 执行此过程，请参阅[添加 DnsServerResourceRecordCName](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx)。  
  
1.  在 DC1 上，在服务器管理器中，单击**工具**，然后单击**DNS**。 此时将打开 DNS 管理器 Microsoft 管理控制台 (MMC)。  
  
2.  在控制台树中，双击**正向查找区域**，右键单击你想要添加别名资源记录，并单击正向查找区域**新别名\(CNAME\)** . **新建资源记录**对话框随即打开。  
  
3.  在中**别名**，键入别名名称**pki**。  
  
4.  类型的值时**别名**，则**完全限定的域名\(FQDN\)** 自动填充在对话框中。 例如，如果别名名称为"pki"和你的域为 corp.contoso.com，则这**pki.corp.contoso.com**是为你自动填充。  
  
5.  在中**完全限定的域名\(FQDN\)目标主机的**，键入你的 Web 服务器的 FQDN。 例如，如果你的 Web 服务器为 WEB1，并且你的域为 corp.contoso.com，则键入**WEB1.corp.contoso.com**。  
  
6.  单击**确定**将新记录添加到该区域。  
  


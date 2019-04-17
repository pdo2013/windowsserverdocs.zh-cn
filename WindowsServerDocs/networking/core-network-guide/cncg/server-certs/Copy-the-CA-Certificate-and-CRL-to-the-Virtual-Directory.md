---
title: 将 CA 证书和 CRL 复制到虚拟目录
description: 本主题介绍指南部署服务器证书 802.1 X 有线和无线部署部分
manager: brianlic
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e37bfce7f8cf33fd7fcb5e6227d783c28bd29d35
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>将 CA 证书和 CRL 复制到虚拟目录

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程，以将你证书颁发机构证书吊销列表和企业版根复制到 Web 服务器，在虚拟目录，并确保正确配置广告客户服务。 之前运行以下命令，请确保你使用这些适用于你的部署替换目录和服务器名称。  
  
若要执行此过程你必须**域管理员**。  
  
#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>将证书吊销列表从 CA1 复制到 WEB1  
  
1.  CA1，以 administrator 身份，运行 Windows PowerShell，然后发布 CRL 使用以下命令：  
  
    - 键入`certutil -crl`，然后按 ENTER。  
  
    - 若要复制到你的 Web 服务器上的文件共享 CA 证书，请键入`copy C:\Windows\system32\certsrv\certenroll\*.crt \\WEB1\pki`，然后按 ENTER。  
    - 若要复制到你的 Web 服务器上的文件共享证书吊销列表，请键入`copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki`，然后按 ENTER。  
  
2. 若要重新启动广告客户服务，请键入`Restart-Service certsvc`，然后按 ENTER。  
  
2.  若要验证是否正确配置你 CDP 和 AIA 扩展位置，请键入`pkiview.msc`，然后按 ENTER。 Pkiview 企业 PKI MMC 将打开。  
  
3.  单击 CA 名称。 例如，如果你 CA 名称 corp CA1 CA，请单击**corp CA1 CA**。 在详细信息窗格中，验证**状态**值**CA 证书**， **AIA 位置 #1**，并**CDP 位置 #1**全部**确定**。  
  
下图显示了 pkiview 结果窗格中的所有项目状态为确定。  
  
![adcs_pkiviewmedia/adcs_pkiview.png)  
  
> [!IMPORTANT]  
> 如果**状态**任何项目是不**确定**，请执行以下操作：  
> -   打开你的 Web 服务器来验证的证书和证书吊销列表中的文件上共享已成功复制到共享。 如果它们未成功复制到该共享，修改正确的文件源与你复制的命令和共享的目标，然后再次运行命令。  
> -   验证你有任何额外空格或您提供的位置中的其他字符，请确保 CA 扩展选项卡上输入 CDP 和 AIA 正确位置。  
> -   验证复制到正确位置 CRL 和加拿大证书以在你的 Web 服务器，并位置匹配 CDP 和 AIA 位置 CA 上提供的位置。  
> -   验证您正确配置虚拟文件夹 CA 证书和 CRL 存储位置的权限。  
  



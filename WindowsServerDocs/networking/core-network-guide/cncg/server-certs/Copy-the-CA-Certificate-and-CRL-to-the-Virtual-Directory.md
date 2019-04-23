---
title: 将 CA 证书和 CRL 复制到虚拟目录
description: 本主题是指南为 802.1x 有线和无线部署部署服务器证书的一部分
manager: dougkim
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.date: 07/19/2018
ms.openlocfilehash: 15cc807db805e1be0349ea51119663e515c7e551
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874028"
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>将 CA 证书和 CRL 复制到虚拟目录

>适用于：Windows 服务器 （半年频道），Windows Server 2016

若要将从证书颁发机构的证书吊销列表和企业根 CA 证书复制到你的 Web 服务器上的虚拟目录并确保正确配置了 AD CS，你可以使用此过程。 之前运行以下命令，请确保替换目录和服务器名称为与适用于你的部署。  
  
若要执行此过程，必须**Domain Admins**。  
  
#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>若要将证书吊销列表从 CA1 复制到 WEB1  
  
1.  CA1，Windows PowerShell 以管理员身份，并运行然后发布 CRL 使用以下命令：  
  
    - 键入 `certutil -crl`，然后按 Enter。  

    - 若要将证书吊销列表复制到你的 Web 服务器上的文件共享中，键入`copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki`，然后按 ENTER。  
  
2.  若要验证是否正确配置了你 CDP 和 AIA 扩展的位置，请键入`pkiview.msc`，然后按 ENTER。 Pkiview 企业 PKI MMC 会打开。  
  
3.  在左窗格中，单击 CA 名称。<p>例如，如果你的 CA 名称 corp CA1 CA，请单击**corp CA1 CA**。 

4. 在结果窗格的状态列中，验证以下设置的值，显示**确定**:

    - **CA 证书**
    - **AIA 位置 #1**
    - **#1 的 CDP 位置**   
  
  
> [!TIP]  
> 如果**状态**任何项不是**确定**，执行以下操作：  
> -   打开您要验证的证书和证书吊销列表文件的 Web 服务器上的共享已成功复制到共享。 如果它们未成功复制到共享，修改您的复制命令与正确的文件源和共享目标，并再次运行命令。  
> -   验证的 CA 扩展选项卡上输入了正确的 CDP 和 AIA 位置。请确保有没有多余的空格或已提供的位置中的其他字符。  
> -   验证复制到正确的位置的 CRL 和 CA 证书以在你的 Web 服务器上且位置匹配 CDP 和 AIA 位置在 CA 上提供的位置。  
> -   验证正确配置的 CA 证书和 CRL 的存储位置的虚拟文件夹的权限。  
  



---
title: 将 CA 证书和 CRL 复制到虚拟目录
description: 本主题是指导 802.1 X 有线和无线部署的 "部署服务器证书" 的一部分
manager: dougkim
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.date: 07/19/2018
ms.openlocfilehash: 9dbe14bec1c39ab5b967276c4faf3e9fc5a9aae3
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546533"
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>将 CA 证书和 CRL 复制到虚拟目录

>适用于：Windows Server（半年频道）、Windows Server 2016

可以使用此过程将证书吊销列表和企业根 CA 证书从证书颁发机构复制到 Web 服务器上的虚拟目录, 并确保正确配置 AD CS。 在运行下面的命令之前, 请确保将目录和服务器名称替换为适用于你的部署的名称。  
  
若要执行此过程, 您必须是**Domain Admins**的成员。  
  
#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>将证书吊销列表从 CA1 复制到 WEB1  
  
1.  在 CA1 上, 以管理员身份运行 Windows PowerShell, 然后使用以下命令发布 CRL:  
  
    - 键入 `certutil -crl`，然后按 Enter。  

    - 若要将 CA1 证书复制到 Web 服务器上的文件共享, 请`copy C:\Windows\system32\certsrv\certenroll\*.crt \\WEB1\pki`键入, 然后按 enter。  
    
    - 若要将证书吊销列表复制到 Web 服务器上的文件共享中, `copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki`请键入, 然后按 enter。  
  
2.  若要验证是否正确配置了 CDP 和 AIA 扩展位置, 请`pkiview.msc`键入, 然后按 enter。 Pkiview 企业 PKI MMC 随即打开。  
  
3.  在左侧窗格中, 单击你的 CA 名称。<p>例如, 如果 CA 名称是 CA1-CA, 请单击 " **CA1-ca**"。 

4. 在结果窗格的 "状态" 列中, 验证以下值是否显示 **"确定"** :

    - **CA 证书**
    - **AIA 位置 #1**
    - **CDP 位置 #1**   
  
  
> [!TIP]  
> 如果任何项目的**状态**不是 **"正常"** , 请执行以下操作:  
> -   打开 Web 服务器上的共享, 验证是否已成功将证书和证书吊销列表文件复制到共享。 如果未成功将它们复制到共享, 请用正确的文件源和共享目标修改复制命令, 然后再次运行这些命令。  
> -   验证是否已在 "CA 扩展" 选项卡上为 CDP 和 AIA 输入正确的位置。确保你提供的位置中没有多余的空格或其他字符。  
> -   验证是否已将 CRL 和 CA 证书复制到 Web 服务器上的正确位置, 以及该位置是否与你为 CA 上的 CDP 和 AIA 位置提供的位置匹配。  
> -   验证是否为存储 CA 证书和 CRL 的虚拟文件夹正确配置了权限。  
  



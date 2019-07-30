---
title: 远程桌面客户端断开连接且无法重新连接到同一会话
description: 排查远程桌面客户端断开连接且无法重新连接到同一会话的问题。
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: ''
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 007668d1c0f8f2a6701813385b0e0bb7a09b29a0
ms.sourcegitcommit: f6503e503d8f08ba8000db9c5eda890551d4db37
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68529947"
---
# <a name="remote-desktop-client-disconnects-and-cant-reconnect-to-the-same-session"></a>远程桌面客户端断开连接且无法重新连接到同一会话

在远程桌面客户端从远程桌面断开连接后，该客户端无法立即重新连接。 用户收到下述错误消息之一：

  - 由于出现安全错误，客户端无法连接到终端服务器。 请确保登录到网络，然后重新尝试进行连接。
  - 远程桌面已断开连接。 由于出现安全错误，客户端无法连接到远程计算机。 请验证是否已登录到网络，然后重试连接。

当远程桌面客户端重新连接时，RDSH 服务器会将客户端重新连接到新会话而非原始会话。 但是，当你检查 RDSH 服务器时，会发现原始会话仍处于活动状态，并未进入断开连接状态。

若要解决此问题，可以在“计算机配置”\\“管理模板”\\“Windows 组件”\\“远程桌面服务”\\“远程桌面会话主机”\\“连接”组策略文件夹中启用“配置 keep-alive 连接间隔”策略。   如果启用此策略，则必须输入 keep-alive 间隔。 keep-alive 间隔确定服务器检查会话状态的频率（以分钟为单位）。

也可通过重新配置身份验证和配置设置解决此问题。 可以在服务器级别重新配置这些设置，也可以使用组策略对象 (GPO) 来这样做。 以下是重新配置设置的方法：“计算机配置”\\“管理模板”\\“Windows 组件”\\“远程桌面服务”\\“远程桌面会话主机”\\“安全性”组策略文件夹。 

1. 在 RD 会话主机服务器上，打开“远程桌面会话主机配置”。 
2. 在“连接”下右键单击连接的名称，然后选择“属性”。  
3. 在该连接的“属性”对话框中的“常规”选项卡上，在“安全”层中选择一种安全方法。   
4. 转到“加密级别”，选择所需级别。  可以选择“低”、“客户端兼容”、“高”或“符合 FIPS”。    

> [!NOTE]  
>  - 如果客户端与 RD 会话主机服务器之间的通信需要最高级别的加密，请使用符合 FIPS 的加密。
>  - 在“组策略”中配置的任何加密级别设置会替代使用远程桌面服务配置工具配置的设置。 此外，如果启用[系统加密:使用符合 FIPS 的算法进行加密、哈希处理和签名](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/system-cryptography-use-fips-compliant-algorithms-for-encryption-hashing-and-signing)策略，则此设置会替代“设置客户端连接加密级别”策略。  系统加密策略位于“计算机配置”\\“Windows 设置”\\“安全设置”\\“本地策略”\\“安全选项”文件夹中。 
>  - 更改加密级别后，新的加密级别将在用户下一次登录时生效。 如果需要在一台服务器上使用多个加密级别，请安装多个网络适配器并单独配置每个适配器。
>  - 若要验证证书是否有相应的私钥，请转到“远程桌面服务配置”，右键单击要查看其证书的连接，选择“常规”，然后选择“编辑”。   然后，选择“查看证书”。  转到“常规”选项卡时，如果有密钥，则会看到“你已有对应于此证书的私钥”语句。  也可以使用“证书”管理单元查看此信息。
>  - “符合 FIPS”加密（远程桌面服务器配置中的“系统加密:  使用符合 FIPS 的算法进行加密、哈希处理和签名”策略或“符合 FIPS”设置）将通过那些使用 Microsoft 加密模块的联邦信息处理标准 (FIPS) 140-1 加密算法，对在服务器和客户端之间发送的数据进行加密和解密。  有关详细信息，请参阅 [FIPS 140 验证](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation)。
>  - “高”设置使用强 128 位加密对在服务器和客户端之间发送的数据进行加密。 
>  - “客户端兼容”设置以客户端支持的最大密钥强度加密客户端与服务器之间发送的数据。 
>  - “低”设置使用 56 位加密算法对从客户端发送到服务器的数据进行加密。 

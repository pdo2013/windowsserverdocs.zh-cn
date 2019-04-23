---
title: 验证服务器证书的服务器注册
description: 本主题是指南为 802.1x 有线和无线部署部署服务器证书的一部分
manager: brianlic
ms.topic: article
ms.assetid: bd80a018-5a30-47c3-89fc-aacb9f5ad298
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 45ba7a9a7fc5b9622ab1b9a94f38f4bf4de13192
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850898"
---
# <a name="verify-server-enrollment-of-a-server-certificate"></a>验证服务器证书的服务器注册

>适用于：Windows 服务器 （半年频道），Windows Server 2016

此过程可用于验证你的网络策略服务器 (NPS) 服务器已注册服务器证书从证书颁发机构 (CA)。   
  
>[!NOTE]  
>中的成员身份**Domain Admins**组是完成这些过程所需的最低要求。  
  
## <a name="verify-network-policy-server-nps-enrollment-of-a-server-certificate"></a>验证服务器证书的网络策略服务器 (NPS) 注册  
  
由于 NPS 用于身份验证和授权的网络连接请求，务必确保服务器证书已颁发给 NPSs 在网络策略中使用时有效。  
  
若要验证服务器证书已正确配置和注册到 NPS，必须配置测试网络策略，并允许 NPS 验证 NPS 可以使用进行身份验证的证书。  
  
### <a name="to-verify-nps-enrollment-of-a-server-certificate"></a>若要验证 NPS 服务器证书的注册  
  
1.  在服务器管理器中，单击**工具**，然后单击**网络策略服务器**。 此时将打开网络策略服务器 Microsoft 管理控制台 (MMC)。  
  
2.  双击**策略**，右键单击**网络策略**，然后单击**新建**。 此时将打开新建网络策略向导。  
  
3.  在中**指定网络策略名称和连接类型**，在**策略名称**，类型**测试策略**。 絋粄**类型的网络访问服务器**具有值**未指定**，然后单击**下一步**。  
  
4.  在中**指定条件**，单击**添加**。 在中**选择条件**，单击**Windows 组**，然后单击**添加**。  
  
5.  在中**组**，单击**添加组**。 在中**选择组**，类型**域用户**，然后按 ENTER。 单击 **“确定”**，然后单击 **“下一步”**。  
  
6.  在中**指定访问权限**，确保**授予访问权限**已选择，然后单击**下一步**。  
  
7.  在中**配置身份验证方法**，单击**添加**。 在中**添加 EAP**，单击**Microsoft:受保护的 EAP (PEAP)**，然后单击**确定**。 在中**EAP 类型**，选择**Microsoft:受保护的 EAP (PEAP)**，然后单击**编辑**。 **编辑受保护的 EAP 属性**对话框随即打开。  
  
8.  在中**编辑受保护的 EAP 属性**对话框中**颁发给证书**，NPS 的格式显示你的服务器证书的名称*ComputerName*。*域*。 例如，如果你 NPS 为 nps-01，且你的域的 example.com，NPS 将显示证书**NPS 01.example.com**。 此外，在**颁发者**，显示的证书颁发机构的名称，然后在**到期日期**，显示服务器证书的到期日期。 此示例演示在 NPS 已注册有效的服务器证书，它可用于向尝试通过网络访问服务器，如虚拟专用网络 (VPN) 服务器、 802.1 访问网络的客户端计算机证明其身份 X 支持无线访问点、 远程桌面网关服务器和 802.1 X 的以太网交换机。  
  
    > [!IMPORTANT]  
    > 如果 NPS 不会显示有效的服务器证书，并且它提供本地计算机找不到这样的证书的消息，如果有此问题的两个可能的原因。 很可能未正确刷新组策略和 NPS 尚未注册 ca 颁发的证书。 在这种情况下，重新启动 NPS。 在计算机重新启动后，刷新组策略，并可以执行此过程以验证已注册的服务器证书。 如果刷新组策略不能解决此问题，证书模板、 证书自动注册，或两者都未正确配置。 若要解决这些问题，本指南的开头开始并执行所有步骤，以确保您已提供的设置正确无误。  
  
9. 如果验证有效的服务器证书是否存在后，你可以单击**确定**并**取消**退出新建网络策略向导。  
  
    > [!NOTE]  
    > 因为未完成该向导，在 NPS 中不创建测试网络策略。  
  



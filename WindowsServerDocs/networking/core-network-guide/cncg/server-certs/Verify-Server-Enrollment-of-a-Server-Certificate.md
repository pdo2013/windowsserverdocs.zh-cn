---
title: 验证服务器证书服务器的注册
description: 本主题介绍指南部署服务器证书 802.1 X 有线和无线部署部分
manager: brianlic
ms.topic: article
ms.assetid: bd80a018-5a30-47c3-89fc-aacb9f5ad298
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d8ff51fa83972e2fc73ee54628eeb89e2927046d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="verify-server-enrollment-of-a-server-certificate"></a>验证服务器证书服务器的注册

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程来验证你的网络策略 Server (NPS) 服务器登记了服务器从认证颁发机构证书。   
  
>[!NOTE]  
>在会员**域管理员**组是的最低要求才能完成这些步骤。  
  
## <a name="verify-network-policy-server-nps-enrollment-of-a-server-certificate"></a>验证服务器证书网络策略 Server (NPS) 的注册  
  
因为 NPS 用于验证和授权的网络连接请求，请务必确保你已颁发给 NPS 服务器服务器证书网络策略在使用时有效。  
  
若要验证的服务器证书正确配置和到 NPS 服务器注册，必须配置测试网络策略，并允许 NPS 验证 NPS 可以使用进行身份验证的证书。  
  
### <a name="to-verify-nps-server-enrollment-of-a-server-certificate"></a>若要验证的服务器证书 NPS 服务器注册  
  
1.  在服务器管理器中，单击**工具**，然后单击**网络策略服务器**。 打开网络策略服务器 Microsoft 管理控制台 (MMC)。  
  
2.  双击**策略**，右键单击**网络策略**，然后单击**新建**。 打开新的网络策略的向导。  
  
3.  在**指定网络策略的名称和连接类型**中**策略名称**，类型**测试策略**。 确保**种类型的网络访问权限服务器**具有值**未指定**，然后单击**下一步**。  
  
4.  在**指定条件**，单击**添加**。 在**选择条件**，单击**Windows 组**，然后单击**添加**。  
  
5.  在**组**，单击**添加组**。 在**选择组**，类型**域用户**，然后按 ENTER。 单击**确定**，然后单击**下一步**。  
  
6.  在**指定的访问权限**，确保**授予访问权限**选中，则，然后单击**下一步**。  
  
7.  在**配置身份验证方法**，单击**添加**。 在**添加 EAP**，单击**Microsoft： 保护 EAP (PEAP)**，然后单击**确定**。 在**EAP 类型**、 选择**Microsoft： 保护 EAP (PEAP)**，然后单击**编辑**。 **编辑保护 EAP 属性**对话框中打开。  
  
8.  在**编辑保护 EAP 属性**对话框中，在**证书颁发给**、 NPS 显示服务器证书的名称格式*ComputerName*。*域*。 例如，如果 NPS-01 名为 NPS 服务器，并且你域 example.com、NPS 显示证书**NPS-01.example.com**。此外，在**发行商**，未经认证颁发的名称将显示，并在**到期日期**，会显示服务器证书的到期日期。 这也证明 NPS 服务器已注册，它可使用证明到尝试通过你的网络访问权限服务器，如虚拟专用网络 (VPN) 服务器、 802.1 访问该网络的客户端计算机其身份一个服务器是否有效证书 X 支持的无线接入点、 远程桌面网关服务器和 802.1 X 的以太网切换。  
  
    > [!IMPORTANT]  
    > 如果 NPS 不会显示服务器有效证书，并且它将提供在本地计算机找不到这样的证书的消息，有两个可能的原因，此问题。 很可能不正确，刷新组策略，所做的那样，并 NPS 服务器未注册证书 ca。 在此情况下，重启 NPS 服务器。 计算机重新启动后，刷新组策略，并且你可以执行此过程，以验证的服务器证书已注册。 如果恢复组策略不能解决此问题，证书模板、 证书自动注册或两者都不正确配置。 若要解决这些问题，请本指南从头开始，并执行所有步骤再次确保您所提供的设置的准确性。  
  
9. 当你已验证的服务器是否有效证书存在时，你可以单击**确定**和**取消**退出新的网络策略的向导。  
  
    > [!NOTE]  
    > 你不完成向导，因为不会在 NPS 创建测试网络策略。  
  



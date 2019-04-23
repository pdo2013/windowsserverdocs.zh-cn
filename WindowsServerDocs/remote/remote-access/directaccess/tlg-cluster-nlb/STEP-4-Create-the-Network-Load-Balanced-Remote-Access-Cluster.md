---
title: 步骤 4 创建网络负载平衡的远程访问群集
description: 本主题是一部分的测试实验室指南-使用 Windows Server 2016 的 Windows NLB 的群集中演示 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 509eaa08-c49d-448d-a71e-c1c45519ccd5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 11f4fe7b68f69b00ec0f8fb9764e0fb4460a2851
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845948"
---
# <a name="step-4-create-the-network-load-balanced-remote-access-cluster"></a>步骤 4 创建网络负载平衡的远程访问群集

>适用于：Windows 服务器 （半年频道），Windows Server 2016

 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012，可以创建群集的远程访问服务器。 群集可用作单一逻辑服务器，并可集中的配置和管理群集中的服务器。 使用网络负载平衡 (NLB) 时对单个群集中最多 8 个远程访问成员的支持。 远程访问群集提供高可用性和负载平衡的 DirectAccess 客户端连接到内部网络。  
  
以下过程，您可以创建和测试的远程访问群集：  
  
1. 在 EDGE1 和 EDGE2 上安装网络负载平衡功能。 启用负载平衡之前, 必须 EDGE1 和 EDGE2 上安装网络负载平衡功能。
  
2. 启用 EDGE1 上进行负载均衡。 最初在单一服务器模式下安装 EDGE1。 若要启用负载平衡，请为 EDGE1 配置新外部和内部专用的 IP 地址 (Dip)。 对上一个 Dip EDGE1 上的自动配置为群集的虚拟 IP 地址 (Vip)。 新的外部 DIP 是 131.107.0.10、 新的内部 IPv4 DIP 是 10.0.0.10，新的内部 IPv6 DIP 是 2001:db8:1::10。 群集 Vip 是 131.107.0.2 和 131.107.0.3 （外部） 和 10.0.0.2 和并使用 2001:db8:1::2 （内部）。
  
3. 向负载平衡群集中添加 EDGE2。 启用负载平衡之后, 可以现在添加到群集以提供负载平衡和高可用性的 DirectAccess 客户端连接 EDGE2。

## <a name="prerequisites"></a>先决条件

如果要在虚拟机上创建此测试实验室，则必须启用 MAC 地址欺骗 EDGE1 和 EDGE2。  
  
### <a name="enable-mac-address-spoofing-on-edge1-and-edge2"></a>启用 MAC 地址欺骗 EDGE1 和 EDGE2  
  
1.  对 EDGE1 和 EDGE2 执行正常关闭。  
  
2.  在托管你的虚拟机的计算机上**Hyper-v 管理器**，右键单击 EDGE1，，然后单击**设置**。  
  
3.  上**设置**对话框中**硬件**列表中，单击连接到公司网络网络的网络适配器，然后在详细信息窗格中，选择**启用 MAC 地址欺骗**复选框。  
  
4.  在中**硬件**列表中，单击连接到 Internet 网络的网络适配器，然后在详细信息窗格中，选择**启用 MAC 地址欺骗**复选框。  
  
5.  上**设置**对话框中，单击**确定**。  
  
6.  重复此过程从步骤 2 开始 EDGE2。  
  
## <a name="install-the-network-load-balancing-feature-on-edge1-and-edge2"></a>EDGE1 和 EDGE2 上安装网络负载平衡功能  
若要在群集中配置 EDGE1 和 EDGE2，必须在 EDGE1 和 EDGE2 上安装网络负载平衡功能。  
  
### <a name="to-install-network-load-balancing"></a>若要安装网络负载平衡  
  
1.  在 EDGE1，在服务器管理器控制台中，在**仪表板**，单击**添加角色和功能**。  
  
2.  单击**下一步**四次以转到服务器的功能选择屏幕上。  
  
3.  上**选择的功能**对话框中，选择**网络负载平衡**，单击**添加功能**，单击**下一步**，然后单击**安装**。  
  
4.  在“安装进度”对话框中，验证安装是否成功，然后单击“关闭”。  
  
5.  重复此过程在 EDGE2。  
  
## <a name="enable-load-balancing-on-edge1"></a>EDGE1 上启用负载平衡  
使用此过程来启用负载平衡和 EDGE1 上配置新的 Dip。  
  
### <a name="enable-load-balancing"></a>启用负载平衡  
  
1.  在 EDGE1，单击**启动**，类型**RAMgmtUI.exe**，然后按 ENTER。 如果出现了“用户帐户控制”对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”。  
  
2.  在远程访问管理控制台中，在左窗格中，单击**配置**，然后在**任务**窗格中，单击**启用负载平衡**。  
  
3.  启用负载平衡向导中，单击**下一步**。  
  
4.  上**负载平衡方法**页上，单击**使用 Windows 网络负载平衡 (NLB)**，然后单击**下一步**。  
  
5.  上**外部的专用 IP 地址**页上，在**IPv4 地址**框中，键入**131.107.0.10**，在**子网掩码**框中，验证子网前缀**255.255.255.0**，然后单击**下一步**。  
  
6.  上**内部专用 IP 地址**页上，执行以下操作，并单击**下一步**:  
  
    1.  在中**IPv4 地址**框中，键入**10.0.0.10**并在**子网掩码**框中，验证子网前缀**255.255.255.0**。  
  
    2.  在中**IPv6 地址**框中，键入**2001:db8:1::10**并在子网前缀长度验证的值是**64**。  
  
7.  上**摘要**页上，单击**提交**。  
  
8.  上**启用负载平衡**对话框中，单击**关闭**。  
  
9. 启用负载平衡向导中，单击**关闭**。  
  
## <a name="add-edge2-to-the-load-balanced-cluster"></a>将 EDGE2 添加到负载平衡群集  
使用此过程将 EDGE2 添加到 NLB 群集。  
  
> [!NOTE]  
> 你应等待两分钟后完成前面的步骤再继续。 启用 NLB 后, RAConfigTask 运行，并使用 NLB 设置配置计算机。 这可能需要几分钟才能完成，并且当管理员运行另一个 NLB 相关的配置任务结束之前，该配置将失败。  
  
### <a name="add-edge2-to-the-cluster"></a>向群集添加 EDGE2  
  
1.  在 EDGE1 计算机或虚拟机，在远程访问管理控制台中，在**任务**窗格下**负载平衡的群集**，单击**添加或删除服务器**。  
  
2.  上**添加或删除服务器**对话框中，单击**添加服务器**。  
  
3.  上**将服务器添加**向导**选择服务器**页上，键入**EDGE2**，然后单击**下一步**。  
  
4.  上**网络适配器**页上，在**外部适配器**，请确保**Internet**已选中，然后在**内部适配器**，请确保该**Corpnet**处于选中状态。 单击**浏览**，然后在**Windows 安全**对话框框中，请确保**的 IP-HTTPS 证书**为选中状态，单击**确定**，然后单击**下一步**。  
  
5.  上**摘要**页上，单击**添加**。  
  
6.  在**完成**页上，单击**关闭**。  
  
7.  上**添加或删除服务器**对话框中，单击**提交**。  
  
8.  上**添加和删除服务器**对话框中，单击**关闭**。  
  
9. 上**启动**屏幕上，键入**nlbmgr.exe**，然后按 ENTER。 如果出现了“用户帐户控制”对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”。  
  
10. 在中**网络负载平衡管理器**，单击**内部 DA 群集**。 在细节窗格中，请确保同时**EDGE1(Corpnet)** 并**EDGE2(Corpnet)** 具有状态**聚合**。  
  
11. 如果服务器不是**汇聚**，在控制台树中，右键单击服务器，依次指向**控制主机**，然后单击**启动**。  
  
12. 在中**网络负载平衡管理器**，单击**Internet DA 群集**。 请确保在详细信息窗格中，同时**EDGE1(Internet)** 并**EDGE2(Internet)** 具有状态**聚合**。  
  
13. 如果服务器不是**汇聚**，在控制台树中，右键单击服务器，依次指向**控制主机**，然后单击**启动**。

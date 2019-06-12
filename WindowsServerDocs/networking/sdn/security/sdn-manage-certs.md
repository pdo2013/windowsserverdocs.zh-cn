---
title: 软件定义的网络管理证书
description: 可以使用本主题，了解如何部署软件定义网络 (SDN) 中 Windows Server 2016 Datacenter 时为网络控制器 Northbound 和 Southbound 沟通管理证书。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: d29a98e24b475c38fee61972bf9efbd5a2528974
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446259"
---
# <a name="manage-certificates-for-software-defined-networking"></a>软件定义的网络管理证书

>适用于：Windows 服务器 （半年频道），Windows Server 2016

你可以使用本主题，了解如何管理部署软件定义的网络时的网络控制器 Northbound 和 Southbound 通信的证书\(SDN\)在 Windows Server 2016 Datacenter 中使用的系统Center Virtual Machine Manager \(SCVMM\)作为 SDN 管理客户端。

>[!NOTE]
>有关网络控制器概述信息，请参阅[网络控制器](../technologies/network-controller/Network-Controller.md)。

如果不使用 Kerberos 保护网络控制器通信，可以使用身份验证、 授权和加密的 X.509 证书。

Windows Server 2016 Datacenter 中的 SDN 支持这两种自助\-签名和证书颁发机构\(CA\)-签名的 X.509 证书。 本主题提供有关创建这些证书，并将其应用的分步说明，以保护与管理客户端的网络控制器 Northbound 通信通道的安全和 Southbound 通信了与网络设备，如软件负载均衡器\(SLB\)。
.
当你使用的证书\-基于身份验证，必须注册网络控制器节点上的以下方法中使用的一个证书。

1. 加密南向通信使用安全套接字层\(SSL\)网络控制器节点管理客户端，如 System Center Virtual Machine Manager 之间。
2. 网络控制器节点和 Southbound 设备和服务，例如 HYPER-V 主机和软件负载均衡器之间的身份验证\(SLBs\)。

## <a name="creating-and-enrolling-an-x509-certificate"></a>创建和注册的 X.509 证书

可以创建和注册自\-签名证书或由 CA 颁发的证书。

>[!NOTE]
>当使用 SCVMM 部署网络控制器时，必须指定用于在网络控制器服务模板配置的过程中加密 Northbound 通信的 X.509 证书。

证书配置必须包含以下值。

- 值**RestEndPoint**文本框中必须是网络控制器完全限定的域名\(FQDN\)或 IP 地址。 
- **RestEndPoint**值必须与使用者名称匹配\(公用名，CN\)的 X.509 证书。

### <a name="creating-a-self-signed-x509-certificate"></a>创建自\-签名的 X.509 证书

可以创建自签名的 X.509 证书并使用私钥将其导出\(使用密码来保护\)按照以下步骤为单\-节点和多个\-节点部署的网络控制器.

当创建自我\-签名证书，可以使用以下指导原则。

- 可用于 DnsName 参数中的网络控制器 REST 终结点的 IP 地址，但不是建议，因为它需要的网络控制器节点是单一管理子网内全部位于\(例如在单个机架上\)
- 对于多个节点 NC 部署，你指定的 DNS 名称将成为网络控制器群集的 FQDN \(DNS 主机 A 记录会自动创建。\) 
- 对于单节点网络控制器部署，DNS 名称可以是网络控制器主机名称后跟完整域名。

#### <a name="multiple-node"></a>多个节点

可以使用[New-selfsignedcertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) Windows PowerShell 命令来创建自\-签名的证书。

**语法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**示例用法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>单个节点

可以使用[New-selfsignedcertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) Windows PowerShell 命令来创建自\-签名的证书。

**语法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**示例用法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>创建 CA\-签名的 X.509 证书

若要使用的是 CA 创建证书，你必须已部署公钥基础结构\(PKI\)与 Active Directory 证书服务\(AD CS\)。 

>[!NOTE]
>可以使用第三方 Ca 或工具，如 openssl，若要使用网络控制器创建使用的证书，但是本主题中的说明进行操作的特定于 AD CS。 若要了解如何使用第三方 CA 或工具，请参阅所使用的软件的文档。

向 CA 创建证书包括下列步骤。

1. 你或贵组织的域或安全管理员配置的证书模板
2. 你或贵组织的网络控制器管理员或 SCVMM 管理员从 CA 申请新证书。

#### <a name="certificate-configuration-requirements"></a>证书配置要求

在下一步中配置证书模板，请确保你配置的模板包括所需的以下元素。

1. 证书使用者名称必须是 HYPER-V 主机的 FQDN
2. 必须将证书放置在本地计算机个人存储区 (我 – cert: \localmachine\my)
3. 该证书必须具有这两种服务器身份验证 (EKU:1.3.6.1.5.5.7.3.1) 和客户端身份验证 (EKU:1.3.6.1.5.5.7.3.2) 应用程序策略。

>[!NOTE]
>如果个人\(我 – cert: \localmachine\my\)证书存储区上超\-V 主机有多个 X.509 证书的使用者名称 (CN) 为主机完全限定域名\(FQDN\)，请确保将由 SDN 的证书存在 OID 1.3.6.1.4.1.311.95.1.1.1 具有自定义的其他增强型密钥用法属性。 否则，网络控制器和主机之间的通信可能无法工作。

#### <a name="to-configure-the-certificate-template"></a>若要配置证书模板
  
>[!NOTE]
>在执行此过程之前，应查看证书要求和证书模板控制台中的可用证书模板。 您可以修改现有模板，或创建现有模板的副本并可以修改模板的副本。 建议创建现有模板的副本。

1. 在其中安装 AD CS，服务器管理器中的服务器上单击**工具**，然后单击**证书颁发机构**。 证书颁发机构 Microsoft 管理控制台\(MMC\)随即打开。 
2. 在 MMC 中，双击 CA 名称，右键单击**证书模板**，然后单击**管理**。
3. 将打开证书模板控制台。 在详细信息窗格中将显示所有的证书模板。
4. 在详细信息窗格中，单击你想要复制的模板。
5.  单击**操作**菜单，并单击**复制模板**。 该模板**属性**对话框随即打开。
6.  在模板中**属性**对话框中，在**使用者名称**选项卡上，单击**在请求中提供**。 \(此设置是必需的网络控制器 SSL 证书。\)
7.  在模板中**属性**对话框中，在**请求处理**选项卡上，确保**允许导出私钥**处于选中状态。 此外，请确保**签名和加密**选择目的。
8.  在模板中**属性**对话框中，在**扩展**选项卡上，选择**密钥用法**，然后单击**编辑**。
9.  在中**签名**，确保**数字签名**处于选中状态。
10.  在模板中**属性**对话框中，在**扩展**选项卡上，选择**应用程序策略**，然后单击**编辑**。
11.  在中**应用程序策略**，确保**客户端身份验证**并**服务器身份验证**列出。
12.  保存证书模板的副本的唯一名称，例如**网络控制器模板**。

#### <a name="to-request-a-certificate-from-the-ca"></a>若要从 CA 请求证书

您可以使用证书管理单元请求证书。 你可以请求任何类型的证书已预配置并可供处理证书请求的 CA 的管理员。

**用户**或本地**管理员**是完成此过程所需的最低组成员身份。

1. 打开证书管理单元中的计算机。
2. 在控制台树中，单击**证书\(本地计算机\)** 。 选择**个人**证书存储区。
3. 上**操作**菜单，指向 * * 的所有任务<strong>，然后单击 * * 申请新证书</strong>以启动证书注册向导。 单击“下一步”  。
4. 选择**由管理员配置**证书注册策略，然后单击**下一步**。
5. 选择**Active Directory 注册策略**\(基于上一节中配置的 CA 模板\)。
6. 展开**详细信息**部分，然后配置以下各项。
   1. 絋粄**密钥用法**同时包含<strong>数字签名 * * 和 * * 密钥加密</strong>。
   2. 絋粄**应用程序策略**同时包含**服务器身份验证** \(1.3.6.1.5.5.7.3.1\)并**客户端身份验证**\(1.3.6.1.5.5.7.3.2\)。
7. 单击**属性**。
8. 上**使用者**选项卡上，在**使用者名称**，在**类型**，选择**公用名**。 在值中，指定**网络控制器 REST 终结点**。
9. 单击“应用”  ，然后单击“确定”  。
10. 单击**注册**。

在证书 MMC 中，单击要查看已注册的 ca 证书的个人存储区。

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>导出并将该证书复制到 SCVMM 库

创建自后\-签名或 CA\-签名的证书，您必须导出包含私钥的证书\(以.pfx 格式\)和不带私钥\(以 Base-64.cer 格式\)证书管理单元中。 

然后必须将复制到两个导出的文件**ServerCertificate.cr**并**NCCertificate.cr**导入 NC 服务模板时的时间在指定的文件夹。

1. 打开证书管理单元 (certlm.msc) 并在本地计算机的个人证书存储中找到的证书。
2. 右\-单击证书中，单击**的所有任务**，然后单击**导出**。 此时将打开“证书导出向导”。 单击“下一步”  。
3. 选择**是**，将导出专用密钥选项，单击**下一步**。
4. 选择**个人信息交换-PKCS #12 (。PFX)** 并接受默认值为**包括证书路径中的所有证书**在可能的情况。
5. 分配用户/组和要导出的证书的密码，请单击**下一步**。
6. 在导出页的文件，浏览要放置导出的文件的位置并为其提供一个名称。
7. 同样，将导出的证书。CER 格式。 注意：若要将导出到。CER 格式，请取消选中是，导出专用密钥选项。
8. 复制。PFX 的 ServerCertificate.cr 文件夹。
9. 复制。CER 文件复制到 NCCertificate.cr 文件夹。

完成后，刷新 SCVMM 库中的这些文件夹，并确保已复制这些证书。 继续使用网络控制器服务模板配置和部署。

## <a name="authenticating-southbound-devices-and-services"></a>Southbound 设备和服务进行身份验证 

与主机和 SLB MUX 设备的网络控制器通信使用证书进行身份验证。 与主机之间的通信是通过 OVSDB 协议，而与 SLB MUX 设备之间的通信是通过 WCF 协议。

### <a name="hyper-v-host-communication-with-network-controller"></a>与网络控制器的 hyper V 主机通信

与通过 OVSDB 的 HYPER-V 主机的通信，网络控制器需要提供到主机的证书。 默认情况下，SCVMM 提取在网络控制器上配置的 SSL 证书，并将其用于与主机进行北向通信。

这是为什么该 SSL 证书必须具有配置客户端身份验证 EKU 的原因。 在"服务器"REST 上配置此证书资源\(的 HYPER-V 主机都包含在为服务器资源的网络控制器\)，并可通过运行 Windows PowerShell 命令查看**Get NetworkControllerServer**。

下面是在服务器 REST 资源的部分示例。

      "resourceId": "host31.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "host31.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],

相互身份验证的 HYPER-V 主机还必须具有与网络控制器进行通信的证书。 

你可以注册来自证书颁发机构的证书\(CA\)。 如果主机计算机上找不到基于 CA 证书，SCVMM 创建自签名的证书，并在主机上设置它。

网络控制器和证书必须由每个其他受信任的 HYPER-V 主机。 HYPER-V 主机证书的根证书必须存在于网络控制器受信任的根证书颁发机构存储在本地计算机，反之亦然。 

当使用自助\-签名证书，SCVMM 可以确保所需的证书的本地计算机的受信任的根证书颁发机构存储中存在。 

如果 CA 基于证书将用于 HYPER-V 主机，您需要确保 CA 根证书上的本地计算机的网络控制器的受信任的根证书颁发机构存储区中存在。

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>与网络控制器的软件负载均衡器 MUX 通信

软件负载均衡器多路传送器\(MUX\) ，网络控制器通信的 WCF 协议，使用证书进行身份验证。

默认情况下，SCVMM 提取在网络控制器上配置的 SSL 证书，并将其用于进行北向通信与 Mux 设备。 在"NetworkControllerLoadBalancerMux"上配置此证书的 REST 资源并可通过执行 Powershell cmdlet 查看**Get NetworkControllerLoadBalancerMux**。

MUX REST 资源的示例\(分部\):

      "resourceId": "slbmux1.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "slbmux1.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],

相互身份验证，则还必须在 SLB MUX 设备上安装证书。 在部署软件负载均衡器使用 SCVMM 时，此证书是由 SCVMM 自动进行配置。

>[!IMPORTANT]
>在主机和 SLB 节点中，很重要，受信任的根证书颁发机构证书存储区不包含任何证书的"颁发给"不是"颁发者"相同。 如果发生这种情况，网络控制器和 southbound 设备之间的通信将失败。

网络控制器和 SLB MUX 证书必须受彼此\(SLB MUX 证书的根证书必须位于受信任的根证书颁发机构存储在网络控制器计算机，反之亦然\)。 当使用自助\-签名证书，SCVMM 可确保所需的证书都位于本地计算机存储中受信任的根证书颁发机构。




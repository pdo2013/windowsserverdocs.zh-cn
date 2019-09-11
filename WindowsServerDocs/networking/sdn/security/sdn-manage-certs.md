---
title: 管理软件定义的网络的证书
description: 当你在 Windows Server 2016 Datacenter 中部署软件定义的网络（SDN）时，你可以使用本主题来了解如何管理网络控制器 Northbound 和 Southbound 通信的证书。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: 8e2feba8232ae87d59478d3522c4e6f02baf27b8
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870100"
---
# <a name="manage-certificates-for-software-defined-networking"></a>管理软件定义的网络的证书

>适用于：Windows Server（半年频道）、Windows Server 2016

当你在 Windows Server 2016 Datacenter 中部署软件定义的网络\(SDN\)并使用系统时，你可以使用本主题来了解如何管理网络控制器 Northbound 和 Southbound 通信的证书将 SCVMM \(\) Virtual Machine Manager 为 SDN 管理客户端。

>[!NOTE]
>有关网络控制器的概述信息，请参阅[网络控制器](../technologies/network-controller/Network-Controller.md)。

如果你未使用 Kerberos 来保护网络控制器通信，则可以使用 x.509 证书进行身份验证、授权和加密。

Windows Server 2016 Datacenter 中的 SDN 支持自\-签名证书和证书\(\)颁发机构签名的 x.509 证书。 本主题提供了有关创建这些证书并将其应用于使用管理客户端和与网络设备（例如软件）进行 Southbound 通信的安全网络控制器 Northbound 通信通道的分步说明。负载均衡\(器\)SLB。
.
使用基于证书\-的身份验证时，必须在使用以下方法的网络控制器节点上注册一个证书。

1. 加密 Northbound 与网络控制器\(节点\)和管理客户端之间安全套接字层 SSL 的通信，如 System Center Virtual Machine Manager。
2. 网络控制器节点与 Southbound 设备和服务（例如 hyper-v 主机和软件负载平衡\(器 SLBs\)）之间的身份验证。

## <a name="creating-and-enrolling-an-x509-certificate"></a>创建和注册 x.509 证书

你可以创建并注册自\-签名证书或 CA 颁发的证书。

>[!NOTE]
>使用 SCVMM 部署网络控制器时，必须指定在配置网络控制器服务模板期间用于对 Northbound 通信进行加密的 x.509 证书。

证书配置必须包含以下值。

- **RestEndPoint**文本框的值必须是网络控制器完全限定的域名\(FQDN\)或 IP 地址。 
- **RestEndPoint**值必须与 x.509 证书的使用\(者名称公用名\) （CN）匹配。

### <a name="creating-a-self-signed-x509-certificate"></a>创建自\-签名 x.509 证书

你可以创建自签名的 x.509 证书， \(并使用通过密码\)保护的私钥将其导出，方法是在单\-节点和多\-节点网络控制器部署中执行以下步骤.

创建自\-签名证书时，可以使用以下准则。

- 你可以使用 DnsName 参数的网络控制器 REST 终结点的 IP 地址，但不建议这样做，因为它要求网络控制器节点都位于单个管理子网\(中，例如在单个机架上\)
- 对于多节点 NC 部署，指定的 dns 名称将成为网络控制器群集\(dns 主机的 FQDN。\) 
- 对于单节点网络控制器部署，DNS 名称可以是网络控制器的主机名，后跟完整域名。

#### <a name="multiple-node"></a>多个节点

可以使用[new-selfsignedcertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) Windows PowerShell 命令创建自\-签名证书。

**语法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**示例用法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>单节点

可以使用[new-selfsignedcertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) Windows PowerShell 命令创建自\-签名证书。

**语法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**示例用法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>创建 CA\-签名的 x.509 证书

若要使用 CA 创建证书，必须已使用 Active Directory \(证书服务\(AD CS\)部署了公钥基础\)结构 PKI。 

>[!NOTE]
>你可以使用第三方 Ca 或工具（如 openssl）创建与网络控制器一起使用的证书，但本主题中的说明特定于 AD CS。 若要了解如何使用第三方 CA 或工具，请参阅您正在使用的软件的文档。

使用 CA 创建证书包括以下步骤。

1. 你或你的组织的域或安全管理员配置证书模板
2. 你或你的组织的网络控制器管理员或 SCVMM 管理员请求来自 CA 的新证书。

#### <a name="certificate-configuration-requirements"></a>证书配置要求

在下一步中配置证书模板时，请确保所配置的模板包含以下必需元素。

1. 证书使用者名称必须是 Hyper-v 主机的 FQDN
2. 证书必须位于本地计算机个人存储中（My-cert： \ localmachine\my）
3. 证书必须具有服务器身份验证（EKU：1.3.6.1.5.5.7.3.1）和客户端身份验证（EKU：1.3.6.1.5.5.7.3.2）应用程序策略。

>[!NOTE]
>\(如果 hyper-v\)\) \(主机上的 "我的证书： \ localmachine\my" 证书存储有多个 x.509 证书，使用者名称（CN）作为主机完全限定的域名 FQDN，\-确保 SDN 使用的证书具有 OID 1.3.6.1.4.1.311.95.1.1.1 的附加自定义增强型密钥用法属性。 否则，网络控制器与主机之间的通信可能不起作用。

#### <a name="to-configure-the-certificate-template"></a>配置证书模板
  
>[!NOTE]
>在执行此过程之前，应在证书模板控制台中查看证书要求和可用的证书模板。 您可以修改现有模板，也可以创建现有模板的副本，然后修改模板的副本。 建议创建现有模板的副本。

1. 在安装 AD CS 的服务器上服务器管理器中，单击 "**工具**"，然后单击 "**证书颁发机构**"。 此时将打开证书颁发机构\("\) Microsoft 管理控制台" MMC。 
2. 在 MMC 中，双击 CA 名称，右键单击 "**证书模板**"，然后单击 "**管理**"。
3. 此时将打开 "证书模板" 控制台。 所有证书模板都显示在 "详细信息" 窗格中。
4. 在详细信息窗格中，单击要复制的模板。
5.  单击 "**操作**" 菜单，然后单击 "**复制模板**"。 此时将打开 "模板**属性**" 对话框。
6.  在 "模板**属性**" 对话框的 "**使用者名称**" 选项卡上，单击 **"在请求中提供"** 。 \(此设置是网络控制器 SSL 证书所必需的。\)
7.  在 "模板**属性**" 对话框中的 "**请求处理**" 选项卡上，确保选择了 "**允许导出私钥**"。 还要确保已选择 "**签名和加密**" 目的。
8.  在 "模板**属性**" 对话框中的 "**扩展**" 选项卡上，选择 "**密钥用法**"，然后单击 "**编辑**"。
9.  在 "**签名**" 中，确保选择了 "**数字签名**"。
10.  在 "模板**属性**" 对话框中的 "**扩展**" 选项卡上，选择 "**应用程序策略**"，然后单击 "**编辑**"。
11.  在 "**应用程序策略**" 中，确保列出了**客户端身份验证**和**服务器身份验证**。
12.  使用唯一名称（如**网络控制器模板**）保存证书模板的副本。

#### <a name="to-request-a-certificate-from-the-ca"></a>从 CA 请求证书

您可以使用证书管理单元请求证书。 你可以请求任何类型的证书，这些证书已预先配置，并由处理证书请求的 CA 的管理员提供。

**用户**或本地**管理员**是完成此过程所需的最低组成员身份。

1. 打开计算机的 "证书" 管理单元。
2. 在控制台树中，单击 **" \(证书"\)"本地计算机**"。 选择 "**个人**" 证书存储。
3. 在 "**操作**" 菜单上，指向 "所有任务<strong>"，然后单击 "申请新证书"</strong>以启动证书注册向导。 单击“下一步”。
4. 选择管理员证书注册策略**配置**的，然后单击 "**下一步**"。
5. 根据你在上一部分\)中配置的 CA 模板选择**Active Directory 注册策略** \(。
6. 展开 "**详细信息**" 部分，然后配置以下各项。
   1. 请确保**密钥用法**同时包含<strong>数字签名 * * 和 * * 密钥加密</strong>。
   2. 确保**应用程序策略**包括**服务器身份验证** \(1.3.6.1.5.5.7.3.1\)和**客户端身份验证** \(1.3.6.1.5.5.7.3.2。\)
7. 单击**属性**。
8. 在 "**使用者**" 选项卡上的 "**使用者名称** **" 中，选择 "** **公用名**"。 在 "值" 中，指定**网络控制器 REST 终结点**。
9. 单击“应用”，然后单击“确定”。
10. 单击**注册**。

在 "证书" MMC 中，单击 "个人" 存储区，查看已从 CA 注册的证书。

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>导出证书并将其复制到 SCVMM 库

创建\-自签名证书或 CA\-签名证书后，必须以 .pfx 格式\)导出具有私钥\(的证书，且不带\(以64格式的私钥。\)从 "证书" 管理单元。 

然后，必须将两个导出文件复制到您在导入 NC 服务模板时指定的**ServerCertificate.cr**和**NCCertificate.cr**文件夹。

1. 打开 "证书" 管理单元（certlm.msc），并在本地计算机的 "个人" 证书存储中找到该证书。
2. 右键\-单击该证书，单击 "**所有任务**"，然后单击 "**导出**"。 此时将打开“证书导出向导”。 单击“下一步”。
3. 选择 **"是**，导出私钥" 选项，然后单击 "**下一步**"。
4. 选择 "**个人信息交换-PKCS #12 （。PFX）** 并接受默认值，以便在可能的情况下**包括证书路径中的所有证书**。
5. 为要导出的证书分配用户/组和密码，然后单击 "**下一步**"。
6. 在 "要导出的文件" 页上，浏览要放置导出文件的位置，并为其指定名称。
7. 同样，在中导出证书。CER 格式。 注意:将导出到。CER 格式，取消选中 "是，导出私钥" 选项。
8. 复制。PFX 用于 ServerCertificate.cr 文件夹。
9. 复制。CER 文件到 NCCertificate.cr 文件夹。

完成后，请刷新 SCVMM 库中的这些文件夹，并确保已复制这些证书。 继续网络控制器服务模板的配置和部署。

## <a name="authenticating-southbound-devices-and-services"></a>Southbound 设备和服务的身份验证 

网络控制器与主机和 SLB MUX 设备通信使用证书进行身份验证。 与 OVSDB 协议通信时，与 SLB MUX 设备通信的方式高于 WCF 协议。

### <a name="hyper-v-host-communication-with-network-controller"></a>Hyper-v 主机与网络控制器之间的通信

若要通过 OVSDB 与 Hyper-v 主机进行通信，网络控制器需要向主机提供证书。 默认情况下，SCVMM 选取网络控制器上配置的 SSL 证书，并将其用于 southbound 与主机的通信。

这就是 SSL 证书必须配置客户端身份验证 EKU 的原因。 此证书是在 "服务器" REST 资源\(上配置的。 hyper-v 主机在网络控制器中表示为服务器资源\)，可通过运行 Windows PowerShell 命令**NetworkControllerServer 进行查看。** .

下面是服务器 REST 资源的部分示例。

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

对于相互身份验证，Hyper-v 主机还必须具有证书才能与网络控制器通信。 

你可以从证书颁发机构\(CA\)注册证书。 如果在主机计算机上找不到基于 CA 的证书，SCVMM 会创建一个自签名证书，并在主机上对其进行设置。

网络控制器和 Hyper-v 主机证书必须互相信任。 Hyper-v 主机证书的根证书必须存在于本地计算机的 "受信任的根证书颁发机构" 存储中，反之亦然。 

使用自\-签名证书时，SCVMM 确保本地计算机的 "受信任的根证书颁发机构" 存储中存在所需的证书。 

如果为 Hyper-v 主机使用基于 CA 的证书，则需要确保 CA 根证书存在于本地计算机的网络控制器的 "受信任的根证书颁发机构" 存储中。

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>软件负载均衡器与网络控制器之间的通信

软件负载平衡器多\(路\)器 MUX 和网络控制器使用证书进行身份验证，通过 WCF 协议进行通信。

默认情况下，SCVMM 选取网络控制器上配置的 SSL 证书，并将其用于 southbound 与 Mux 设备的通信。 此证书在 "NetworkControllerLoadBalancerMux" REST 资源上配置，可通过执行 Powershell cmdlet **NetworkControllerLoadBalancerMux**来查看。

MUX REST 资源\(部分\)示例：

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

对于相互身份验证，还必须在 SLB MUX 设备上具有证书。 使用 SCVMM 部署软件负载平衡器时，SCVMM 会自动配置此证书。

>[!IMPORTANT]
>在主机和 SLB 节点上，"受信任的根证书颁发机构" 证书存储区不包括任何 "颁发给" 的证书与 "颁发者" 不同，这一点非常重要。 如果发生这种情况，网络控制器与 southbound 设备之间的通信将失败。

网络控制器和 SLB mux 证书必须彼此\(信任。 slb mux 证书的根证书必须存在于网络控制器计算机受信任的根证书颁发机构存储中，反之亦然。\) 使用自\-签名证书时，SCVMM 确保本地计算机的 "受信任的根证书颁发机构" 存储中包含所需的证书。




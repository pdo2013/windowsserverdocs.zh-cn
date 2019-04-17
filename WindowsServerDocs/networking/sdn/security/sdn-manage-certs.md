---
title: 管理证书软件定义的网络
description: 你可以使用本主题以了解如何进行部署软件定义网络 (SDN) 在 Windows Server 2016 Datacenter 时 Northbound 网络控制器和 Southbound 通信管理证书。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3036de9161cabc2f3a85a1d3b2ce7739f0ff6bd3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="manage-certificates-for-software-defined-networking"></a>管理证书软件定义的网络

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解如何管理 Northbound 网络控制器和 Southbound 通信证书，当您使用的 System Center 虚拟机 Manager \(SCVMM\) 为 SDN 管理客户端和部署 Windows Server 2016 数据中心中的软件定义网络 \(SDN\)。

>[!NOTE]
>有关网络控制器概要信息，请参阅[网络控制器](../technologies/network-controller/Network-Controller.md)。

如果你不使用 Kerberos 保护的网络控制器通信，您可以使用 X.509 证书，身份验证、 授权，和加密。

在 Windows Server 2016 Datacenter SDN 支持两者 self\ 登录和证书颁发机构 \ (CA\)-登录 X.509 证书。 本主题提供的网络设备，如软件负载平衡 \(SLB\) 创建这些证书，并将其应用以指导安全与管理客户端的联络渠道 Northbound 网络控制器和 Southbound 通信。
.
当你使用的基于 certificate\ 的身份验证时，你必须注册网络控制器节点上的一个证书，可通过以下方式。

1. 加密安全套接字层 \(SSL\) 节点网络控制器和管理客户端，如 System Center 虚拟机 Manager 之间 Northbound 通信。
2. 网络控制器节点 Southbound 设备和服务，如 HYPER-V 主机和软件负载平衡 \(SLBs\) 之间的身份验证。

## <a name="creating-and-enrolling-an-x509-certificate"></a>创建和 X.509 证书的注册

你可以创建并的证书，CA 发行或 self\ 签名证书的注册。

>[!NOTE]
>当你正在使用 SCVMM 部署网络控制器时、 必须指定用来配置了网络控制器服务模板期间加密 Northbound 通信 X.509 证书。

证书配置必须包括以下值。

- 值**RestEndPoint**文本框必须网络控制器完全限定域名 \(FQDN\) 或 IP 地址。 
- **RestEndPoint**值必须与主题名称匹配 \ （CN\ 常见的名称) X.509 证书。

### <a name="creating-a-self-signed-x509-certificate"></a>创建 Self\ 登录 X.509 证书

你可以创建 X.509 自签名的证书，并将其导出专用密钥 \ （设有 password\） 按照以下步骤网络控制器的 single\ 节点和 multiple\ 节点部署。

当您创建 self\ 签名证书时，你可以使用下面的准则。

- 用于 DnsName 参数-网络控制器其余端点的 IP 地址，但这不建议，因为它需要网络控制器节点是所有位于单个管理子网 \ （例如位于单个 rack\)
- 多个节点 NC 部署，DNS 指定的名称，你将变得网络控制器群集 FQDN \ (自动创建了 DNS 主机 A 记录。 \) 
- 对于单个节点网络控制器部署，DNS 名称可以网络控制器主机名称后跟完整的域名。

#### <a name="multiple-node"></a>多个节点

你可以使用[新建 SelfSignedCertificate](https://technet.microsoft.com/en-us/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) Windows PowerShell 命令创建 self\ 签名证书。

**语法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**示例使用情况**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>一个节点

你可以使用[新建 SelfSignedCertificate](https://technet.microsoft.com/en-us/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) Windows PowerShell 命令创建 self\ 签名证书。

**语法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**示例使用情况**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>创建 CA\ 登录 X.509 证书

若要使用的是 CA 创建证书，你必须已部署使用 Active Directory 证书服务 \(AD CS\) 公钥基础结构 \(PKI\)。 

>[!NOTE]
>使用第三方 Ca 或 openssl，如工具创建证书用于与网络控制器、，但是，本主题中的说明进行操作特定于广告客户服务。 若要了解如何使用第三方 CA 或的工具，请参阅文档，你正在使用的软件。

与 CA 创建证书包含以下步骤。

1. 你或你的组织域或安全管理员配置证书模板
2. 你或你的组织控制器的网络管理员或 SCVMM 管理员请求新的证书，ca。

#### <a name="certificate-configuration-requirements"></a>证书配置要求

时配置证书模板中的下一步，确保你配置模板包含所需的以下元素。

1. 必须 HYPER-V 主机 FQDN 证书主题名称。
2. 必须在本地计算机个人应用商店中放置证书 (我 – 证书： \localmachine\my)
3. 证书必须这两个服务器身份验证 (EKU: 1.3.6.1.5.5.7.3.1) 和客户端身份验证 (EKU: 1.3.6.1.5.5.7.3.2) 应用程序的策略。

>[!NOTE]
>如果个人 \ (我 – 证书：\localmachine\my\) 证书 Hyper\ V 主机上的应用商店有多个 X.509 作为主机完全限定域名 \(FQDN\) 证书的对象名称 (CN)，请确保将 SDN 由使用证书已具有 OID 1.3.6.1.4.1.311.95.1.1.1 其他自定义增强键使用量属性。 否则，之间网络控制器和主机通信可能无法工作。

#### <a name="to-configure-the-certificate-template"></a>若要配置证书模板
  
>[!NOTE]
>执行此过程之前，您应查看证书要求提供证书中和模板证书模板主机。 可以修改现有模板或创建重复的现有模板，然后修改模板的副本。 建议创建一份现有模板。

1. 在服务器了安装广告客户服务，在服务器管理器中，单击**工具**，然后单击**证书颁发机构**。 证书颁发机构 Microsoft 管理控制台 \(MMC\) 打开。 
2. 在 MMC 中，双击 CA 名称、 右键单击**证书模板**，然后单击**管理**。
3. 证书模板主机将打开。 所有证书模板显示详细信息窗格中。
4. 在详细信息窗格中，单击你想要复制的模板。
5.  单击**操作**菜单，然后依次单击**复制模板**。 模板**属性**对话框中打开。
6.  在模板**属性**对话框中，在**主题名称**选项卡上，单击**请求中提供**。 \ (此设置，才能进行网络控制器 SSL 证书。 \)
7.  在模板**属性**对话框中，在**处理请求**选项卡时，请确保**允许专用键导出**选择。 确保**签名和加密**选中目的。
8.  在模板**属性**对话框中，在**扩展**选项卡上，选择**密钥用法**，然后单击**编辑**。
9.  在**签名**，确保**数字签名**选择。
10.  在模板**属性**对话框中，在**扩展**选项卡上，选择**应用程序策略**，然后单击**编辑**。
11.  在**应用程序策略**，确保**客户端身份验证**和**服务器身份验证**列出。
12.  唯一的名称，如保存的证书模板副本**网络控制器模板**。

#### <a name="to-request-a-certificate-from-the-ca"></a>若要从 CA 申请证书

你可以使用证书贴靠到请求证书。 你可以请求证书已预配置的和供 CA 进程证书请求管理员使用的任何的类型。

**用户**或本地**管理员**是要完成此过程的最低组成员。

1. 对于计算机打开证书管理单元。
2. 在控制台树中，单击**证书 \(Local Computer\)**。 选择**个人**证书官方商城。
3. 在**操作**菜单、 指向 * * 所有任务 * *，，然后单击**申请新证书**要启动该证书的注册向导。 单击**下一步**。
4. 选择**由你的管理员配置**证书的注册策略，然后单击**下一步**。
5. 选择**Active Directory 注册策略**\ （基于你在上一 section\ 配置 CA 模板）。
6. 展开**详细信息**部分并配置了以下各项。
    1. 确保**键使用量**包括 * * 数字签名 * * 和**密钥加密**。
    2. 确保**应用程序策略**包括**服务器身份验证**\(1.3.6.1.5.5.7.3.1\) 和**客户端身份验证**\(1.3.6.1.5.5.7.3.2\)。
7. 单击**属性**。
8. 在**主题**选项卡上，在**主题名称**，请在**类型**、 选择**常见名称**。 在值指定**网络控制器其余端点**。
9. 单击**应用**，然后单击**确定**。
10. 单击**注册**。

在证书 MMC 中，单击个人的应用商店以查看你登记了从 CA 证书。

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>导出和证书复制到 SCVMM 库

创建后 self\ 登录或 CA\ 签名证书，你必须导出证书专用密钥 \(in.pfx format\) 和而无需专用密钥 \ （以基于 64.cer format\) 从证书管理单元。 

你必须将复制到两个的已导出的文件**ServerCertificate.cr**和**NCCertificate.cr**导入 NC 服务模板时次指定的文件夹。

1. 打开中 (certlm.msc) 的证书贴靠，并且在本地计算机个人证书应用商店中找到的证书。
2. Right\ 键单击证书，请单击**所有任务**，然后单击**导出**。 证书导出向导将打开。 单击**下一步**。
3. 选择**是**导出专用选项，请单击**下一步**。
4. 选择**个人信息 Exchange-#12 PKCS (。PFX)**并接受默认为**数据包括所有证书颁发路径中**尽可能。
5. 分配用户组和证书导出你的密码，请单击**下一步**。
6. 在文件导出页面，浏览你想要放置的已导出的文件的位置，并为该集锦命名。
7. 同样，导出中的证书。CER 格式。 注意： 要导出到。CER 格式取消选中是，导出专用选项。
8. 副本。PFX ServerCertificate.cr 文件夹。
9. 副本。CER NCCertificate.cr 文件夹的文件。

当你完成后时，刷新 SCVMM 库中的这些文件夹，并确保你具有复制这些证书。 使用网络控制器服务模板配置和部署继续操作。

## <a name="authenticating-southbound-devices-and-services"></a>身份验证 Southbound 设备和服务 

网络控制器通信主机和 SLB 输入混合设备使用的身份验证的证书。 与主机通信是通过 OVSDB 协议，尽管通信 SLB 输入混合设备是通过 WCF 协议。

### <a name="hyper-v-host-communication-with-network-controller"></a>与网络控制器 hyper V 主机通信

通过 OVSDB HYPER-V 主机的通信、 网络控制器需要出示的证书到主计算机。 默认情况下，SCVMM 选取 SSL 证书网络控制器上配置，并将其用于与主机 southbound 通信。

这就是为何 SSL 证书必须配置客户端身份验证 EKU 的原因。 此证书是否配置"服务器"其余资源 \ （HYPER-V 主机都包含在网络控制器服务器 resource\ 作为），并且可以通过运行 Windows PowerShell 命令查看**获取 NetworkControllerServer**。

下面是服务器资源其余部分示例。

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

相互身份验证、 HYPER-V 主机还必须具有网络控制器与进行通信的证书。 

您可以注册证书颁发机构 \(CA\) 从证书。 如果在主计算机上找不到 CA 基于证书，SCVMM 创建自签名的证书，并提供该主机上。

必须通过彼此信任网络控制器和 HYPER-V 主机证书。 必须存在于 HYPER-V 主机证书根证书网络控制器信任根证书颁发机构存储在本地计算机，反之亦然。 

当你使用的 self\ 签名证书时，SCVMM 可确保所需的证书存在本地计算机信任根证书颁发机构应用商店中。 

如果你将基于 CA 证书用于 HYPER-V 主机，你需要确保 CA 根证书出现了网络控制器信任根证书颁发机构官方商城本地计算机上。

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>与网络控制器的软件负载平衡输入混合通信

软件负载平衡多重通道 \(MUX\) 和网络控制器通信 WCF 协议中，通过使用进行身份验证的证书。

默认情况下，SCVMM 选取 SSL 证书网络控制器上配置，并使用它进行 southbound 通信与输入混合设备。 此证书是否配置"NetworkControllerLoadBalancerMux"上停留资源，并且可以通过执行 Powershell cmdlet 查看**获取 NetworkControllerLoadBalancerMux**。

输入混合其余资源 \(partial\) 的示例：

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

对于相互身份验证，你还必须证书 SLB 输入混合设备上。 部署软件负载平衡使用 SCVMM 时，将自动通过 SCVMM 配置该证书。

>[!IMPORTANT]
>在主机和 SLB 节点，请务必信任根证书颁发机构证书应用商店中不包括任何证书:"已颁发给"不是"颁发机构"相同。 如果发生这种情况，网络控制器和 southbound 设备之间进行通信无法正常工作。

必须通过彼此信任网络控制器和 SLB 输入混合证书 \ （必须出现了网络控制器机器受信任的根证书颁发机构存储和钳 versa\ SLB 输入混合证书根证书）。 当你使用的 self\ 签名证书时，SCVMM 确保中所需的证书有在受信任的根证书颁发机构存储在本地计算机。




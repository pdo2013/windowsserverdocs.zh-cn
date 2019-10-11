---
title: 用于排查 DNS 相关激活问题的指南
description: ''
ms.topic: article
ms.date: 09/10/2019
ms.technology: server-general
ms.assetid: ''
author: Teresa-Motiv
ms.author: v-tea
ms.localizationpriority: medium
ms.openlocfilehash: e2bd9c766f07591e0c643a6cea644b2db7a95364
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2019
ms.locfileid: "71960961"
---
# <a name="guidelines-for-troubleshooting-dns-related-activation-issues"></a>用于排查 DNS 相关激活问题的指南

如果符合下面的一个或多个条件，则可能必须使用这其中的某些方法：

- 使用批量许可介质和批量许可常规产品密钥安装下述操作系统之一：&nbsp;
   - Windows Server Standard 2012 R2
   - Windows Server 2016
   - Windows Server 2012 R2
   - Windows Server 2012
   - Windows Server 2008 R2
   - Windows Server 2008
   - Windows 10
   - Windows 8.1
   - Windows 8
- 激活向导不能连接到 KMS 主机。

当你尝试激活客户端系统时，激活向导使用 DNS 来查找相应的运行 KMS 软件的计算机。 如果向导在查询 DNS 后没有找到 KMS 主机的 DNS 条目，则会报告错误。   

<a id="list"></a>请查看以下列表，找到适合自己情况的方法：

- 如果不能安装 KMS 主机或不能使用 KMS 激活，请尝试[将产品密钥更改为 MAK](#change-the-product-key-to-an-mak) 过程。
- 如果必须安装并配置 KMS 主机，请使用[配置客户端激活时所使用的 KMS 主机](#configure-a-kms-host-for-the-clients-to-activate-against)过程。
- 如果客户端找不到现有 KMS 主机，请使用以下过程来排查路由配置问题。 这些过程按照从简单到复杂的顺序进行排列。
  - [验证到 DNS 服务器的基本 IP 连接](#verify-basic-ip-connectivity-to-the-dns-server)
  - [验证 KMS 主机配置](#verify-the-configuration-of-the-kms-host)  
  - [确定路由问题的类型](#determine-the-type-of-routing-issue)
  - [验证 DNS 配置](#verify-the-dns-configuration)
  - [手动创建 KMS SRV 记录](#manually-create-a-kms-srv-record)
  - [手动为 KMS 客户端分配 KMS 主机](#manually-assign-a-kms-host-to-a-kms-client)
  - [将 KMS 主机配置为在多个 DNS 域中发布](#configure-the-kms-host-to-publish-in-multiple-dns-domains)

## <a name="change-the-product-key-to-an-mak"></a>将产品密钥更改为 MAK

如果不能安装 KMS 主机，或者因为某个其他的原因而不能使用 KMS 激活，请将产品密钥更改为 MAK。 如果从 Microsoft Developer Network (MSDN) 或 TechNet 下载 Windows 映像，则在介质下列出的库存单位 (SKU) 通常为批量许可介质，提供的产品密钥为 MAK 密钥。

若要将产品密钥更改为 MAK，请执行以下步骤：

1. 打开提升的命令提示符窗口。 为此，请按 Windows 徽标键+X，右键单击“命令提示符”，然后选择“以管理员身份运行”   。 如果收到管理员密码提示或确认提示，请键入密码或进行确认。
2. 在命令提示符处运行以下命令：
   ```cmd
    slmgr -ipk xxxxx-xxxxx-xxxxx-xxxxx-xxxxx
   ```
   > [!NOTE]
   > **xxxxx-xxxxx-xxxxx-xxxxx-xxxxx** 占位符表示 MAK 产品密钥。  

[返回到过程列表。](#list)

## <a name="configure-a-kms-host-for-the-clients-to-activate-against"></a>配置客户端激活时所使用的 KMS 主机

KMS 激活要求配置客户端激活时所使用的 KMS 主机。 如果环境中没有配置的 KMS 主机，请使用适当的 KMS 主机密钥安装并激活一个。 在网络上配置用于托管 KMS 软件的计算机以后，请发布域名系统 (DNS) 设置。

若要了解 KMS 主机配置过程，请参阅[使用密钥管理服务进行激活](https://docs.microsoft.com/windows/deployment/volume-activation/activate-using-key-management-service-vamt)和[安装和配置 VMAT](https://docs.microsoft.com/windows/deployment/volume-activation/install-configure-vamt)。

[返回到过程列表。](#list)

## <a name="verify-basic-ip-connectivity-to-the-dns-server"></a>验证到 DNS 服务器的基本 IP 连接

使用 ping 命令验证到 DNS 服务器的基本 IP 连接。 为此，请在遇到错误的 KMS 客户端上和 KMS 主机上执行以下步骤：

1. 打开提升的命令提示符窗口。
1. 在命令提示符处运行以下命令：
   ```cmd
   ping <DNS_Server_IP_address>
   ```
   > [!NOTE]
   > 如果此命令的输出不包含“来自...的回复”短语，则表明存在网络问题或 DNS 问题，必须解决该问题后才能使用本文中的其他过程。 若要详细了解在不能 ping DNS 服务器的情况下如何排查 TCP/IP 问题，请参阅[针对 TCP/IP 问题的高级故障排除](https://docs.microsoft.com/windows/client-management/troubleshoot-tcpip)。

[返回到过程列表。](#list)

## <a name="verify-the-configuration-of-the-kms-host"></a>验证 KMS 主机的配置

检查 KMS 主机服务器的注册表，确定是否已将它注册到 DNS。 默认情况下，KMS 主机服务器每 24 小时动态注册 DNS SRV 记录一次。 
> [!IMPORTANT]
> 请认真遵循本部分所述的步骤。 如果注册表修改不正确，可能会发生严重问题。 在修改注册表之前，请[备份注册表](https://support.microsoft.com/en-us/help/322756)，以便在出现问题时可以还原。  

若要检查此设置，请执行以下步骤：
1. 启动“注册表编辑器”。 为此，请右键单击“开始”，选择“运行”，键入 **regedit**，然后按 Enter。  
1. 找到 **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SL** 子项，检查 **DisableDnsPublishing** 条目的值。 此条目有以下可能值：
   - **0** 或未定义（默认）：KMS 主机服务器每 24 小时注册 SRV 记录一次。
   - **1**：KMS 主机服务器不自动注册 SRV 记录。 如果实现不支持动态更新，请参阅[手动创建 KMS SRV 记录](#manually-create-a-kms-srv-record)。  
1. 如果 **DisableDnsPublishing** 条目缺失，请创建它（类型为 DWORD）。 如果可以接受动态注册，请将此值保留为未定义或设置为 **0**。

[返回到过程列表。](#list)

## <a name="determine-the-type-of-routing-issue"></a>确定路由问题的类型

可使用以下命令来确定这是名称解析问题还是 SRV 记录问题。  

1. 在 KMS 客户端上，打开提升的命令提示符窗口。  
1. 在命令提示符处运行以下命令：
   ```cmd
   cscript \windows\system32\slmgr.vbs -skms <KMS_FQDN>:<port>
   cscript \windows\system32\slmgr.vbs -ato
   ```
   > [!NOTE]
   > 在此命令中，<KMS_FQDN> 表示 KMS 主机的完全限定的域名 (FQDN)，\<port\> 表示 KMS 使用的 TCP 端口。  

   如果这些命令解决了此问题，则表明这是 SRV 记录问题。 排查其问题时，可以使用[手动为 KMS 客户端分配 KMS 主机](#manually-assign-a-kms-host-to-a-kms-client)过程中记录的某个命令。  

1. 如果此问题仍然存在，请运行以下命令：
   ```cmd
   cscript \windows\system32\slmgr.vbs -skms <IP Address>:<port>
   cscript \windows\system32\slmgr.vbs -ato
   ```
   > [!NOTE]
   > 在此命令中，\<IP Address\> 表示 KMS 主机的 IP 地址，\<port\> 表示 KMS 使用的 TCP 端口。  

   如果这些命令解决了此问题，则表明这很可能是名称解析问题。 有关其他故障排除信息，请参阅[验证 DNS 配置](#verify-the-dns-configuration)过程。

1. 如果这些命令都无法解决此问题，请检查计算机的防火墙配置。 在 KMS 客户端和 KMS 主机之间进行的任何激活通信均使用 1688 TCP 端口。 KMS 客户端与 KMS 主机上的防火墙都必须允许通过端口 1688 进行通信。

[返回到过程列表。](#list)

## <a name="verify-the-dns-configuration"></a>验证 DNS 配置

>[!NOTE]
> 除非另有说明，否则请在遇到相应错误的 KMS 客户端上执行以下步骤。

1. 打开提升的命令提示符窗口
1. 在命令提示符处运行以下命令：
   ```cmd
   IPCONFIG /all
   ```
1. 注意命令结果中的以下信息：
   - KMS 客户端计算机的已分配 IP 地址
   - KMS 客户端计算机使用的主 DNS 服务器的 IP 地址
   - KMS 客户端计算机使用的默认网关的 IP 地址
   - KMS 客户端计算机使用的 DNS 后缀搜索列表
1. 验证 KMS 主机 SRV 记录是否已在 DNS 中注册。 要实现这一点，请执行下列操作：  
   1. 打开提升的命令提示符窗口。
   1. 在命令提示符处运行以下命令：
      ```cmd
      nslookup -type=all _vlmcs._tcp>kms.txt
      ```
   1. 打开此命令生成的 KMS.txt 文件。 此文件应该包含一个或多个类似于以下条目的条目：
       ```
       _vlmcs._tcp.contoso.com SRV service location:
       priority = 0
       weight = 0
       port = 1688 svr hostname = kms-server.contoso.com
       ```
       > [!NOTE]
       > 在此条目中，contoso.com 表示 KMS 主机的域。
      1. 验证 KMS 主机的 IP 地址、主机名、端口和域。
      1. 如果这些 **_vlmcs** 条目存在且包含预期的 KMS 主机名，请转到[手动为 KMS 客户端分配 KMS 主机](#manually-assign-a-kms-host-to-a-kms-client)。  
      > [!NOTE]
      > 即使 [**nslookup**](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup) 命令找到了 KMS 主机，也并不意味着 DNS 客户端能够找到 KMS 主机。 如果 **nslookup** 命令找到了 KMS 主机，但你仍然不能使用 KMS 主机进行激活，请检查其他 DNS 设置，例如主 DNS 后缀以及 DNS 后缀的搜索列表。
1. 验证主 DNS 后缀的搜索列表是否包含与 KMS 主机相关联的 DNS 域后缀。 如果搜索列表不包含该信息，请转到[将 KMS 主机配置为在多个 DNS 域中发布](#configure-the-kms-host-to-publish-in-multiple-dns-domains)过程。

[返回到过程列表。](#list)

## <a name="manually-create-a-kms-srv-record"></a>手动创建 KMS SRV 记录

若要手动为使用 Microsoft DNS 服务器的 KMS 主机创建 SRV 记录，请执行以下步骤：

1. 在 DNS 服务器上，打开 DNS 管理器。 若要打开 DNS 管理器，请依次选择“开始”、“管理工具”、“DNS”。   
1. 选择必须在其上创建 SRV 资源记录的 DNS 服务器。
1. 在控制台树中，展开“正向查找区域”  ，右键单击域，然后选择“其他新记录”。 
1. 在列表中向下滚动，选择“服务位置(SRV)”  ，然后选择“创建记录”。 
1. 键入以下信息：
   - 服务： **_VLMCS**
   - 协议： **_TCP**
   - 端口号:**1688**
   - 提供服务的主机： **&lt;*KMS 主机的 FQDN*&gt;**
1. 完成这些操作后，请选择“确定”  ，然后选择“完成”  。

若要手动为使用兼容 BIND 9.x 的 DNS 服务器的 KMS 主机创建 SRV 记录，请按该 DNS 服务器的说明操作，并提供以下 SRV 记录信息：

- 名称： **_vlmcs._TCP**&nbsp;
- 类型：**SRV**&nbsp;
- 优先级：**0**
- 权重：**0**
- 端口：**1688**
- 主机名： **&lt;KMS 主机的 FQDN 或 A-Name  &gt;**

> [!NOTE]
> KMS 不使用“优先级”或“权重”值。   但是，记录必须包含它们。

若要将兼容 BIND 9.x 的 DNS 服务器配置为支持 KMS 自动发布功能，请将 DNS 服务器配置为允许从 KMS 主机进行资源记录更新。 例如，将以下行添加到 Named.conf 或 Named.conf.local 的区域定义中：

```cmd
allow-update { any; };
```
## <a name="manually-assign-a-kms-host-to-a-kms-client"></a>手动为 KMS 客户端分配 KMS 主机

默认情况下，KMS 客户端使用自动发现过程。 根据此过程的要求，KMS 客户端会查询 DNS 中的一系列服务器，这些服务器已在客户端的成员身份区域中发布了 _vlmcs SRV 记录。 DNS 以随机顺序返回 KMS 主机的列表。 客户端会选取一个 KMS 主机并尝试在其上建立一个会话。 如果该尝试成功，客户端会缓存 KMS 主机的名称并尝试在下一次续订尝试时使用它。 如何会话设置失败，客户端会随机选取另一个 KMS 主机。 强烈建议使用自动发现过程。  

但是，你可以手动为特定的 KMS 客户端分配 KMS 主机。 为此，请按照以下步骤操作。

1. 在 KMS 客户端上，打开提升的命令提示符窗口。
1. 根据实现情况执行以下步骤之一：
   - 若要使用 KMS 主机的 FQDN 来分配该主机，请运行以下命令：
     ```cmd
     cscript \windows\system32\slmgr.vbs -skms <KMS_FQDN>:<port>
     ```
   - 若要使用 KMS 主机的版本 4 IP 地址来分配该主机，请运行以下命令：
     ```cmd
     cscript \windows\system32\slmgr.vbs -skms <IPv4Address>:<port>
     ```
    - 若要使用 KMS 主机的版本 6 IP 地址来分配该主机，请运行以下命令：
      ```cmd
      cscript \windows\system32\slmgr.vbs -skms <IPv6Address>:<port>
      ```
    - 若要使用 KMS 主机的 NETBIOS 名称来分配该主机，请运行以下命令：
      ```cmd
      cscript \windows\system32\slmgr.vbs -skms <NETBIOSName>:<port>
      ```
   - 若要在 KMS 客户端上恢复为自动发现，请运行以下命令：
     ```cmd
     cscript \windows\system32\slmgr.vbs -ckms
     ```
     > [!NOTE]
     > 这些命令使用以下占位符：
     >- **<KMS_FQDN>** 表示 KMS 主机的完全限定的域名 (FQDN)
     >- **\<IPv4Address\>** 表示 KMS 主机的 IPv4 地址
     >- **\<IPv6Address\>** 表示 KMS 主机的 IPv6 地址
     >- **\<NETBIOSName\>** 表示 KMS 主机的 NETBIOS 名称
     >- **\<port\>** 表示 KMS 使用的 TCP 端口。  

## <a name="configure-the-kms-host-to-publish-in-multiple-dns-domains"></a>将 KMS 主机配置为在多个 DNS 域中发布

> [!IMPORTANT]
> 请认真遵循本部分所述的步骤。 如果注册表修改不正确，可能会发生严重问题。 在修改注册表之前，请[备份注册表](https://support.microsoft.com/help/322756)，以便在出现问题时可以还原。

如[手动为 KMS 客户端分配 KMS 主机](#manually-assign-a-kms-host-to-a-kms-client)中所述，KMS 客户端通常使用自动发现过程来标识 KMS 主机。 此过程要求必须在 KMS 客户端计算机的 DNS 区域中提供 _vlmcs SRV 记录。 DNS 区域对应于计算机的主 DNS 后缀，或者对应于以下项之一：
- DNS 系统（例如 Active Directory 域服务 (AD DS) DNS）分配的计算机域（适用于已加入域的计算机）。
- 动态主机配置协议 (DHCP) 分配的计算机域（适用于工作组计算机）。 根据征求意见文档 (RFC) 2132 中的定义，此域名由代码值为 15 的选项定义。

默认情况下，KMS 主机将其 SRV 记录注册到 DNS 区域中，该区域对应于 KMS 主机的域。 例如，假定 KMS 主机加入 contoso.com 域。 在这种情况下，KMS 主机会将其 _vmlcs SRV 记录注册到 contoso.com DNS 区域。 因此，记录会将服务标识为 VLMCS._TCP.CONTOSO.COM。

如果 KMS 主机和 KMS 客户端使用不同的 DNS 区域，则必须将 KMS 主机配置为自动在多个 DNS 域中发布其 SRV 记录。 要实现这一点，请执行下列操作：

1. 在 KMS 主机上启动注册表编辑器。 
1. 找到并选择 **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SL** 子项。
1. 在“详细信息”窗格中右键单击空白区域，选择“新建”，然后选择“多值字符串值”。   
1. 输入 **DnsDomainPublishList** 作为新条目的名称。
1. 右键单击这个新的 **DnsDomainPublishList** 条目，然后选择“修改”。 
1. 在“编辑多值字符串”对话框中，键入 KMS 在单独的行中发布的每个 DNS 域后缀，然后选择“确定”。  
   > [!NOTE]
   > Windows Server 2008 R2 的 **DnsDomainPublishList** 的格式有所不同。 有关详细信息，请参阅“批量激活技术参考指南”。
1. 使用“服务”管理工具重启“软件许可”服务。 此操作创建 SRV 记录。
1. 验证 KMS 客户端是否可以使用典型的方法来联系已配置的 KMS 主机。 验证 KMS 客户端是否可以按名称和按 IP 地址来正确标识 KMS 主机。 如果这两个验证中的任一验证失败，请调查此 DNS 客户端解析程序问题。
1. 若要清除以前在 KMS 客户端上缓存的 KMS 主机名，请在 KMS 客户端上打开已提升权限的命令提示符窗口，然后运行以下命令：
   ```cmd
   cscript C:\Windows\System32\slmgr.vbs -ckms
   ```

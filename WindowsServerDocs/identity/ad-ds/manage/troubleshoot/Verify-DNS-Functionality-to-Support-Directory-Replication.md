---
ms.assetid: 709353b0-b913-4367-8580-44745183e2bc
title: 验证 DNS 功能以支持目录复制
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: ''
ms.suite: na
ms.technology: identity-adds
ms.author: joflore
ms.date: 05/31/2017
ms.tgt_pltfrm: na
author: Femila
ms.openlocfilehash: a55b95ee516abda8bdbae6e9829a161ef060012e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871868"
---
# <a name="verify-dns-functionality-to-support-directory-replication"></a>验证 DNS 功能以支持目录复制

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

 若要检查可能会影响需要与 Active Directory 复制的域名系统 (DNS) 设置，可以开始通过运行基本测试，以确保 DNS 正常操作为您的域。 运行基本测试后，可以测试 DNS 功能，包括资源记录注册和动态更新的其他方面。

但您可以运行此测试的任何域控制器上，通常在您认为的域控制器上运行此测试的基本 DNS 功能可能会遇到复制问题，例如，报告事件 Id 1844，1925、 2087、 域控制器或2088 DNS 事件查看器目录服务日志中。



## <a name="running-the-domain-controller-basic-dns-testtitle"></a>运行域控制器的基本 DNS 测试</title>

基本 DNS 测试检查 DNS 功能的以下方面：


- **连接：** 测试确定是否可以通过联系的域控制器在 DNS 中注册<system>ping</system>命令，并具有轻型目录访问协议 / 远程过程调用 (LDAP/RPC) 连接。 如果连接测试失败的域控制器上，没有其他测试运行对该域控制器。 运行任何其他 DNS 测试之前，将自动执行连接测试。
- **基本服务：** 测试确认以下服务正在运行并测试的域控制器上可用：DNS 客户端服务、 Net Logon 服务，密钥分发中心 (KDC) 服务和 DNS 服务器服务 （如果在域控制器上安装 DNS）。
- **DNS 客户端配置：** 测试确认所有网络适配器的 DNS 客户端计算机上的 DNS 服务器都是可访问。
- **资源记录注册：** 测试确认至少一个客户端计算机配置的 DNS 服务器上注册的每个域控制器的主机 (A) 资源记录。
- **区域和颁发机构起始 (SOA):** 如果域控制器正在运行的 DNS 服务器服务，测试确认的 Active Directory 域区域和 Active Directory 域区域的授权 (SOA) 资源记录的开始都存在。
- **根区域：** 检查是否存在根 （.） 区域。

成员身份中 Enterprise Admins 或等效身份是完成这些过程所需的最小。

可以使用以下过程来验证 DNS 的基本功能。
     
### <a name="to-verify-basic-dns-functionality"></a>若要验证基本 DNS 功能：


1. 在你想要测试的域控制器或 Active Directory 域服务 (AD DS) 安装了工具的域成员计算机上，以管理员身份打开命令提示符。 若要以管理员身份打开命令提示符，请单击“开始”。 
2. 在“开始搜索”中，键入 ldp。 
3. 在开始菜单的顶部，右键单击命令提示符，然后单击以管理员身份运行。 如果出现“用户帐户控制”对话框，请确认它显示的是所需操作，然后单击“继续”。
4. 在命令提示符处，键入以下命令，然后按 ENTER: `dcdiag /test:dns /v /s:<DCName> /DnsBasic /f:dcdiagreport.txt`
</br></br>替换为实际的可分辨的名称、 NetBIOS 名称或 DNS 名称的域控制器&lt;DCName&gt;。 或者，可以在林中测试的所有域控制器，通过键入而不是 /s: /e:。 /F 开关指定文件名，它在前一个命令为 dcdiagreport.txt。 如果你想要将文件放在当前工作目录之外的某个位置中，可以指定文件路径，例如 /f:c:reportsdcdiagreport.txt。

5. 在记事本或类似的文本编辑器中打开 dcdiagreport.txt 文件。 若要打开该文件在记事本中，在命令提示符处，键入记事本 dcdiagreport.txt，然后按 ENTER。 如果在不同的工作目录中放置该文件，包括文件的路径。 例如，如果该文件置于 c:reports，键入记事本 c:reportsdcdiagreport.txt，然后按 ENTER。
6. 滚动到文件底部附近的摘要表。 
</br></br>记下的所有域控制器名称该报表的摘要表中的"警告"失败"状态。  尝试确定是否没有问题域控制器的详细场专题部分查找的搜索字符串"DC:DCName，"DCName 所在的域控制器的实际名称。

如果您看到所需的明显配置更改，使它们，根据需要。 例如，如果您注意到你的域控制器之一具有显然不正确的 IP 地址，则可以更正它。 然后，重新运行测试。

若要验证的配置更改，重新运行了 /e： 或 /s： 开关，根据需要 Dcdiag /test:DNS /v 命令。 如果您不具备 IP 版本 6 (IPv6) 在域控制器上启用，则应该预料的主机 (AAAA) 验证一部分测试失败，但如果不在网络上使用 IPv6，这些记录不需要。
            
## <a name="verifying-resource-record-registration"></a>验证资源记录注册
    
目标域控制器使用的 DNS 别名 (CNAME) 资源记录来查找其源域控制器复制伙伴。 虽然运行 Windows Server （从 Windows Server 2003 Service Pack 1 (SP1) 开始） 的域控制器可以通过使用完全限定的域名 (Fqdn) 来查找源复制伙伴或如果失败，NetBIOS namesthe 是否存在别名 (CNAME)资源记录，但应为正常运行的正确 DNS 验证。 
      
可以使用以下过程来验证资源记录注册，包括别名 (CNAME) 资源记录注册。
      
### <a name="to-verify-resource-record-registrationtitle"></a>若要验证资源记录注册</title>


1. 以管理员身份打开命令提示符。 若要打开命令提示符下以管理员身份，请单击开始。 在“开始搜索”中，键入 ldp。 
2. 在开始菜单的顶部，右键单击命令提示符，然后单击以管理员身份运行。 如果出现“用户帐户控制”对话框，请确认它显示的是所需操作，然后单击“继续”。  </br></br>可以使用 Dcdiag 工具来验证通过运行所必需的域控制器位置的所有资源记录的注册`dcdiag /test:dns /DnsRecordRegistration`命令。

此命令验证已注册的 DNS 中的以下资源记录：


- **别名 (CNAME):** 全局唯一标识符 (GUID) 的基于查找的复制伙伴的资源记录
- **主机 (A):** 包含域控制器的 IP 地址的主机资源记录
- **LDAP SRV:** 查找 LDAP 服务器的服务 (SRV) 资源记录
- **GC SRV**： 服务 (SRV) 资源记录，找到全局编录服务器
- **PDC SRV**： 找到主域控制器 (PDC) 模拟器操作主机的服务 (SRV) 资源记录

可以使用以下过程来验证别名 (CNAME) 资源记录注册单独。

### <a name="to-verify-alias-cname-resource-record-registration"></a>若要验证别名 (CNAME) 资源记录注册

1. 打开 DNS 管理单元中。 若要打开 DNS，请单击开始。 在开始搜索中，键入 dnsmgmt.msc，，然后按 ENTER。 如果出现用户帐户控制对话框中，确认它显示的操作，并单击继续。
2. 使用 DNS 管理单元中的域控制器正在运行的 DNS 服务器服务，其中服务器主持 Active Directory 域的域控制器与同名的 DNS 区域位置。
3. 在控制台树中，单击名为的 _msdcs 区域。Dns_Domain_Name。
4. 在细节窗格中，验证以下资源记录都存在： 名为 Dsa_Guid._msdcs 一个别名 (CNAME) 资源记录。<placeholder>Dns_Domain_Name</placeholder>和相应托管 DNS 服务器的名称 (A) 资源记录。

如果未注册别名 (CNAME) 资源记录，请验证该动态更新工作正常。 下一节中使用测试验证动态更新。
    
## <a name="verifying-dynamic-update"></a>验证动态更新
    
如果基本 DNS 测试显示资源记录在 DNS 中不存在，请使用动态更新测试来确定为何 Net Logon 服务未注册资源记录自动。 若要验证 Active Directory 域区域配置为接受安全动态更新并执行的测试记录 (_dcdiag_test_record) 注册，请使用以下过程。 测试记录测试后自动删除。

### <a name="to-verify-dynamic-updatetitle"></a>若要验证动态更新</title>


1. 以管理员身份打开命令提示符。 若要打开命令提示符下以管理员身份，请单击开始。 在“开始搜索”中，键入 ldp。 在开始菜单的顶部，右键单击命令提示符，然后单击以管理员身份运行。 如果出现“用户帐户控制”对话框，请确认它显示的是所需操作，然后单击“继续”。
2. 在命令提示符处，键入以下命令，然后按 ENTER: `dcdiag /test:dns /v /s:<DCName> /DnsDynamicUpdate`
   </br></br>替换为可分辨的名称、 NetBIOS 名称或 DNS 名称的域控制器&lt;DCName&gt;。 或者，可以在林中测试的所有域控制器，通过键入而不是 /s: /e:。 如果您不具备在域控制器上启用了 IPv6，则应该预料的主机 (AAAA) 资源记录部分测试失败，这是正常情况时不启用 IPv6。

如果未配置安全动态更新，可以使用以下过程来配置它们。

### <a name="to-enable-secure-dynamic-updates"></a>若要启用安全动态更新


1. 打开 DNS 管理单元中。 若要打开 DNS，请单击开始。 
2. 在开始搜索中，键入 dnsmgmt.msc，，然后按 ENTER。 如果出现用户帐户控制对话框中，确认它显示你想，然后单击继续的操作。
3. 在控制台树中，右键单击适用区域，然后单击属性。
4. 在常规选项卡上，验证区域类型为 Active Directory 集成。
5. 在动态更新中，单击仅安全。

## <a name="registering-dns-resource-records"></a>注册 DNS 资源记录
    
如果 DNS 资源记录未显示在 DNS 中为源域控制器，你已验证的动态更新，和你想要立即注册 DNS 资源记录，您可以通过使用以下过程手动强制注册。 域控制器上的 Net Logon 服务注册所需的域控制器位于网络上的 DNS 资源记录。 DNS 客户端服务注册到的别名 (CNAME) 记录点的主机 (A) 资源记录。

### <a name="to-register-dns-resource-records-manuallytitle"></a>若要手动注册 DNS 资源记录</title>


1. 以管理员身份打开命令提示符。 若要打开命令提示符下以管理员身份，请单击开始。 
2. 在“开始搜索”中，键入 ldp。 
3. 顶部开始，右键单击命令提示符，然后单击以管理员身份运行。 如果出现“用户帐户控制”对话框，请确认它显示的是所需操作，然后单击“继续”。
4. 若要启动的域控制器定位程序资源的注册记录手动在源域控制器上，在命令提示符处，键入以下命令，然后按 ENTER: `net stop netlogon && net start netlogon`
5. 若要启动注册主机 (A) 资源记录手动，在命令提示符处，键入以下命令，然后按 ENTER: `ipconfig /flushdns && ipconfig /registerdns`
6. 在命令提示符处，键入以下命令，然后按 ENTER: `dcdiag /test:dns /v /s:<DCName>` </br></br>替换为可分辨的名称、 NetBIOS 名称或 DNS 名称的域控制器&lt;DCName&gt;。 查看要确保 DNS 测试均通过的测试的输出。 如果您不具备在域控制器上启用了 IPv6，则应该预料的主机 (AAAA) 资源记录部分测试失败，这是正常情况时不启用 IPv6。

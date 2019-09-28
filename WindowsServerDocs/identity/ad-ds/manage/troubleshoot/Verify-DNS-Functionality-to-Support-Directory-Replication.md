---
ms.assetid: 709353b0-b913-4367-8580-44745183e2bc
title: 验证 DNS 功能以支持目录复制
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.service: ''
ms.suite: na
ms.technology: identity-adds
ms.author: joflore
ms.date: 05/31/2017
ms.tgt_pltfrm: na
author: Femila
ms.openlocfilehash: 066f7ebe1cd8f981344e059797daa9d3f86bdf49
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409055"
---
# <a name="verify-dns-functionality-to-support-directory-replication"></a>验证 DNS 功能以支持目录复制

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

 若要检查可能会干扰 Active Directory 复制的域名系统（DNS）设置，你可以先运行基本测试，确保 DNS 为你的域正常运行。 运行基本测试后，可以测试 DNS 功能的其他方面，包括资源记录注册和动态更新。

尽管可以在任何域控制器上运行此基本 DNS 功能测试，但通常在你认为可能遇到复制问题的域控制器上运行此测试，例如，报告事件 Id 1844、1925、2087或的域控制器事件查看器目录服务 DNS 日志中的2088。



## <a name="running-the-domain-controller-basic-dns-testtitle"></a>运行域控制器基本 DNS 测试 @ no__t-0

基本 DNS 测试检查 DNS 功能的以下方面：


- **连接**测试确定域控制器是否已在 DNS 中注册，可以通过<system>ping</system>命令联系到域控制器，并且具有轻型目录访问协议/远程过程调用（LDAP/RPC）连接。 如果域控制器上的连接测试失败，则不会对该域控制器运行其他测试。 在运行任何其他 DNS 测试之前，将自动执行连接测试。
- **基本服务：** 该测试确认以下服务正在运行并且在已测试的域控制器上可用：DNS 客户端服务、Net Logon 服务、密钥发行中心（KDC）服务和 DNS 服务器服务（如果域控制器上安装了 DNS）。
- **DNS 客户端配置：** 该测试确认 DNS 客户端计算机上的所有网络适配器上的 DNS 服务器都是可访问的。
- **资源记录注册：** 该测试确认每个域控制器的主机（A）资源记录已在客户端计算机上配置的至少一个 DNS 服务器上注册。
- **区域和授权机构起始（SOA）：** 如果域控制器运行的是 DNS 服务器服务，则测试确认 Active Directory 域区域的 Active Directory 域区域和授权机构（SOA）资源记录存在。
- **根区域：** 检查根（.）区域是否存在。

Enterprise Admins 中的成员身份或同等身份是完成这些过程所需的最低要求。

你可以使用以下过程来验证基本 DNS 功能。
     
### <a name="to-verify-basic-dns-functionality"></a>验证基本 DNS 功能：


1. 在要测试的域控制器或安装了 Active Directory 域服务（AD DS）工具的域成员计算机上，以管理员身份打开命令提示符。 若要以管理员身份打开命令提示符，请单击“开始”。 
2. 在“开始搜索”中，键入 ldp。 
3. 在 "开始" 菜单的顶部，右键单击 "命令提示符"，然后单击 "以管理员身份运行"。 如果出现“用户帐户控制”对话框，请确认它显示的是所需操作，然后单击“继续”。
4. 在命令提示符下，键入以下命令，然后按 ENTER： `dcdiag /test:dns /v /s:<DCName> /DnsBasic /f:dcdiagreport.txt`
</br></br>替换 &lt;DCName @ no__t 的域控制器的实际可分辨名称、NetBIOS 名称或 DNS 名称。 作为替代方法，你可以通过键入/e：而不是/s：来测试林中的所有域控制器。 /F 开关指定文件名，在上一个命令中为 dcdiagreport。 如果要将文件放在当前工作目录以外的位置，则可以指定文件路径，例如/f： c:reportsdcdiagreport.txt。

5. 在记事本或类似的文本编辑器中打开 dcdiagreport 文件。 若要在记事本中打开该文件，请在命令提示符下键入 Notepad dcdiagreport，然后按 ENTER。 如果将文件放在不同的工作目录中，请包含文件的路径。 例如，如果将文件放在 c:reports 中，请键入 notepad c:reportsdcdiagreport.txt，然后按 ENTER。
6. 滚动到文件底部附近的摘要表。 
</br></br>请注意在摘要表中报告 "警告" 或 "失败" 状态的所有域控制器的名称。  尝试通过搜索字符串 "DC：查找详细的分类部分来确定是否存在问题的域控制器。DCName，其中 DCName 是域控制器的实际名称。

如果你看到所需的配置更改，请根据需要进行更改。 例如，如果你注意到某个域控制器有一个明显不正确的 IP 地址，则可以更正它。 然后重新运行测试。

若要验证配置更改，请根据需要重新运行 Dcdiag/test：带有/e：或/s： switch 的 "DNS/v" 命令。 如果在域控制器上未启用 IP 版本6（IPv6），则应预计测试的主机（AAAA）验证部分失败，但如果不在网络上使用 IPv6，则不需要这些记录。
            
## <a name="verifying-resource-record-registration"></a>验证资源记录注册
    
目标域控制器使用 DNS 别名（CNAME）资源记录定位其源域控制器复制伙伴。 尽管运行 Windows Server 的域控制器（从带有 Service Pack 1 （SP1）的 Windows Server 2003 开始）可以使用完全限定的域名（Fqdn）查找源复制伙伴，如果失败，则会使用别名（CNAME）的 NetBIOS namesthe预期资源记录，并且应该验证其是否正常运行。 
      
你可以使用以下过程来验证资源记录注册，包括别名（CNAME）资源记录注册。
      
### <a name="to-verify-resource-record-registrationtitle"></a>验证资源记录注册 @ no__t-0


1. 以管理员身份打开命令提示符。 若要以管理员身份打开命令提示符，请单击 "启动"。 在“开始搜索”中，键入 ldp。 
2. 在 "开始" 菜单的顶部，右键单击 "命令提示符"，然后单击 "以管理员身份运行"。 如果出现“用户帐户控制”对话框，请确认它显示的是所需操作，然后单击“继续”。  </br></br>通过运行 `dcdiag /test:dns /DnsRecordRegistration` 命令，可以使用 Dcdiag 工具来验证是否注册了域控制器位置所必需的所有资源记录。

此命令验证是否在 DNS 中注册了以下资源记录：


- **别名（CNAME）：** 用于定位复制伙伴的基于全局唯一标识符（GUID）的资源记录
- **主机（A）：** 包含域控制器的 IP 地址的主机资源记录
- **LDAP SRV：** 用于查找 LDAP 服务器的服务（SRV）资源记录
- **GC SRV**：查找全局编录服务器的服务（SRV）资源记录
- **PDC SRV**：查找主域控制器（PDC）仿真器操作主机的服务（SRV）资源记录

你可以使用以下过程来验证别名（CNAME）资源记录注册。

### <a name="to-verify-alias-cname-resource-record-registration"></a>验证别名（CNAME）资源记录注册

1. 打开 DNS 管理单元。 若要打开 DNS，请单击 "启动"。 在 "开始搜索" 中，键入 dnsmgmt.msc，然后按 ENTER。 如果出现 "用户帐户控制" 对话框，请确认它显示了所需的操作，然后单击 "继续"。
2. 使用 DNS 管理单元查找任何运行 DNS 服务器服务的域控制器，其中，服务器承载的 DNS 区域与域控制器的 Active Directory 域相同。
3. 在控制台树中，单击名为 _msdcs 的区域。Dns_Domain_Name.
4. 在详细信息窗格中，验证是否存在以下资源记录：名为 Dsa_Guid. _msdcs 的别名（CNAME）资源记录。<placeholder>Dns_Domain_Name</placeholder>是 Dns 服务器名称的相应主机（a）资源记录。

如果别名（CNAME）资源记录未注册，请验证动态更新是否正常工作。 使用以下部分中的测试来验证动态更新。
    
## <a name="verifying-dynamic-update"></a>验证动态更新
    
如果基本 DNS 测试显示 DNS 中不存在资源记录，则使用动态更新测试来确定 Net Logon 服务未自动注册资源记录的原因。 若要验证 Active Directory 域区域是否配置为接受安全动态更新并执行测试记录（_dcdiag_test_record）的注册，请使用以下过程。 测试记录在测试后会自动删除。

### <a name="to-verify-dynamic-updatetitle"></a>验证动态更新 @ no__t-0


1. 以管理员身份打开命令提示符。 若要以管理员身份打开命令提示符，请单击 "启动"。 在“开始搜索”中，键入 ldp。 在 "开始" 菜单的顶部，右键单击 "命令提示符"，然后单击 "以管理员身份运行"。 如果出现“用户帐户控制”对话框，请确认它显示的是所需操作，然后单击“继续”。
2. 在命令提示符下，键入以下命令，然后按 ENTER： `dcdiag /test:dns /v /s:<DCName> /DnsDynamicUpdate`
   </br></br>将域控制器的可分辨名称、NetBIOS 名称或 DNS 名称替换 &lt;DCName @ no__t-1。 作为替代方法，你可以通过键入/e：而不是/s：来测试林中的所有域控制器。 如果在域控制器上未启用 IPv6，则应预计测试的主机（AAAA）资源记录部分失败，这是未启用 IPv6 时的正常情况。

如果未配置安全动态更新，则可以使用以下过程对其进行配置。

### <a name="to-enable-secure-dynamic-updates"></a>启用安全动态更新


1. 打开 DNS 管理单元。 若要打开 DNS，请单击 "启动"。 
2. 在 "开始搜索" 中，键入 dnsmgmt.msc，然后按 ENTER。 如果出现 "用户帐户控制" 对话框，请确认它显示的是所需操作，然后单击 "继续"。
3. 在控制台树中，右键单击适用的区域，然后单击 "属性"。
4. 在 "常规" 选项卡上，验证区域类型是否 Active Directory 集成。
5. 在 "动态更新" 中，单击 "仅安全"。

## <a name="registering-dns-resource-records"></a>注册 DNS 资源记录
    
如果源域控制器的 dns 资源记录未出现在 DNS 中，你已经验证了动态更新，并且你想要立即注册 DNS 资源记录，你可以使用以下过程手动注册。 域控制器上的 Net Logon 服务将注册域控制器在网络上所需的 DNS 资源记录。 DNS 客户端服务会注册别名（CNAME）记录所指向的主机（A）资源记录。

### <a name="to-register-dns-resource-records-manuallytitle"></a>手动注册 DNS 资源记录 @ no__t-0


1. 以管理员身份打开命令提示符。 若要以管理员身份打开命令提示符，请单击 "启动"。 
2. 在“开始搜索”中，键入 ldp。 
3. 在开头的顶部，右键单击 "命令提示符"，然后单击 "以管理员身份运行"。 如果出现“用户帐户控制”对话框，请确认它显示的是所需操作，然后单击“继续”。
4. 若要在源域控制器上手动启动域控制器定位器资源记录的注册，请在命令提示符下键入以下命令，然后按 ENTER： `net stop netlogon && net start netlogon`
5. 若要手动启动主机（A）资源记录的注册，请在命令提示符下键入以下命令，然后按 ENTER： `ipconfig /flushdns && ipconfig /registerdns`
6. 在命令提示符下，键入以下命令，然后按 ENTER： `dcdiag /test:dns /v /s:<DCName>` </br></br>将域控制器的可分辨名称、NetBIOS 名称或 DNS 名称替换 &lt;DCName @ no__t-1。 查看测试的输出，以确保通过 DNS 测试。 如果在域控制器上未启用 IPv6，则应预计测试的主机（AAAA）资源记录部分失败，这是未启用 IPv6 时的正常情况。

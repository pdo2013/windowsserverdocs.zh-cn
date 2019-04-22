---
ms.assetid: ba7f2b9f-7351-4680-b7d8-a5f270614f1c
title: Active Directory 域服务安装和删除的新功能
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 66455a9ec4eb8a6ff6bfcfa387aeb59acb3ddcc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821348"
---
# <a name="whats-new-in-active-directory-domain-services-installation-and-removal"></a>Active Directory 域服务安装和删除的新功能

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Windows Server 2012 中的 active Directory 域服务 (AD DS) 部署比以前版本的 Windows Server 简单和快速。 AD DS 安装过程现在构建在 Windows PowerShell 上且与服务器管理器集成。 将域控制器引入现有 Active Directory 环境所需的步骤数减少。 这使得创建新 Active Directory 环境的过程更简单、更高效。 新 AD DS 部署过程可将会阻止安装的错误率降到最低。  
  
此外，可以同时在多台服务器上安装 AD DS 服务器角色二进制文件（即 AD DS 服务器角色）。 你还可以在各台服务器上远程运行 AD DS 安装向导。 这些改进提供了更加灵活的部署运行 Windows Server 2012，尤其是对于多个域控制器需要部署到不同区域中的机构的大型全球部署的域控制器。  
  
AD DS 安装包括以下功能：  
  
- **Adprep.exe 集成到 AD DS 安装过程中。** 准备现有 Active Directory 所需的繁琐步骤全已简化或自动执行，如需要使用各种不同凭据、复制 Adprep.exe 文件或登录特定域控制器。 这缩短了安装 AD DS 所需的时间，并减少了会阻止域控制器升级的错误率。  

   对于首选是在新域控制器安装之前运行 adprep.exe 命令的环境，仍可以独立于 AD DS 安装执行 adprep.exe 命令。 Windows Server 2012 版本的 adprep.exe 远程运行，因此可以从运行 64 位版本的 Windows Server 2008 或更高版本的服务器中执行所有必要命令。  

- **新 AD DS 安装构建在 Windows PowerShell 上且可远程调用。** 新 AD DS 安装与服务器管理器集成，因此可使用相同界面安装在安装其他服务器角色时使用的 AD DS。 对于 Windows PowerShell 用户，AD DS 部署 cmdlet 提供更多功能和更大灵活性。 命令行和 GUI 安装选项具有相同的功能。  
- **新 AD DS 安装包括先决条件验证。** 可在安装开始之前发现任何潜在错误。 你可以在出现前就更正错误情况，而无需担心仅部分完成升级。 例如，如果需要运行 adprep /domainprep，安装向导将验证用户是否具有足够权限执行操作。  
- **配置页面按序列分组，镜像最常用升级选项的要求，包含分组到较少向导页面的相关选项。** 这可为进行安装选择提供更好的上下文。  
- **你可以导出一个 Windows PowerShell 脚本，其中包含图形安装期间指定的所有选项的。** 安装或删除结束后，可以将设置导出到 Windows PowerShell 脚本，以用于自动执行相同操作。  
- **仅在重新启动前才进行关键复制。** 允许在重新启动前复制非关键数据的新开关。 有关详细信息，请参阅 [ADDSDeployment cmdlet arguments](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params)。  

## <a name="BKMK_ADConfigurationWizard"></a>Active Directory 域服务配置向导

从 Windows Server 2012 开始，安装域控制器时指定设置的用户界面 (UI) 选项 Active Directory 域服务配置向导取代旧 Active Directory 域服务安装向导。 Active Directory 域服务配置向导在完成添加角色向导后开始。  

> [!WARNING]  
> 旧 Active Directory 域服务安装向导 (dcpromo.exe) 已弃用从 Windows Server 2012 开始。  

在中[安装 Active Directory 域服务&#40;级别 100&#41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)，UI 过程显示如何启动添加角色向导以安装 AD DS 服务器角色二进制文件，然后运行 Active Directory 域服务配置向导完成域控制器安装。 Windows PowerShell 示例显示如何使用 AD DS 部署 cmdlet 完成这两个步骤。  
  
## <a name="BKMK_NewAdprep"></a>Adprep.exe 集成

从 Windows Server 2012 开始，则只有一个版本的 Adprep.exe (无 32 位版本 adprep32.exe)。 根据需要安装到现有 Active Directory 域或林运行 Windows Server 2012 的域控制器时自动运行 Adprep 命令。  
  
尽管 adprep 操作自动运行，但可以单独运行 Adprep.exe。 例如，如果安装 AD DS 的用户不是 Enterprise Admins 组成员，而需要是成员才能运行 Adprep /forestprep，则可能需要单独运行该命令。 但是，只需运行 adprep.exe，如果想要就地升级第一个 Windows Server 2012 域控制器 （换而言之，计划就地升级运行 Windows Server 2012 的域控制器的操作系统）。  
  
Adprep.exe 位于 Windows Server 2012 安装光盘的 \support\adprep 文件夹中。Windows Server 2012 版本的 adprep 可以远程执行。  
  
Windows Server 2012 版本的 adprep.exe 可在运行 64 位版本的 Windows Server 2008 或更高版本任何服务器上运行。 服务器需要建立与林的架构主机和要添加域控制器的域的基础结构主机建立网络连接。 如果在运行 Windows Server 2003 的服务器上托管以下角色之一，则必须远程运行 adprep。 运行 adprep 的服务器不必是域控制器。 它可以加入域或位于工作组中。  

> [!NOTE]  
> 如果尝试在运行 Windows Server 2003 的服务器上运行 Windows Server 2012 版本的 adprep.exe，将出现以下错误：  
>   
> Adprep.exe 不是有效 Win32 应用程序。  

![新增功能](media/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal/AdprepNotValid.gif)  

有关解决 Adprep.exe 返回的其他错误的信息，请参阅 [Known issues](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues)。  

### <a name="group-membership-check-against-windows-server-2003-operations-master-roles"></a>针对 Windows Server 2003 操作主机角色的组成员身份检查

对于每个命令（/forestprep、/domainprep 或 /rodcprep），Adprep 都将执行组成员身份检查，以确定指定凭据是否表示某些组中的帐户。 为了执行此检查，Adprep 将联系操作主机角色所有者。 如果操作主机在运行 Windows Server 2003，而且运行 Adprep.exe 来确保在所有情况下都执行组成员身份检查，则需要指定 /user 和 /userdomain 命令行参数。  
  
/User 和 /userdomain 是 Windows Server 2012 中 Adprep.exe 的新参数。 这些参数分别指定运行 adprep 命令的用户的用户帐户名称和用户域。 Adprep.exe 命令行实用程序阻止指定 /userdomain 和 /user 中的一个，而忽略另一个。  
  
但是，Adprep 操作也可在使用 Windows PowerShell 或服务器管理器安装 AD DS 时运行。 这些体验将相同基础实现 (adprep.dll) 作为 adprep.exe 共享。 Windows PowerShell 和服务器管理器体验具有不同的凭据输入，而不强制 adprep.exe 的相同要求。 使用 Windows PowerShell 或服务器管理器时，可将 /user 而非 /userdomain 的值传递到 adprep.dll。 如果指定 /user 但未指定 /userdomain，本地计算机的域用于执行检查。 如果计算机未加入域，则无法检查组成员身份。  
  
无法检查组成员身份时，Adprep 将在 adprep 日志文件中显示警告消息并继续：  

```
Adprep was unable to check the specified user's group membership. This could happen if the FSMO role owner <DNS host name of operations master> is running Windows Server 2003 or lower version of Windows.  
```

如果运行 Adprep.exe 时未指定 /user 和 /userdomain 参数且操作主机在运行 Windows Server 2003，Adprep.exe 将联系当前登录用户的域中的域控制器。 如果当前登录用户不是域帐户，Adprep.exe 无法执行组成员身份检查。 如果使用智能卡凭据，则即使指定了 /user 和 /userdomain，Adprep.exe 也无法执行组成员身份检查。  
  
如果 Adprep 成功完成，则无需操作。 如果 Adprep 在执行期间失败且出现访问错误，请提供具有正确成员身份的帐户。 有关详细信息，请参阅[运行 Adprep.exe 和安装 Active Directory 域服务的凭据要求](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)。  
  
### <a name="syntax-for-adprep-in-windows-server-2012"></a>Windows Server 2012 中的 Adprep 语法

使用以下语法可独立于 AD DS 安装运行 adprep：  

```
Adprep.exe /forestprep /forest <forest name> /userdomain <user domain name> /user <user name> /password *  
```

在命令中使用 /logdsid，以便生成更详细的日志记录。 adprep.log 位于 %windir%\System32\Debug\Adprep\Logs 中。  

### <a name="running-adprep-using-smartcard"></a>使用智能卡运行 adprep

Windows Server 2012 版本的 adprep.exe 将智能卡用作凭据，但没有简单方法可指定通过命令行的智能卡凭据。 一种方法是通过 PowerShell cmdlet Get-Credential 获取智能卡凭据。 然后使用返回的 PSCredential 对象的用户名（显示为 `@@...`）。 密码是智能卡的 PIN。  

如果指定 /user，则 Adprep.exe 需要 /userdomain。 对于智能卡凭据，/userdomain 应该是智能卡表示的基础用户帐户的域。  

### <a name="adprep-domainprep-gpprep-command-is-not-run-automatically"></a>Adprep /domainprep /gpprep 命令不会自动运行。

adprep /domainprep /gpprep 命令不在 AD DS 安装中运行。 此命令设置策略的结果集 (RSOP) 计划模式功能需要的权限。 有关此命令的详细信息，请参阅 [Microsoft 知识库文章 324392](https://support.microsoft.com/kb/324392)。 如果需要在 Active Directory 域中运行命令，可以独立于 AD DS 安装运行它。 如果已在准备部署运行 Windows Server 2003 SP1 或更高版本的域控制器时运行该命令，则无需再次运行命令。  

您可以安全地添加到现有域而无需运行 adprep /domainprep /gpprep，运行 Windows Server 2012 的域控制器，但 RSOP 计划模式将无法正常工作。  

## <a name="BKMK_PrereqCheck"></a>AD DS 安装先决条件验证

AD DS 安装向导将在安装开始前，检查是否满足以下先决条件。 这可让你可更正可能会阻止安装的问题。  
  
例如，Adprep 相关先决条件包括：  

- Adprep 凭据验证：如果需要运行 adprep，安装向导将验证用户是否具有足够权限执行所需的 Adprep 操作。  
- 架构主机可用性检查：如果安装向导确定需要运行 adprep /forestprep，则将验证架构主机是否处于联机状态，否则将失败。  
- 基础结构主机可用性检查：如果安装向导确定需要运行 adprep /domainprep，则将验证基础结构主机是否处于联机状态，否则将失败。

继承自旧 Active Directory 安装向导 (dcpromo.exe) 的其他先决条件检查包括：  

- 林名称验证：确保林名称有效且当前不存在。  
- NetBIOS 名称验证：检查提供的 NetBIOS 名称是否有效以及是否不与现有名称冲突。  
- 组件路径验证：验证 Active Directory 数据库、日志和 SYSVOL 的路径是否有效，以及是否具有足够可用空间。  
- 子域名验证：确保父域名称和新子域名称有效且不与现有域冲突。  
- 树域名验证：确保指定树名称有效且当前不存在。  

## <a name="BKMK_SystemReqs"></a>系统要求

Windows Server 2012 的系统要求是 Windows Server 2008 R2 相比并无变化。 有关详细信息，请参阅[Windows Server 2008 R2 SP1 系统要求](https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx)(https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx)。  

部分功能可能具有附加要求。 例如，虚拟域控制器克隆功能要求的 PDC 仿真器运行 Windows Server 2012 和安装了 HYPER-V 角色运行 Windows Server 2012 的计算机。  

## <a name="BKMK_KnownIssues"></a>已知的问题

本部分列出了一些会影响 Windows Server 2012 中的 AD DS 安装的已知问题。 有关更多已知问题，请参阅[域控制器部署疑难解答](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md)。  

- 如果在远程运行 adprep /forestprep 时，Windows 防火墙阻止 WMI 访问架构主机，以下错误将记录到位于 %systemroot%\system32\debug\adprep 的 adprep 日志中：  

   ```
   Adprep encountered a Win32 error.
   Error code: 0x6ba Error message: The RPC server is unavailable.  
   ```

   在这种情况下，可通过直接在架构主机上运行 adprep /forestprep 来解决错误，也可以运行下列命令之一以允许 WMI 流量通过 Windows 防火墙。

   对于 Windows Server 2008 或更高版本：

   ```
   netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes
   ```

   对于 Windows Server 2003：

   ```
   netsh firewall set service RemoteAdmin enable
   ```

   adprep 完成后，可以运行下列命令之一再次阻止 WMI 流量：

   ```
   netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=no
   ```

   ```
   netsh firewall set service remoteadmin disable
   ```

- 你可以键入 Ctrl + C 以取消 Install-ADDSForest cmdlet。 取消将停止安装，且对服务器状态所做的任何更改都将还原。 但是，在发出取消命令后，控制权不会返回给 Windows PowerShell，且 cmdlet 会无限期挂起。  
- **如果目标服务器未加入到域之前安装，使用智能卡凭据的其他域控制器的安装将失败。**  

   这种情况下返回的错误消息如下：  

   无法连接到复制源域控制器 *源域控制器名称*。 （例外：Logonfailure：用户名未知或密码错误）  

   如果让目标服务器加入域，然后使用智能卡执行安装，安装成功。  
  
- **ADDSDeployment 模块未在 32 位进程下运行。** 如果要自动执行部署和配置 Windows Server 2012 使用包括 ADDSDeployment cmdlet 和任何其他不支持本机 64 位进程的 cmdlet 的脚本，脚本会失败，出现错误，指示 ADDSDeployment找不到 cmdlet。  

   在这种情况下，需要独立于不支持本机 64 位进程的 cmdlet 运行 ADDSDeployment cmdlet。  

- 名为复原文件系统的 Windows Server 2012 中没有的新文件系统。 请勿将 Active Directory 数据库、日志文件或 SYSVOL 存储到使用恢复文件系统 (ReFS) 格式化的数据卷上。 有关 ReFS 的详细信息，请参阅[针对 Windows 构建下一代文件系统：ReFS](http://blogs.msdn.com/b/b8/archive/2012/01/16/building-the-next-generation-file-system-for-windows-refs.aspx)。  
- 在服务器管理器中，在服务器核心安装上运行 AD DS 或其他服务器角色和已升级到 Windows Server 2012 的服务器的服务器角色可以显示为红色状态，即使按预期方式收集事件和状态也是如此。 运行预发行版本也会影响 Windows Server 2012 的服务器核心安装的服务器。  

### <a name="active-directory-domain-services-installation-hangs-if-an-error-prevents-critical-replication"></a>如果错误阻止关键复制，Active Directory 域服务安装将挂起。

如果 AD DS 安装在关键复制阶段遇到错误，安装会无限期挂起。 例如，如果网络错误阻止关键复制完成，不会继续安装。  
  
如果使用服务器管理器安装，可能会看到安装进度页保持打开状态，但屏幕未报告任何错误，而且进度可能约 15 分钟无更改。 如果使用 Windows PowerShell，Windows PowerShell 窗口中显示的进度将超过 15 分钟无更改。  
  
如果遇到此问题，请检查目标服务器的 %systemroot%\debug 文件夹中的 dcpromo.log 文件。 日志文件通常指明复制重复失败。 此问题的部分已知原因包括：  

- 网络问题阻止要升级的目标服务器和复制源域控制器之间的关键复制。  

   例如，dcpromo.log 显示：  

   ```  
   05/02/2012 14:16:46 [INFO] EVENTLOG (Error): NTDS Replication / DS RPC Client : 1963  
   Internal event: The following local directory service received an exception from a remote procedure call (RPC) connection. Extensive RPC information was requested. This is intermediate information and might not contain a possible cause.  
   Process ID:   
   500  
   Reported error information:  
   Error value:   
   Could not find the domain controller for this domain. (1908)  
   directory service:   
   <domain>.com  
   Extensive error information:  
   Error value:   
   A security package specific error occurred. 1825  
   directory service:   
   <DC Name>  
   ```  

   由于安装过程无限期地重试关键复制，因此在解决基础网络问题后，域控制器安装将继续。 根据需要，使用工具（如 ipconfig、nslookup 和 netmon）调查网络问题。 请确保您要升级的域控制器之间存在连接，在 AD DS 安装过程中选择的复制伙伴。 另外还要确保名称解析正常工作。  

   在安装开始前的先决条件检查期间，验证有关网络连接和名称解析的 AD DS 安装要求。 但在先决条件验证发生之后安装完成之前的时段，也会出现一些错误情况，如安装期间复制伙伴发生故障。  

- 副本域控制器安装期间，将为安装凭据指定目标服务器的本地管理员帐户，且本地管理员帐户的密码将与域管理员帐户的密码匹配。 在这种情况下，可以完成安装向导并开始安装之前遇到"拒绝访问"故障。  

   例如，dcpromo.log 显示：  

   ```  
   03/30/2012 11:36:51 [INFO] Creating the NTDS Settings object for this Active Directory Domain Controller on the remote AD DC DC2.contoso.com...  
   03/30/2012 11:36:51 [INFO] EVENTLOG (Error): NTDS Replication / DS RPC Client : 1963Internal event: The following local directory service received an exception from a remote procedure call (RPC) connection. Extensive RPC information was requested. This is intermediate information and might not contain a possible cause.  
   Process ID:   
   508  
   Reported error information:  
   Error value:   
   Access is denied. (5)  
   directory service:   
   DC2.contoso.com  
   ```  

   如果指定本地管理员帐户和密码导致错误，那么要进行恢复，就需要重新安装操作系统，对无法完成安装的域控制器 [执行帐户元数据清理](https://technet.microsoft.com/library/cc816907(WS.10).aspx) ，然后使用域管理员凭据再次尝试 AD DS 安装。 重启服务器不会纠正该错误情况，因为服务器会标明 AD DS 已安装，即使安装没有成功完成也是如此。  

### <a name="BKMK_nonnormalDNSNameWarning"></a>指定一个非规范化的 DNS 名称时，会发出警告 active Directory 域服务配置向导

如果创建新域或新林且指定的 DNS 域名包括非标准化的国际化字符，则 Active Directory 域服务配置向导将显示警告，指示 DNS 名称查询将失败。 尽管在“部署配置”页指定了 DNS 域名，但在向导后面的“先决条件检查”页上显示警告。  

如果使用未规范化的名称，如 füßball.com 或 ΣΤ.com 指定 DNS 域名 (规范化的版本为： füssball.com 和 β)，尝试访问使用 WinHTTP 的客户端应用程序将调用 Api 的名称解析之前规范化名称。 如果用户在某个对话框中键入"ΣΤ.com"，如"β"和任何 DNS 服务器将其与匹配"ΣΤ.com"的资源记录将发送 DNS 查询。 用户无法解析名称。  

以下示例讲解使用非标准化 IDN 名称时可能出现的问题之一。  

1. 创建数据并使用非规范化名称在域注册 dns 服务器上： füßball.com  
2. 计算机"nps"加入到域和获取注册其名称： nps.füßball.com  
3. 客户端应用程序尝试连接到服务器 nps.füßball.com  
4. 客户端应用程序尝试解析名称 nps.füßball.com 调用 Api 的名称解析。  
5. 由于规范化，名称将转换为 nps.füssball.com 和通过网络以 nps.füßball.com 查询  
6. 客户端应用程序不能解析名称，因为已注册的名称为 nps.füßball.com  

如果在 Active Directory 域服务配置向导的“先决条件检查”页中出现警告，请返回“部署配置”页并指定标准化 DNS 域名。 如果使用 Windows PowerShell 安装新域，请为 -DomainName 选项指定标准化 DNS 名称。  

有关 IDN 的详细信息，请参阅 [处理国际化域名 (IDN)](https://msdn.microsoft.com/library/windows/desktop/dd318142(v=vs.85).aspx)。  

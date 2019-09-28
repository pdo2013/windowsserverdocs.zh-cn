---
title: 使用受保护的构造诊断工具进行故障排除
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 07691d5b-046c-45ea-8570-a0a85c3f2d22
manager: dongill
author: huu
ms.technology: security-guarded-fabric
ms.openlocfilehash: deeaa7eab01dd5da6d997dd6ec039a3319e5c2b7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386463"
---
# <a name="troubleshooting-using-the-guarded-fabric-diagnostic-tool"></a>使用受保护的构造诊断工具进行故障排除

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

本主题介绍如何使用受保护的结构诊断工具来确定和修正受保护的结构基础结构的部署、配置和日常操作中的常见故障。 这包括主机保护者服务（HGS）、所有受保护的主机，以及 DNS 和 Active Directory 等服务。 诊断工具可用于执行对失败的受保护结构的会审中的第一步，为管理员提供解决中断和标识配置错误的资产的起点。 该工具不能取代对受保护的构造进行操作的声音，只是为了快速验证日常操作过程中遇到的最常见问题。

有关本主题中使用的 cmdlet 的文档，请参阅[TechNet](https://technet.microsoft.com/library/mt718834.aspx)。

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

# <a name="quick-start"></a>快速启动

您可以通过使用具有本地管理员权限的 Windows PowerShell 会话调用以下内容来诊断受保护的主机或 HGS 节点：
```PowerShell
Get-HgsTrace -RunDiagnostics -Detailed
```
这会自动检测当前主机的角色，并诊断可以自动检测到的任何相关问题。  在此过程中生成的所有结果将显示，因为存在 `-Detailed` 开关。

本主题的其余部分将详细介绍 `Get-HgsTrace` 的高级用法，例如一次诊断多个主机以及检测复杂的跨节点错误配置。

## <a name="diagnostics-overview"></a>诊断概述
受保护的结构诊断在安装了与虚拟机相关的工具和功能的任何主机上可用，包括运行服务器核心的主机。  目前，诊断包含在以下功能/包中：

1. 主机保护者服务角色
2. 主机保护者 Hyper-V 支持
3. 用于结构管理的 VM 防护工具
4. 远程服务器管理工具 (RSAT)

这意味着，可以在所有受保护的主机、HGS 节点、特定的构造管理服务器以及安装了[RSAT](https://www.microsoft.com/download/details.aspx?id=45520)的任何 Windows 10 工作站上使用诊断工具。  可以从上述任一计算机调用诊断，并在受保护的构造中诊断任何受保护的主机或 HGS 节点;使用远程跟踪目标时，诊断可以查找并连接到运行诊断的计算机以外的其他主机。

诊断目标的每个主机称为 "跟踪目标"。  跟踪目标由其主机名和角色标识。  角色描述给定跟踪目标在受保护的构造中执行的函数。  目前，跟踪目标支持 @no__t 0 和 @no__t 1 角色。  请注意，主机可以同时占用多个角色，诊断也支持这种情况，但不应在生产环境中执行此操作。  HGS 主机和 Hyper-v 主机应始终保持独立和不同。

管理员可以通过运行 @no__t 来开始任何诊断任务。  此命令基于运行时提供的开关执行两个不同的功能：跟踪收集和诊断。  这两者共同构成了受保护的结构诊断工具。  尽管没有明确要求，但最有用的诊断需要跟踪，这些跟踪只能在跟踪目标上通过管理员凭据收集。  如果用户执行跟踪集合所持有的权限不足，则需要提升的跟踪将会失败，而其他的跟踪将会失败。  这允许在有权限的操作员执行会审的情况下进行部分诊断。 

### <a name="trace-collection"></a>跟踪集合
默认情况下，`Get-HgsTrace` 只收集跟踪并将其保存到临时文件夹中。  跟踪采用名为的文件夹的形式，在目标主机后以特殊格式的文件进行填充，这些文件描述了如何配置主机。  跟踪还包含元数据，用于描述如何调用诊断以收集跟踪。  在执行手动诊断时，诊断将使用此数据来解除冻结有关主机的信息。

如有必要，可以手动查看跟踪。  所有格式都可以是用户可读的（XML），也可以使用标准工具（如 X509 证书和 Windows 加密 Shell 扩展）随时进行检查。  但请注意，跟踪不是针对手动诊断设计的，并且通过 `Get-HgsTrace` 的诊断工具处理跟踪始终更为有效。

运行跟踪收集的结果不会指示给定主机的运行状况。  它们只是指示已成功收集跟踪。  需要使用 `Get-HgsTrace` 的诊断工具来确定跟踪是否指示出现故障的环境。

使用 @no__t 参数，你可以将跟踪集合限制为仅限操作指定诊断所需的跟踪。  这会减少收集的数据量，以及调用诊断所需的权限。

### <a name="diagnosis"></a>诊断
可以通过 @no__t 参数 @no__t 提供跟踪的位置，并指定 `-RunDiagnostics` 开关，来诊断收集的跟踪。  此外，`Get-HgsTrace` 可通过提供 @no__t 1 开关和跟踪目标列表在单个传递中执行收集和诊断。  如果未提供任何跟踪目标，则将当前计算机用作隐式目标，其角色通过检查已安装的 Windows PowerShell 模块来推断。

诊断将提供分层格式的结果，显示哪些跟踪目标、诊断集和个别诊断负责特定故障。  如果可以根据接下来应执行的操作进行确定，则故障包括修正和解决建议。  默认情况下，将隐藏传递和不相关的结果。  若要查看诊断测试的所有内容，请指定 `-Detailed` 开关。  这将导致显示所有结果，而不考虑它们的状态。

可以限制使用 `-Diagnostic` 参数运行的诊断集。  这允许您指定应针对跟踪目标运行的诊断类，并禁止所有其他类。  可用诊断类的示例包括网络、最佳做法和客户端硬件。  有关可用诊断的最新列表，请参阅[cmdlet 文档](https://technet.microsoft.com/library/mt718831.aspx)。

> [!WARNING]
> 诊断不能代替强大的监视和事件响应管道。  有一个 System Center Operations Manager 包可用于监视受保护的构造，还提供了各种事件日志通道，可以监视这些通道以便及早检测问题。  然后，可以使用诊断快速诊断这些故障，并建立一系列操作。

## <a name="targeting-diagnostics"></a>面向诊断

`Get-HgsTrace` 对跟踪目标进行操作。  跟踪目标是与某个 HGS 节点或受保护的构造中的受保护主机相对应的对象。  可以将其视为 @no__t 的扩展，其中包括诊断所需的信息，如构造中的主机角色。  目标可以是隐式生成的（例如本地或手动诊断），也可以用 `New-HgsTraceTarget` 命令显式生成。

### <a name="local-diagnosis"></a>本地诊断

默认情况下，`Get-HgsTrace` 将以本地主机（即在其中调用 cmdlet 的位置）为目标。  这称为隐式本地目标。  仅当未在 `-Target` 参数中提供任何目标，并且 `-Path` 中找不到预先存在的跟踪时，才使用隐式本地目标。

隐式本地目标使用角色推理来确定当前主机在受保护的构造中所扮演的角色。  这取决于已安装的 Windows PowerShell 模块，这些模块大致对应于系统上已安装的功能。  如果存在 `HgsServer` 模块，则跟踪目标将角色 @no__t 为-1，并且存在 `HgsClient` 模块将导致跟踪目标将角色 @no__t 为3。  给定主机可以同时出现这两种模块，在这种情况下，它将被视为 @no__t 0 和 @no__t。

因此，默认情况下，诊断在本地收集跟踪：
```PowerShell
Get-HgsTrace
```
...等效于以下内容：
```PowerShell
New-HgsTraceTarget -Local | Get-HgsTrace
```
> [!TIP]
> `Get-HgsTrace` 可通过管道或直接通过 `-Target` 参数接受目标。  这两个操作之间没有区别。

### <a name="remote-diagnosis-using-trace-targets"></a>使用跟踪目标的远程诊断

可以通过使用远程连接信息生成跟踪目标来远程诊断主机。  所有所需的都是主机名和一组可以使用 Windows PowerShell 远程处理进行连接的凭据。
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $server
```
此示例将生成一个提示，收集远程用户凭据，然后使用 `hgs-01.secure.contoso.com` 的远程主机完成跟踪收集时，将运行诊断。  生成的跟踪将下载到 localhost，然后进行诊断。  诊断结果与执行[本地诊断](#local-diagnosis)时的结果相同。  同样，不需要指定角色，因为它可以根据远程系统上安装的 Windows PowerShell 模块进行推断。

远程诊断利用 Windows PowerShell 远程处理来访问远程主机。  因此，跟踪目标必须启用 Windows PowerShell 远程处理（请参阅[Enable enable-psremoting](https://technet.microsoft.com/library/hh849694.aspx)），并且已正确配置 localhost 以启动到目标的连接。

> [!NOTE]
> 在大多数情况下，只需将 localhost 作为同一个 Active Directory 林的一部分，并使用有效的 DNS 主机名。  如果你的环境使用更复杂的联合身份验证模型或者你希望使用直接 IP 地址进行连接，则可能需要执行其他配置，例如设置 WinRM[受信任的主机](https://technet.microsoft.com/library/ff700227.aspx)。

你可以通过使用 @no__t cmdlet 验证跟踪目标是否已正确实例化并配置为接受连接：
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
$server | Test-HgsTraceTarget
```
当且仅当 @no__t 可以与跟踪目标建立远程诊断会话时，此命令将返回 `$True`。  如果失败，此 cmdlet 将返回相关的状态信息，以便进一步排查 Windows PowerShell 远程处理连接问题。

#### <a name="implicit-credentials"></a>隐式凭据

从具有足够权限的用户执行远程诊断以远程连接到跟踪目标时，无需向 @no__t 提供凭据。  当打开连接时，@no__t cmdlet 将自动重复使用调用该 cmdlet 的用户的凭据。

> [!WARNING]
> 某些限制适用于重用凭据，特别是在执行所谓的 "第二跃点" 时。  当尝试将凭据从远程会话内部重新使用到另一台计算机时，会发生这种情况。  需要[设置 CredSSP](https://technet.microsoft.com/library/hh849872.aspx)以支持此方案，但这不在受保护的结构管理和故障排除范围内。

#### <a name="using-windows-powershell-just-enough-administration-jea-and-diagnostics"></a>使用 Windows PowerShell 进行足够的管理（JEA）和诊断

远程诊断支持使用 JEA 约束的 Windows PowerShell 终结点。 默认情况下，远程跟踪目标将使用默认的 `microsoft.powershell` 终结点进行连接。  如果跟踪目标具有 @no__t 的角色，它还将尝试使用在安装 HGS 时配置的 `microsoft.windows.hgs` 终结点。

如果要使用自定义终结点，则在使用 @no__t 参数构造跟踪目标时必须指定会话配置名称，如下所示：

```PowerShell
New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential) -PSSessionConfigurationName "microsoft.windows.hgs"
```

#### <a name="diagnosing-multiple-hosts"></a>诊断多个主机

可以一次将多个跟踪目标传递到 `Get-HgsTrace`。  这包括本地和远程目标的混合。  每个目标将依次跟踪，然后将同时诊断每个目标的跟踪。  诊断工具可以使用部署的更多知识来识别不能检测到的复杂跨节点错误配置。  使用此功能时，只需同时提供多个主机的跟踪（对于手动诊断），或在调用 `Get-HgsTrace` （如果为远程诊断）时以多个主机为目标。

下面的示例使用远程诊断来对由两个 HGS 节点和两个受保护的主机（其中一个受保护的主机用于启动 `Get-HgsTrace`）组成的构造进行会审。

```PowerShell
$hgs01 = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Credential (Enter-Credential)
$hgs02 = New-HgsTraceTarget -HostName "hgs-02.secure.contoso.com" -Credential (Enter-Credential)
$gh01 = New-HgsTraceTarget -Local
$gh02 = New-HgsTraceTarget -HostName "guardedhost-02.contoso.com"
Get-HgsTrace -Target $hgs01,$hgs02,$gh01,$gh02 -RunDiagnostics
```

> [!NOTE]
> 诊断多个节点时，无需诊断整个受保护的构造。  在许多情况下，足以包含可能涉及到给定故障条件的所有节点。  这通常是受保护的主机的一个子集和一个来自 HGS 群集的多个节点。

## <a name="manual-diagnosis-using-saved-traces"></a>使用保存的跟踪进行手动诊断

有时，你可能想要重新运行诊断而不重新收集跟踪，或者你可能不具有用于远程诊断构造中的所有主机的必要凭据。  手动诊断是一种机制，通过该机制，你仍可以使用 `Get-HgsTrace` 执行全结构分类，但不使用远程跟踪集合。

在执行手动诊断之前，你将需要确保构造中将要会审的每个主机的管理员都已准备就绪，并愿意以你的名义执行命令。  诊断跟踪输出不公开通常被视为敏感信息的任何信息，但用户有权确定是否可以安全地向他人公开此信息。

> [!NOTE]
> 跟踪不匿名和显示网络配置、PKI 设置以及有时被视为专用信息的其他配置。  因此，跟踪只应传输到组织内的受信任实体，而永远不会公开发布。

执行手动诊断的步骤如下所示：

1. 请求每个主机管理员运行 `Get-HgsTrace` 指定已知的 `-Path` 和要针对生成的跟踪运行的诊断列表。  例如：

   ```PowerShell
   Get-HgsTrace -Path C:\Traces -Diagnostic Networking,BestPractices
   ```
2. 请求每个主机管理员将生成的跟踪文件夹打包，并将其发送给您。  此过程可通过电子邮件、文件共享或任何其他机制（基于组织建立的操作策略和过程）驱动。

3. 将所有接收的跟踪合并到一个文件夹中，无其他内容或文件夹。

    * 例如，假设你已将管理员从名为 HGS-01、HGS-02、RR1N2608 和 RR1N2608 的四台计算机发送跟踪。  每个管理员都将使用相同的名称向您发送文件夹。  将显示如下所示的目录结构：

      ```
      FabricTraces
      |- HGS-01
      |  |- TargetMetadata.xml
      |  |- Metadata.xml
      |  |- [any other trace files for this host]
      |- HGS-02
      |  |- [...]
      |- RR1N2608-12
      |  |- [...]
      |- RR1N2608-13
         |- [..]
      ```

4. 执行诊断，并在 `-Path` 参数上提供已汇编跟踪文件夹的路径，并指定 @no__t 1 开关以及您要求管理员收集跟踪的诊断信息。  诊断将假定它无法访问在路径中找到的主机，因此将尝试仅使用预先收集的跟踪。  如果任何跟踪丢失或损坏，诊断将仅失败受影响的测试并正常运行。  例如：

   ```PowerShell
   Get-HgsTrace -RunDiagnostics -Diagnostic Networking,BestPractices -Path ".\FabricTraces"
   ```

### <a name="mixing-saved-traces-with-additional-targets"></a>将保存的跟踪与其他目标混合

在某些情况下，你可能会有一组预先收集的跟踪，你希望使用其他主机跟踪来补充这些跟踪。  可以将预先收集的跟踪与其他目标混合，这些目标将在一次诊断调用中进行跟踪和诊断。

按照说明收集并组装上面指定的跟踪文件夹，使用在预收集的跟踪文件夹中找不到的附加跟踪目标调用 `Get-HgsTrace`：

```PowerShell
$hgs03 = New-HgsTraceTarget -HostName "hgs-03.secure.contoso.com" -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $hgs03 -Path .\FabricTraces
``` 

诊断 cmdlet 将标识所有预收集的主机，以及仍需要跟踪并执行必要跟踪的其他主机。  然后，将诊断所有预先收集和刚收集的跟踪的总和。  生成的 trace 文件夹将包含新跟踪和新跟踪。

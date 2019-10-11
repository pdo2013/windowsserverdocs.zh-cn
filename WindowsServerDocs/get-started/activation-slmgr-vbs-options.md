---
title: 用于获取批量激活信息的 Slmgr.vbs 选项
description: 列出可用于 Slmg.vbs 脚本的选项，并说明如何使用它们
TOCTitle: Slmgr.vbs Options
ms.date: 09/24/2019
ms.technology: server-general
ms.topic: article
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
appliesto:
- Windows Server 2012 R2
- Windows 10
- Windows 8.1
ms.openlocfilehash: 3b1ce54fe1e1e855d605aba58a0f0e37f0324469
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963063"
---
# <a name="slmgrvbs-options-for-obtaining-volume-activation-information"></a>用于获取批量激活信息的 Slmgr.vbs 选项

下面介绍了 Slmgr.vbs 脚本的语法，本文的表介绍了每个命令行选项。

```cmd
slmgr.vbs [<ComputerName> [<User> <Password>]] [<Options>]
```

> [!NOTE]
> 在本文中，方括号 \[] 括起来的是可选参数，尖括号 \<> 括起来的是占位符。 键入这些语句时，请省略括号，并使用相应的值替换占位符。

> [!NOTE]
> 有关使用批量激活的其他软件产品的信息，请参阅专门为这些应用程序编写的文档。

## <a name="using-slmgr-on-remote-computers"></a>在远程计算机上使用 Slmgr

若要管理远程客户端，请使用批量激活管理工具 (VAMT) 版本 1.2 或更高版本，或创建意识到平台之间差异的自定义 WMI 脚本。 有关批量激活的 WMI 属性和方法的详细信息，请参阅[批量激活的 WMI 属性和方法](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502536(v=ws.11))。

> [!IMPORTANT]
> 由于 Windows 7 和 Windows Server 2008 R2 中的 WMI 更改，因此 Slmgr.vbs 脚本不适用于跨平台工作。 不支持使用 Slmgr.vbs 从 Windows Vista® 操作系统管理 Windows 7 或 Windows Server 2008 R2 系统。 尝试从 Windows 7 或 Windows Server 2008 R2 中管理较旧系统将生成特定版本不匹配错误。 例如，运行脚本 cscript slmgr.vbs \<vista\_machine\_name\> /dlv 将产生以下输出  ：
>  
>> Microsoft (R) Windows Script Host Version 5.8 版权所有 (C) Microsoft Corporation。 保留所有权利。
>>  
>> 远程计算机不支持此版本的 SLMgr.vbs

## <a name="general-slmgrvbs-options"></a>常规 Slmgr.vbs 选项

|选项 |描述 |
| - | - |
|\[\<ComputerName\>] |远程计算机的名称（默认为本地计算机） |
|\[\<User\>] |具有远程计算机上所需权限的帐户 |
|\[\<Password\>] |具有远程计算机上所需权限的帐户的密码 |

## <a name="global-options"></a>全局选项

|选项 |描述 |
| - | - |
|\/ipk&nbsp;&lt;ProductKey&gt; |尝试安装 5×5 产品密钥。 确认参数提供的产品密钥有效且适用于已安装的操作系统。<br />如果没有，则返回错误。<br />如果密钥有效且适用，则安装密钥。 如果已安装一个密钥，则静默替换它。<br />若要防止许可证服务中的不稳定性，应重新启动系统或软件保护服务。<br />必须从提升的“命令提示符”窗口下运行此操作，或必须将“标准用户操作”注册表值设置为允许非特权的用户额外访问软件保护服务。 |
|/ato&nbsp;\[\<Activation&nbsp;ID\>] |对于安装了 KMS 主机密钥或多个激活密钥 (MAK) 的零售版和卷系统，/ato 提示 Windows 尝试联机激活  。<br />对于安装了通用批量许可证密钥 (GVLK) 的系统，这提示尝试 KMS 激活。 当运行 /ato 时，已设置为挂起自动 KMS 激活尝试 (/stao) 的系统仍然尝试 KMS 激活   。<br />**注意：** 从 Windows 8（和 Windows Server 2012）开始，/stao 选项已弃用  。 请改用 /act-type 选项  。<br />参数 \<Activation ID\> 扩展 /ato 支持，以标识在计算机上安装的 Windows 版本   。 指定 \<Activation ID\> 参数隔离与该激活 ID 相关联版本的选项的影响  。 运行所有 Slmgr.vbs /dlv 以获取已安装版本的 Windows 的激活 ID  。 如果必须支持其他应用程序，请参阅该应用程序提供的指南，以获取进一步说明。<br />KMS 激活不需要提升的权限。 但是，联机激活需要提升，或必须将标准用户操作注册表值设置为允许非特权的用户额外访问软件保护服务。 |
|\/dli&nbsp;\[<Activation&nbsp;ID\>&nbsp;\|&nbsp;All\] |显示许可证信息。<br />默认情况下， **/dli** 显示已安装的活动 Windows 版本的许可证信息。 指定 \<Activation ID\> 参数可显示与该激活 ID 相关联的指定版本的许可证信息  。 将 All 指定为参数将显示所有适用的已安装产品的许可证信息  。<br />此操作不需要提升的权限。 |
|\/dlv&nbsp;\[<Activation&nbsp;ID\>&nbsp;\|&nbsp;All\] |显示详细的许可证信息。<br />默认情况下， **/dlv** 显示已安装操作系统的许可证信息。 指定 \<Activation ID\> 参数可显示与该激活 ID 相关联的指定版本的许可证信息  。 指定 All 参数将显示所有适用的已安装产品的许可证信息  。<br />此操作不需要提升的权限。 |
|\/xpr \[\<Activation&nbsp;ID\>] |显示产品的激活到期日期。 默认情况下，因为 MAK 和零售激活是永久性的，所以这指的是当前 Windows 版本，主要用于 KMS 客户端。<br />指定 \<Activation ID\> 参数可显示与该激活 ID 相关联的指定版本的激活到期日期。此操作不需要提升的权限  。 |

## <a name="advanced-options"></a>高级选项

|选项 |描述 |
| - | - |
|\/cpky |某些服务操作要求产品密钥在全新体验 (OOBE) 操作期间在注册表中可用。 **/cpky** 选项从注册表中删除产品密钥以防止恶意代码盗用此密钥。<br />对于部署密钥的零售安装，最佳做法建议运行此选项。 因为此选项是这些密钥的默认行为，因此 MAK 和 KMS 主机密钥不需要此选项。 此选项仅是默认行为不是从注册表中清除该密钥的其他密钥类型所必需的。<br />必须在提升的“命令提示符”窗口中运行此操作。 |
|\/ilc&nbsp;&lt;license_file&gt; |此选项安装所需的参数指定的许可证文件。 这些许可证可以作为疑难解答措施安装以支持基于令牌的激活，或者作为上架应用程序的手动安装的一部分安装。<br />在此过程中，不会验证许可证：许可证验证超出 Slmgr.vbs 的范围。 相反，在运行时验证由软件保护服务处理。<br />必须从提升的“命令提示符”窗口下运行此操作，或必须将“标准用户操作”注册表值设置为允许非特权的用户额外访问软件保护服务  。 |
|\/rilc |此选项重新安装存储在 %SystemRoot%\system32\oem 和 %SystemRoot%\System32\spp\tokens 中的所有许可证。 这些许可证是在安装过程中存储的“已知正确”副本。<br />将替换受信任应用商店中的任何匹配许可证。 任何其他许可证（例如，受信任的颁发机构 (TA) 颁发许可证 (IL)、应用程序的许可证）不会受到影响。<br />必须在提升的“命令提示符”窗口中运行此操作，或必须将“标准用户操作”注册表值设置为允许非特权的用户额外访问软件保护服务  。 |
|\/rearm |此选项将重置激活计时器。 **/rearm** 过程也称为 **sysprep /generalize**。<br />如果 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform\SkipRearm 注册表项设为 1，则此操作将不执行任何操作   。 请参阅[批量激活的注册表设置](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502532(v=ws.11))详细了解此注册表项。<br />必须在提升的“命令提示符”窗口中运行此操作，或必须将“标准用户操作”注册表值设置为允许非特权的用户额外访问软件保护服务  。 |
|\/rearm-app &lt;Application&nbsp;ID&gt; |重置指定应用的许可状态。 |
|\/rearm-sku &lt;Application&nbsp;ID&gt; |重置指定 SKU 的许可状态。 |
|\/upk&nbsp;\[&lt;Application&nbsp;ID&gt;] |此选项卸载当前 Windows 版本的产品密钥。 重新启动后，系统将处于未经许可的状态下，除非安装了新产品密钥。<br />还可以使用 \<Activation ID\> 参数来指定不同的已安装产品  。<br />必须从提升的“命令提示符”窗口下运行此操作。 |
|\/dti&nbsp;\[\<Activation&nbsp;ID\>] |为脱机激活显示安装 ID。 |
|\/atp &lt;Confirmation&nbsp;ID&gt; |使用用户提供的确认 ID 激活产品。 |

## <a name="kms-client-options"></a>KMS 客户端选项

|选项 |描述 |
| - | - |
|\/skms \<Name\[:Port]&nbsp;\|&nbsp;\:&nbsp;port\> \[\<Activation&nbsp;ID\>] |此选项指定名称，或者要连接的 KMS 主计算机的端口（可选）。 设置此值将禁用 KMS 主机的自动检测。<br />如果 KMS 主机仅使用 Internet 协议版本 6 (IPv6)，必须采用格式 \<hostname\>:\<port\> 指定地址。 IPv6 地址包含冒号 (:)，Slmgr.vbs 脚本会错误地分析它。<br />必须在提升的“命令提示符”窗口中运行此操作。 |
|\/skms-domain&nbsp;&lt;FQDN&gt; \[\<Activation&nbsp;ID\>] |设置特定的 DNS 域，可以在该域中找到所有 KMS SRV 记录。 如果使用 /skms 选项设置特定的单个 KMS 主机，则此设置将不起作用  。 使用此选项（特别是在断开连接的命名空间环境中）强制 KMS 忽略 DNS 后缀搜索列表，并改为在指定的 DNS 域中查找 KMS 主机记录。 |
|\/ckms&nbsp;\[\<Activation&nbsp;ID\>] |此选项从注册表中删除特定的 KMS 主机名、地址和端口信息，并还原 KMS 自动发现行为。<br />必须在提升的“命令提示符”窗口中运行此操作。 |
|\/skhc |此选项启用 KMS 主机缓存（默认）。 客户端发现工作 KMS 主机之后，此设置可防止域名系统 (DNS) 优先级和权重影响与主机的进一步通信。 如果系统无法再联系工作 KMS 主机，客户端将尝试发现新的主机。<br />必须在提升的“命令提示符”窗口中运行此操作。 |
|\/ckhc |此选项禁用 KMS 主机缓存。 此设置指示客户端在每次尝试 KMS 激活（建议在使用优先级和权重时执行此操作）时使用 DNS 自动发现。<br />必须在提升的“命令提示符”窗口中运行此操作。 |

## <a name="kms-host-configuration-options"></a>KMS 主机配置选项

|选项 |描述 |
| - | - |
|\/sai&nbsp;&lt;Interval&gt; |此选项为未激活的客户端设置时间间隔（以分钟为单位），以尝试连接 KMS。 激活时间间隔必须介于 15 分钟到 30 天之间，尽管建议采用默认值（2 小时）也是如此。<br />KMS 客户端最初从注册表中选取此时间间隔，但在接收第一个 KMS 响应后切换到 KMS 设置。<br />必须在提升的“命令提示符”窗口中运行此操作。 |
|\/sri &lt;Interval&gt; |此选项为激活的客户端设置续订时间间隔（以分钟为单位），以尝试连接 KMS。 续订时间间隔必须介于 15 分钟到 30 天之间。 KMS 服务器和客户端上最初都设置了此选项。 默认值为 10,080 分钟（7 天）。<br />KMS 客户端最初将从注册表中拾取此间隔，但在收到第一个 KMS 响应后将切换到 KMS 设置。<br />必须在提升的“命令提示符”窗口中运行此操作。 |
|\/sprt&nbsp;&lt;Port&gt; |此选项设置在其上侦听客户端激活请求的 KMS 主机的端口。 默认的 TCP 端口为 1688。<br />必须从提升的“命令提示符”窗口下运行此操作。 |
|\/sdns |通过 KMS 主机（默认）启用 DNS 发布。<br />必须在提升的“命令提示符”窗口中运行此操作。 |
|\/cdns |通过 KMS 主机禁用 DNS 发布。<br />必须在提升的“命令提示符”窗口中运行此操作。 |
|\/spri |将 KMS 优先级设置为正常（默认）。<br />必须在提升的“命令提示符”窗口中运行此操作。 |
|\/cpri |将 KMS 优先级设置为低。<br />使用此选项以最大程度地减少来自共同托管环境中的 KMS 的争用。 请注意这可能导致 KMS 匮乏，具体取决于哪些其他应用程序或服务器角色处于活动状态。 谨慎使用。<br />必须在提升的“命令提示符”窗口中运行此操作。 |
|\/act-type \[\<Activation-Type\>] \[\<Activation&nbsp;ID\>] |此选项将限制批量激活的注册表中的值设置为单个类型。 激活类型 **1** 仅将激活限制为 Active Directory；**2** 将其限制为 KMS 激活；**3** 将其限制为基于令牌的激活。 **0** 选项允许任何激活类型，并为默认值。 |

## <a name="token-based-activation-configuration-options"></a>基于令牌的激活配置选项

|选项 |描述 |
| - | - |
|/lil |列出已安装的基于令牌的激活颁发许可证。 |
|\/ril&nbsp;&lt;ILID&gt;&nbsp;&lt;ILvID&gt; |删除已安装的基于令牌的激活颁发许可证。<br />必须从提升的“命令提示符”窗口下运行此操作。 |
|\/stao |设置 **Token-based Activation Only** 标志，从而禁用自动 KMS 激活。<br />必须在提升的“命令提示符”窗口中运行此操作。<br />已在 Windows Server 2012 R2 和 Windows 8.1 中删除此选项。 请改用 **/act–type** 选项。 |
|\/ctao |清除**仅基于令牌的激活** 标志（默认），从而启用自动 KMS 激活。<br />必须在提升的“命令提示符”窗口中运行此操作。<br />已在 Windows Server 2012 R2 和 Windows 8.1 中删除此选项。 请改用 /act-type</strong> 选项  。 |
|\/ltc |列出基于令牌的有效激活证书，这些证书可激活已安装的软件。 |
|\/fta &lt;Certificate&nbsp;Thumbprint&gt; \[&lt;PIN&gt;] |使用标识的证书强制执行基于令牌的激活。 如果使用受硬件（例如智能卡）保护的证书，提供可选个人标识号 (PIN) 以解锁私钥，无需 PIN 提示。 |

## <a name="active-directory-based-activation-configuration-options"></a>基于 Active Directory 的激活配置选项

|选项 |描述 |
| - | - |
|\/ad-activation-online &lt;Product&nbsp;Key&gt; \[\<Activation&nbsp;Object&nbsp;name\>] |使用命令提示符正在运行的凭据收集 Active Directory 数据和启动 Active Directory 林激活。 无需本地管理员访问权限。 但需要对该林的根域中的激活对象容器的读/写权限。 |
|\/ad-activation-get-IID &lt;Product&nbsp;Key&gt; |此选项在手机模式下启动 Active Directory 林激活。 输出内容是安装 ID (IID)，可用于在 Internet 连接不可用时通过电话激活该林。 在激活电话呼叫中提供 IID 后，将返回用于完成激活的 CID。 |
|\/ad-activation-apply-cid &lt;Product&nbsp;Key&gt; &lt;Confirmation&nbsp;ID&gt; \[\<Activation&nbsp;Object&nbsp;name>] |使用此选项时，输入激活电话呼叫中提供的 CID 以完成激活 |
|\[/name:&lt;AO_Name&gt;] |或者，你可以将 **/name** 选项附加到其中任何命令，以为存储在 Active Directory 中的激活对象指定名称。 名称不能超过 40 个 Unicode 字符。 使用双引号显式定义名称字符串。<br />在 Windows Server 2012 R2 和 Windows 8.1 中，可以将该名称直接附加在 /ad-activation-online &lt;Product&nbsp;Key&gt; 和 /ad-activation-apply-cid 之后，而无需使用 /name 选项    。 |
|\/ao-list |显示可用于本地计算机的所有激活对象。 |
|\/del-ao &lt;AO_DN&gt;<br />\/del-ao &lt;AO_RDN&gt; |从林中删除指定的激活对象。 |

## <a name="see-also"></a>另请参阅

- [批量激活技术参考](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502529%28v%3dws.11%29)
- [批量激活概述](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831612%28v%3dws.11%29)


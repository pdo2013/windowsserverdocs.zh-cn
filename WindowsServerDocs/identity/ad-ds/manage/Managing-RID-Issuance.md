---
ms.assetid: aac117a7-aa7a-4322-96ae-e3cc22ada036
title: 管理 RID 颁发
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: dd265fecce06b849bd14d4d6b81503aba7311656
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390071"
---
# <a name="managing-rid-issuance"></a>管理 RID 颁发

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题介绍对 RID 主机 FSMO 角色的更改，包括 RID 主机中新的颁发和监视功能以及如何分析和解决 RID 颁发问题。  
  
-   [管理 RID 颁发](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Manage)  
  
-   [消除 RID 颁发问题](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Tshoot)  
  
有关详细信息，请参阅[AskDS 博客](http://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx)。  
  
## <a name="BKMK_Manage"></a>管理 RID 颁发  
默认情况下，一个域具有大约十亿个安全主体（例如用户、组和计算机）的容量。 当然，没有哪个域具有这么多经常使用的对象。 但是，Microsoft 客户支持发现了如下案例：  
  
-   设置软件或管理脚本时意外地大量创建了用户、组和计算机。  
  
-   委派的用户创建了许多未使用的安全和通讯组  
  
-   许多域控制器已降级、还原或清除元数据  
  
-   执行了林恢复  
  
-   已经常执行 InvalidateRidPool 操作  
  
-   错误地增加了 RID 块大小注册表值  
  
所有这些情况不必要地用尽 RID（经常是出于错误操作）。 许多年来，少许环境用尽了 RID，这迫使它们迁移到新的域或执行林恢复。  
  
Windows Server 2012 可处理 RID 分配问题，这些问题只有在 Active Directory 使用时间增长和覆盖面逐渐变广时才会体现出来。 其中包括更好的事件日志记录、更合适的限制以及在紧急情况下紧急地消除域的全局 RID 空间整体大小的能力。  
  
### <a name="periodic-consumption-warnings"></a>定期使用情况警告  
Windows Server 2012 添加了全局 RID 空间事件跟踪，可在超过重要里程碑时提供早期警告。 模型计算全局池中百分之十 (10) 的已使用标记并在达到时记录事件。 然后它计算剩余部分的下一个已使用的百分之十，该事件循环将继续进行。 当全局 RID 空间耗尽时，由于在递减的池中越来越快地到达百分之十，事件将加速（但是事件记录抑制将阻止每小时生成一个条目以上）。 每个域控制器上的系统事件日志都将编写 Directory-Services-SAM 警告事件 16658。  
  
假定对于默认的 30 位全局 RID 空间，在分配包含第 107,374,182<sup></sup> 个 RID 的池时记录第一批事件日志。 事件速率自然会加快，直到最后一个 100,000 的检查点，总共生成 110 个事件。 解锁的 31 位全局 RID 空间也有类似行为：在 214,748,365 开始，在 117 个事件中结束。  
  
> [!IMPORTANT]  
> 此事件不在预料之中；立即调查域中的用户、计算机和组的创建过程。 创建多于 1 亿个 AD DS 对象非常不正常。  
  
![RID 颁发](media/Managing-RID-Issuance/ADDS_RID_TR_EventWaypoints2.png)  
  
### <a name="rid-pool-invalidation-events"></a>RID 池验证事件  
存在关于本地 DC RID 池已弃用的新事件警报。 这些警报是信息性警报，而且可以预期，尤其是因为有了新的 VDC 功能。 有关事件的详细信息，请参阅下面的事件列表。  
  
### <a name="BKMK_RIDBlockMaxSize"></a>RID 块大小限制  
正常情况下，域控制器一次请求 500 个 RID 的块的 RID 分配。 你可以在域控制器上使用以下注册表 REG_DWORD 值替代默认值：  
  
```  
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\RID Values  
RID Block Size  
  
```  
  
在 Windows Server 2012 之前，该注册表项中未强制执行最大值，隐式 DWORD 最大值除外（值为 0xffffffff 或 4294967295）。 此值远远大于总的全局 RID 空间。 管理员有时不当或意外地配置 RID 块大小，它的值会以极快耗尽全局 RID。  
  
在 Windows Server 2012 中，你无法将此注册表值设置为高于十进制的 15,000（十六进制的 0x3A98）。 这阻止了大规模非预期的 RID 分配。  
  
如果你将该值设置为*高于* 15,000，则该值将被视为 15,000，而且域控制器在每次重新启动时在目录服务事件日志中记录事件 16653，直到修正该值为止。  
  
### <a name="BKMK_GlobalRidSpaceUnlock"></a>全局 RID 空间大小解锁  
在 Windows Server 2012 之前，全局 RID 空间曾限制为共 2<sup>30</sup> 个（或 1,073,741,823）RID。 一旦达到该值，只有将域迁移或将林恢复到较早的时间范围，才能允许创建新的 SID - 即灾难恢复，可采用任何衡量标准。 从 Windows Server 2012 开始，可以解锁 2<sup>31</sup> 位以将全局池增加为 2,147,483,648 个 RID。  
  
AD DS 在所有域控制器的 RootDSE 上下文中名为 **SidCompatibilityVersion** 的特殊隐藏属性中存储此设置。 此属性不可使用 ADSIEdit、LDP 或其他工具读取。 若要查看全局 RID 空间的增加，请从 Directory-Services-SAM 或使用以下 Dcdiag 命令检查系统事件日志中的警告事件 16655：  
  
```  
Dcdiag.exe /TEST:RidManager /v | find /i "Available RID Pool for the Domain"  
  
```  
  
如果增加全局 RID 池，可用池将更改为 2,147,483,647，而不是默认的 1,073,741,823。 例如：  
  
![RID 颁发](media/Managing-RID-Issuance/ADDS_RID_TR_Dcdiag.png)  
  
> [!WARNING]  
> 此解锁*仅*旨在防止用尽 RID，并且*仅*与 RID 上限强制措施结合使用（请参阅下一部分）。 不要在有数百万剩余 RID 且低增长的环境中“提前”进行此设置，因为解锁的 RID 池生成的 SID 潜在存在着应用程序兼容性问题。  
>   
> 除了将整个林恢复到较早的备份，此解锁操作无法还原或删除。  
  
#### <a name="important-caveats"></a>重要注意事项  
当全局 RID 池第 31<sup></sup> 位已解锁时，Windows Server 2003 和 Windows Server 2008 域控制器无法颁发 RID。 Windows Server 2008 R2 域控制器*可以*<sup>使用31位</sup>rid，*但*前提是它们安装了修补程序[KB 2642658](https://support.microsoft.com/kb/2642658) 。 当解锁时，不受支持和未安装修补程序的域控制器将全局 RID 池视为已耗尽。  
  
此功能不由任何域功能级别强制执行；请格外注意，域中仅存在 Windows Server 2012 或更新的 Windows Server 2008 R2 域控制器。  
  
#### <a name="implementing-unlocked-global-rid-space"></a>实现解锁的全局 RID 空间  
若要在收到 RID 上限警报（见下文）后将 RID 池解锁到第 31<sup></sup> 位，请执行以下步骤：  
  
1.  确保 RID 主机角色在 Windows Server 2012 域控制器上运行。 如果未运行，将其转移到 Windows Server 2012 域控制器。  
  
2.  运行 LDP.exe  
  
3.  单击“连接”菜单并为端口 389 上的 Windows Server 2012 RID 主机单击“连接”，然后以域管理员身份单击“绑定”。  
  
4.  单击“浏览”菜单并单击“修改”。  
  
5.  确保“DN”为空。  
  
6.  在 "**编辑项属性**" 中键入：  
  
    ```  
    SidCompatibilityVersion  
    ```  
  
7.  在“值”中，键入：  
  
    ```  
    1  
    ```  
  
8.  确保在“操作”中选中“添加”并单击“输入”。 这将更新“条目列表”。  
  
9. 选择“同步”和“已扩展”选项，然后单击“运行”。  
  
    ![RID 颁发](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModify.png)  
  
10. 如果成功，则 LDP 输出窗口显示：  
  
    ```  
    ***Call Modify...  
     ldap_modify_ext_s(Id, '(null)',[1] attrs, SvrCtrls, ClntCtrls);  
    modified "".  
  
    ```  
  
    ![RID 颁发](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModifySuccess.png)  
  
11. 通过在该域控制器上检查系统事件日志中的 Directory-Services-SAM 信息事件 16655 来确认全局 RID 池已增加。  
  
### <a name="rid-ceiling-enforcement"></a>RID 上限强制措施  
为了提供保护的度量并评估管理感知程度，Windows Server 2012 在全局 RID 范围上引入了人工上限，此上限为全局空间中剩余 RID 的百分之十 (10)。 在人工上限的百分之一 (1) 内，请求 RID 池的域控制器将 Directory-Services-SAM 警告事件 16656 写入其系统事件日志。 当到达 RID 主机 FSMO 上的百分之十上限时，它将 Directory-Services-SAM 事件 16657 写入其系统事件日志，并且不会继续分配任何 RID 池，直到重写上限为止。 这将强制你访问域中 RID 主机的状态并处理潜在的失控 RID 分配；这还可防止域耗尽整个 RID 空间。  
  
此上限硬编码在可用 RID 空间的剩余百分之十处。 即，当 RID 主机分配包含对应全局 RID 空间百分之九十 (90) 的 RID 的池时，此上限将激活。  
  
-   对于默认域，第一个触发点是 2<sup>30</sup>-1 * 0.90 = 966,367,640（或剩余 107,374,183 个 RID）。  
  
-   对于带有解锁的 31 位 RID 空间的域，触发点是 2<sup>31</sup>-1 * 0.90 = 1,932,735,282 个 RID（或剩余 214,748,365 个 RID）。  
  
触发时，RID 主机在对象上将 Active Directory 属性“msDS-RIDPoolAllocationEnabled”（常用名“ms-DS-RID-Pool-Allocation-Enabled”）设置为 FALSE：  
  
CN = RID Manager $，CN = System，DC = *<domain>*  
  
这将写入 16657 事件并防止继续将 RID 颁发到所有域控制器。 域控制器继续消耗任何已颁发给它们的未完成的 RID 池。  
  
要删除该块并允许继续分配 RID 池，请将该值设置为 TRUE。 在 RID 主机执行的下一个 RID 分配上，该属性将返回到它默认的 NOT SET 值。 在此之后，没有进一步的上限，而且最终全局 RID 空间将用尽，需要进行林恢复或域迁移。  
  
#### <a name="removing-the-ceiling-block"></a>删除上限块  
若要在到达人工上限后删除该块，请执行以下步骤：  
  
1.  确保 RID 主机角色在 Windows Server 2012 域控制器上运行。 如果未运行，将其转移到 Windows Server 2012 域控制器。  
  
2.  运行 LDP.exe。  
  
3.  单击“连接”菜单并为端口 389 上的 Windows Server 2012 RID 主机单击“连接”，然后以域管理员身份单击“绑定”。  
  
4.  单击“视图”菜单并单击“树”，然后为“基 DN”选择 RID 主机自己的域命名上下文。 单击“确定”。  
  
5.  在导航窗格中，向下钻取“CN=System”容器并单击“CN=RID Manager$”对象。 右键单击它并单击“修改”。  
  
6.  在编辑条目属性中，键入：  
  
    ```  
    MsDS-RidPoolAllocationEnabled  
    ```  
  
7.  在“值”中，键入（采用大写）：  
  
    ```  
    TRUE  
    ```  
  
8.  在“操作”中选择“替换”并单击“输入”。 这将更新“条目列表”。  
  
9. 启用“同步”和“已扩展”选项，然后单击“运行”：  
  
    ![RID 颁发](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeiling.png)  
  
10. 如果成功，则 LDP 输出窗口显示：  
  
    ```  
    ***Call Modify...  
    ldap_modify_ext_s(ld, 'CN=RID Manager$,CN=System,DC=<domain>',[1] attrs, SvrCtrls, ClntCtrls);  
    Modified "CN=RID Manager$,CN=System,DC=<domain>".  
  
    ```  
  
    ![RID 颁发](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeilingSuccess.png)  
  
### <a name="other-rid-fixes"></a>其他 RID 修补程序  
以前的 Windows Server 操作系统在缺少 rIDSetReferences 属性时有 RID 池泄露问题。 若要在运行 Windows Server 2008 R2 的域控制器上解决此问题，请从[KB 2618669](https://support.microsoft.com/kb/2618669)安装修补程序。  
  
### <a name="unfixed-rid-issues"></a>未解决的 RID 问题  
在历史上，帐户创建失败时会发生 RID 泄露；在创建帐户时，虽然失败，但仍将用尽一个 RID。 常见示例是使用不符合复杂性的密码创建一个用户。  
  
### <a name="rid-fixes-for-earlier-versions-of-windows-server"></a>早期版本 Windows Server 的 RID 修补程序  
上述所有修补程序和更改均已发布 Windows Server 2008 R2 修补程序。 当前没有已计划或进行中的 Windows Server 2008 修补程序。  
  
## <a name="BKMK_Tshoot"></a>消除 RID 颁发问题  
  
### <a name="introduction-to-troubleshooting"></a>疑难解答简介  
RID 颁发疑难解答需要逻辑和线性方法。 除非你正在仔细监视事件日志中 RID 触发的警告和错误，否则你对第一个问题的指示很可能是帐户创建失败。 解决 RID 颁发问题的关键是了解该症状何时符合预期或何时不符合预期；许多 RID 颁发问题可能仅影响一个域控制器，而且与组件改进无关。 下面的这个简图有助于使这些决策更清晰：  
  
![RID 颁发](media/Managing-RID-Issuance/adds_rid_issuance_troubleshooting.png)  
  
### <a name="troubleshooting-options"></a>疑难解答选项  
  
#### <a name="logging-options"></a>日志记录选项  
RID 颁发中的所有日志记录均发生在系统事件日志中的源 Directory-Services-SAM 下。 默认情况下，日志记录处于启用状态并配置为最大详细级别。 如果未针对 Windows Server 2012 中的新组件更改记录任何条目，请将该问题视为典型（即旧，早于 Windows Server 2012） RID 颁发问题，此问题在 Windows 2008 R2 或更早的操作系统中出现。  
  
#### <a name="utilities-and-commands-for-troubleshooting"></a>用于疑难解答的实用工具和命令  
若要解决上述日志未解释的问题（尤其是较早版本的 RID 颁发问题），请使用以下工具列表开始着手：  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Network Monitor 3.4  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>域控制器配置疑难解答的常规方法  
  
1.  该错误是否由简单的权限或域控制器可用性问题导致？  
  
    1.  你是否尝试创建没有必需权限的安全主体？ 检查输出中的拒绝访问错误。  
  
    2.  域控制器是否可用？ 检查返回的错误或 LDAP 或域控制器可用性消息。  
  
2.  返回的错误是否专门提及 RID，而且具体到可以作为指南使用？ 如果是这样，请按该指南进行操作。  
  
3.  返回的错误是否专门提及 RID，但除此之外并不具体？ 例如，“Windows 无法创建对象，因为目录服务无法分配相对标识符。”  
  
    1.  检查域控制器上的系统事件日志中是否2012有[Rid 池请求](https://technet.microsoft.com/library/ee406152(WS.10).aspx)中详细信息（16642、16643、16644、16645、16656）。  
  
    2.  检查域控制器和 RID 主机上的系统事件中新的块指示事件，它们在本主题下文中有详细说明（16655、16656、16657）。  
  
    3.  使用 Repadmin.exe 验证 Active Directory 复制运行状况，使用“Dcdiag.exe /test:ridmanager /v”验证 RID 主机可用性。 如果这些测试不能获得确定的结果，请在域控制器和 RID 主机之间启用双向网络捕获。  
  
### <a name="troubleshooting-specific-problems"></a>有关特定问题的疑难解答  
Windows Server 2012 域控制器上的系统事件日志中记录以下新消息。 自动 AD 运行状况跟踪系统（例如 System Center Operations Manager）应当监视这些事件；全部都值得注意，有些可指示关键的域问题。  
  
|||  
|-|-|  
|事件 ID|16653|  
|Source|Directory-Services-SAM|  
|severity|警告|  
|Message|管理员所配置的帐户标识符 (RID) 的池大小大于支持的最大值。 当域控制器是 RID 主机时，将使用 %1 的最大值。<br /><br />有关详细信息，请参阅 [RID 块大小限制](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_RIDBlockMaxSize)。|  
|注释和解析|RID 块大小的最大值现在是十进制的 15000（十六进制的 3A98）。 域控制器无法请求多于 15,000 个 RID。 在每次启动时记录此事件，直到将该值设置为等于或小于此最大值。|  
  
|||  
|-|-|  
|事件 ID|16654|  
|Source|Directory-Services-SAM|  
|severity|信息性|  
|Message|帐户标识符 (RID) 池已失效。 在以下预期情况下，可能会出现该问题：<br /><br />1.通过备份还原域控制器。<br /><br />2.通过快照还原在虚拟机上运行的域控制器。<br /><br />3.管理员已手动使池失效。<br /><br />有关详细信息，请参阅 https://go.microsoft.com/fwlink/?LinkId=226247 。|  
|注释和解析|如果此事件是意外，请联系所有域管理员并确定他们中谁执行了该操作。 目录服务事件记录还包含有关何时执行这些步骤之一的详细信息。|  
  
|||  
|-|-|  
|事件 ID|16655|  
|Source|Directory-Services-SAM|  
|severity|信息性|  
|Message|帐户标识符 (RID) 的全局最大值已增加为 %1。|  
|注释和解析|如果此事件是意外，请联系所有域管理员并确定他们中谁执行了该操作。 此事件说明总体 RID 池大小的增加超过 2<sup>30</sup> 的默认值，而且不会自动发生；仅通过管理操作发生。|  
  
|||  
|-|-|  
|事件 ID|16656|  
|Source|Directory-Services-SAM|  
|severity|警告|  
|Message|帐户标识符 (RID) 的全局最大值已增加为 %1。|  
|注释和解析|需要执行的操作！ 帐户标识符 (RID) 池已分配到此域控制器。 池值指示此域已使用了总可用帐户标识符中相当大的一部分。<br /><br />当域达到以下阈值时，将会激活保护机制：剩余可用帐户标识符总计：% 1。  保护机制将阻止帐户创建，直到你在 RID 主机域控制器上手动重新启用帐户标识符分配。<br /><br />有关详细信息，请参阅 https://go.microsoft.com/fwlink/?LinkId=228610 。|  
  
|||  
|-|-|  
|事件 ID|16657|  
|Source|Directory-Services-SAM|  
|severity|Error|  
|Message|需要执行的操作！ 此域已使用了总可用帐户标识符 (RID) 中相当大的一部分。 已激活保护机制，因为剩余的总可用帐户标识符小于：X% [artificial ceiling argument]。<br /><br />保护机制阻止帐户创建，直到你在 RID 主机域控制器上手动重新启用帐户标识符分配。<br /><br />在重新启用帐户创建之前执行特定的诊断非常重要，这可以确保此域不会以异常高的速率消耗帐户标识符。 任何标识的问题应该在重新启用帐户创建之前解决。<br /><br />如果未能诊断并修复导致异常高的帐户标识符消耗率的任何基本问题，可能会导致域中的帐户标识符耗尽，然后帐户创建将在此域中永久性地处于禁用状态。<br /><br />有关详细信息，请参阅 https://go.microsoft.com/fwlink/?LinkId=228610 。|  
|注释和解析|联系所有域管理员并通知他们此域中无法再创建任何安全主体，直到重写此保护为止。 有关如何重写保护以及增加总体 RID 池（可能执行）的详细信息，请参阅[全局 RID 空间大小解锁](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_GlobalRidSpaceUnlock)。|  
  
|||  
|-|-|  
|事件 ID|16658|  
|Source|Directory-Services-SAM|  
|severity|警告|  
|Message|此事件是对可用帐户标识符 (RID) 剩余总量的定期更新。 剩余的帐户标识符数大约为：% 1。<br /><br />随着帐户的创建，将使用帐户标识符，当它们耗尽时，域中不可创建新帐户。<br /><br />有关详细信息，请参阅 https://go.microsoft.com/fwlink/?LinkId=228745 。|  
|注释和解析|联系所有域管理员并通知他们 RID 消耗已越过重要里程碑；通过查看安全信任项创建模式来确定这是否为预期的行为。 此事件非常罕见，因为它意味着已分配至少约 1 亿个 RID。|  
  
## <a name="see-also"></a>请参阅  
[在 Windows Server 2012 中管理 RID 颁发](http://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx)  
  



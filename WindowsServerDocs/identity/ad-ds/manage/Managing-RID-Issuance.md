---
ms.assetid: aac117a7-aa7a-4322-96ae-e3cc22ada036
title: "管理 RID 颁发"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f84bcc1aa32e9993903e094fc43feffcbe16a05b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="managing-rid-issuance"></a>管理 RID 颁发

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍更改 RID 主机 FSMO 角色，包括新颁发和监控中 RID 主机以及如何分析和解决 RID 颁发的功能的方法。  
  
-   [管理 RID 颁发](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Manage)  
  
-   [故障排除 RID 颁发](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Tshoot)  
  
提供了详细信息[AskDS 博客](http://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx)。  
  
## <a name="BKMK_Manage"></a>管理 RID 颁发  
默认情况下，域有大约一亿安全原则，如用户、 组和计算机的容量。 当然，有很多经常使用对象具有没有域。 但是，Microsoft 客户支持发现了情况下位置：  
  
-   预配软件或意外批量管理脚本创建用户、 组和计算机。  
  
-   许多未使用的安全和 distribution 组委派的用户创建  
  
-   许多域控制器已降级还原，或清除元数据  
  
-   森林恢复执行  
  
-   InvalidateRidPool 操作频繁  
  
-   删除阻止大小注册表值增加错误  
  
所有这些情况下使用向上 Rid 必要、 通常误。 多年，Rid 退出的一些环境运行，并且此强制它们来迁移到新的域，或者可以执行森林恢复。  
  
Windows Server 2012 解决 RID 分配仅已成为年龄和 Active directory 的领域与有问题的问题。 其中包括更好的事件日志记录、 更合适的限制，并能够到-在紧急情况下的双域的全球 RID 空间的总大小。  
  
### <a name="periodic-consumption-warnings"></a>定期消耗警告  
Windows Server 2012 添加时主要里程碑经过全球 RID 空间事件跟踪，将提供早期警告。 模型计算十 （10） %用于全球池标记，并将记录事件达到时。 然后它计算接下来的 10%剩余使用，并继续事件周期。 当用尽全球 RID 空间时，事件将更快作为 %到 10%人气节目更快地在衰减池中 （但事件日志抑制将阻止每小时的多个输入）。 每个域控制器上的系统事件日志写入目录-服务三千警告事件 16658。  
  
假设默认 30 位全球 RID 空间，分配包含 107,374,182 池时，第一事件日志<sup>日</sup>RID。 事件率加速自然之前 100000，上次检查点了生成总共 110 事件。 行为是解锁 31 位全球 RID 空间相似的： 214,748,365 在启动和 117 事件在完成。  
  
> [!IMPORTANT]  
> 不应此类事件;调查用户、 计算机和立即域中的组创建流程。 创建超过 100 万广告 DS 对象是非常利用普通。  
  
![删除的颁发](media/Managing-RID-Issuance/ADDS_RID_TR_EventWaypoints2.png)  
  
### <a name="rid-pool-invalidation-events"></a>删除池失效事件  
将提供崭新事件提醒本地 DC 不用池被丢弃。 这些信息，并且可能会预期的那样，尤其是由于新直流 V 功能。 在事件，请参阅以下事件列表的详细信息。  
  
### <a name="BKMK_RIDBlockMaxSize"></a>删除阻止大小限制  
通常，域控制器同时请求 RID 以 500 Rid 块的分配。 你可以覆盖域控制器上使用下面的注册表 REG_DWORD 值此默认设置：  
  
```  
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\RID Values  
RID Block Size  
  
```  
  
Windows Server 2012 之前, 时实施该注册表项，除隐式 DWORD 最大 （其值 0xffffffff 或 4294967295） 中没有最大的值。 此值为远大于总全球 RID 空间。 管理员有时不恰当地或意外配置不用阻止大小耗尽巨大速率全球 RID 的值。  
  
Windows Server 2012，在你无法设置注册表该值高于 15000 小数点 (0x3A98 十六进制)。 这将阻止巨大意外的 RID 分配。  
  
如果你设置的值*更高版本*比 15000，则该值视为 15000 并域控制器将记录事件 16653 中的在每次重启，直到更正值的目录服务事件日志。  
  
### <a name="BKMK_GlobalRidSpaceUnlock"></a>全球 RID 空间大小解锁  
Windows Server 2012 之前, 全球 RID 空间是仅限于 2<sup>30</sup> （或 1073741823） 总 Rid。 达到之后，仅域迁移或森林恢复到较早的时间范围允许任何测量 Sid 创建新-灾难恢复。 在 Windows Server 2012，2 启动<sup>31</sup>位可解锁以便提高到 2147483648 Rid 全球池。  
  
广告 DS 特殊隐藏特性名为中存储此设置**SidCompatibilityVersion**上的所有域控制器 RootDSE 上下文。 此属性不较高的可读性使用 ADSIEdit、 LDP 或其他工具。 若要查看增加全球 RID 空间，检查警告事件 16655 目录-服务三千从系统事件日志，或使用以下 Dcdiag 命令：  
  
```  
Dcdiag.exe /TEST:RidManager /v | find /i "Available RID Pool for the Domain"  
  
```  
  
如果增加全球 RID 池，可用的池将更改为而不是默认 1,073,741,823 2147483647 中。 例如：  
  
![删除的颁发](media/Managing-RID-Issuance/ADDS_RID_TR_Dcdiag.png)  
  
> [!WARNING]  
> 此解锁旨在*仅*以防止退出 RID 运行，并且要使用*仅*不用上限强制结合 （请参阅下一步部分）。 不要不"预先"此设置的环境中有数百万剩余 Rid 和低增长，如与 Sid 从解锁 RID 池生成可能存在应用程序兼容性问题。  
>   
> 这解锁操作无法还原或删除，除非由完成森林恢复与之前的备份。  
  
#### <a name="important-caveats"></a>重要的几点注意事项  
Windows Server 2003 和 Windows Server 2008 域控制器在全球 RID 池 31 时，不能发出 Rid<sup>圣</sup>位处于解锁状态。 Windows Server 2008 R2 域控制器*可以*使用 31<sup>圣</sup>位 Rid *，但只有当*它们具有修补程序[KB 2642658](https://support.microsoft.com/kb/2642658)安装。 不受支持和修补域控制器视为用完时解锁全球 RID 池。  
  
此功能不会强制进行任何域的功能级别;仅在 Windows Server 2012 或已更新的 Windows Server 2008 R2 域控制器在域中存在的参加格外小心。  
  
#### <a name="implementing-unlocked-global-rid-space"></a>实现解锁全球不用空间  
解锁到 31 RID 池<sup>圣</sup>位后接收 RID 上限警报 （如下所示） 执行以下步骤：  
  
1.  请确保角色正在运行 Windows Server 2012 域控制器的不用母版。 如果没有，请将其传输到 Windows Server 2012 域控制器。  
  
2.  运行 LDP.exe  
  
3.  单击**连接**菜单，然后单击**连接**Windows Server 2012 不用主机上端口 389，，然后单击**绑定**域管理员身份。  
  
4.  单击**浏览**菜单，然后单击**修改**。  
  
5.  确保**DN**为空。  
  
6.  在**编辑条目特性**，键入：  
  
    ```  
    SidCompatibilityVersion  
    ```  
  
7.  在**值**，键入：  
  
    ```  
    1  
    ```  
  
8.  确保**添加**中选择**操作**单击**Enter**。 此更新**列表入口**。  
  
9. 选择**同步**和**扩展**选项，然后单击**运行**。  
  
    ![删除的颁发](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModify.png)  
  
10. 如果成功，LDP 输出窗口中显示：  
  
    ```  
    ***Call Modify...  
     ldap_modify_ext_s(Id, '(null)',[1] attrs, SvrCtrls, ClntCtrls);  
    modified "".  
  
    ```  
  
    ![删除的颁发](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModifySuccess.png)  
  
11. 确认增加通过检查系统事件日志中为目录-服务三千信息事件 16655 该域控制器上的全球 RID 池。  
  
### <a name="rid-ceiling-enforcement"></a>删除上限强制  
若要为提供一种保护，并提升管理感知，Windows Server 2012 引入了十 （10） %剩余 Rid 全球空间中的全局 RID 范围人工上限。 当在一 （1） %的人工上限，域控制器请求 RID 池目录-服务三千警告事件 16656 写入其系统事件日志。 当联络不用母版 FSMO 的 10%上限，它向其系统事件日志写入目录-服务三千事件 16657 和将不分配任何进一步 RID 池之前重上限。 这将强制你评估 RID 主机域中的状态并解决潜在失控而 RID 分配;这还将从耗尽整个 RID 空间保护域。  
  
此上限是硬盘编码在 %到 10%剩余的可用 RID 空间。 也就是说，上限激活时 RID 主机分配包含 RID 对应的全球 RID 空间 90 （90） %池。  
  
-   默认的域的第一个触发器点是 2<sup>30</sup>-1 * 0.90 = 966,367,640 （或 107,374,183 Rid 剩余）。  
  
-   对于域解锁 31 位 RID 空间，触发器点为 2<sup>31</sup>-1 * 0.90 = 1,932,735,282 Rid （或 214,748,365 Rid 剩余）。  
  
RID 主机触发时，设置 Active Directory 属性**msDS RIDPoolAllocationEnabled** (常见名称**ms-DS-RID-Pool-Allocation-Enabled**) 为 FALSE 对象上：  
  
CN = 美元，CN 的 RID 管理器 = 系统特区 =*<domain>*  
  
这将 16657 事件，并可进一步防止 RID 阻止发布到所有域控制器。 继续使用已颁发给他们的任何未完成 RID 池域控制器。  
  
若要删除阻止并允许 RID 池分配，以便继续，请将为 TRUE 该值。 在下一步 RID 分配由 RID 主机，特性将返回到其默认设置的值。 在此之后，有任何进一步天花板内，并且最终，全球 RID 空间耗尽，需要森林恢复或域迁移。  
  
#### <a name="removing-the-ceiling-block"></a>删除上限阻止  
若要删除后联络人工上限阻止，请执行以下步骤：  
  
1.  请确保角色正在运行 Windows Server 2012 域控制器的不用母版。 如果没有，请将其传输到 Windows Server 2012 域控制器。  
  
2.  运行 LDP.exe。  
  
3.  单击**连接**菜单，然后单击*连接*Windows Server 2012 不用主机上端口 389，，然后单击**绑定**域管理员身份。  
  
4.  单击**视图**菜单，然后单击**树**，然后为**基本 DN**选择不用母版的自己的域命名上下文。 单击**确定**。  
  
5.  在导航窗格中，向下钻取插入**CN = 系统**容器和单击**CN = RID 管理器 $**对象。 右键单击它，然后单击**修改**。  
  
6.  在编辑条目属性中，键入：  
  
    ```  
    MsDS-RidPoolAllocationEnabled  
    ```  
  
7.  在**值**，类型 （以大写形式）：  
  
    ```  
    TRUE  
    ```  
  
8.  选择**替换**中**操作**单击**Enter**。 此更新**列表入口**。  
  
9. 启用**同步**和**扩展**选项，然后单击**运行**:  
  
    ![删除的颁发](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeiling.png)  
  
10. 如果成功，LDP 输出窗口中显示：  
  
    ```  
    ***Call Modify...  
    ldap_modify_ext_s(ld, 'CN=RID Manager$,CN=System,DC=<domain>',[1] attrs, SvrCtrls, ClntCtrls);  
    Modified "CN=RID Manager$,CN=System,DC=<domain>".  
  
    ```  
  
    ![删除的颁发](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeilingSuccess.png)  
  
### <a name="other-rid-fixes"></a>其他 RID 修补程序  
以前的 Windows Server 操作系统了泄漏时 RID 池缺少 rIDSetReferences 属性。 若要解决此问题，在运行 Windows Server 2008 R2 域控制器上的，安装中的修补程序[KB 2618669](https://support.microsoft.com/kb/2618669)。  
  
### <a name="unfixed-rid-issues"></a>固定的 RID 问题  
过去已 RID 泄漏在帐户创建失败;时创建一个帐户，则失败仍会使用向上 RID。 常见示例是要创建用户使用密码不符合复杂性。  
  
### <a name="rid-fixes-for-earlier-versions-of-windows-server"></a>删除修复的较早版本的 Windows Server  
所有的修复和更改上述已发布的 Windows Server 2008 R2 修补程序。 当前没有任何 Windows Server 2008 修补程序计划或正在进行。  
  
## <a name="BKMK_Tshoot"></a>故障排除 RID 颁发  
  
### <a name="introduction-to-troubleshooting"></a>故障排除简介  
删除的颁发疑难解答需要逻辑和线性的方法。 会监视你的事件日志仔细 RID 触发警告和错误，除非你第一迹象的问题很可能会失败的帐户创建。 故障排除 RID 颁发的关键是以了解; 时应症状许多 RID 颁发问题可能会影响只有一个域控制器，并且没有任何与组件改进。 下面此简单图示可以帮助做出决定更清晰：  
  
![删除的颁发](media/Managing-RID-Issuance/adds_rid_issuance_troubleshooting.png)  
  
### <a name="troubleshooting-options"></a>故障排除的选项  
  
#### <a name="logging-options"></a>登录选项  
所有登录 RID 颁发发生系统事件日志下目录-服务三千源, 中。 启用并且最大的详细级别，默认配置日志记录。 如果没有项登录以使新的组件更改在 Windows Server 2012，将视为经典 (即传统的、 预 Windows Server 2012) 的问题 Windows 2008 R2 或较旧操作系统中看到 RID 颁发问题。  
  
#### <a name="utilities-and-commands-for-troubleshooting"></a>实用程序和疑难解答的命令  
无法通过上述日志中的所述的问题进行疑难解答尤其旧 RID 颁发问题-作为起始点使用下面列出的工具：  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   网络监视器 3.4  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>常规方法以获取疑难解答域控制器配置  
  
1.  通过一种简单而导致错误或域控制器可用性问题的权限？  
  
    1.  你尝试创建一个安全主要不必要的权限？ 检查拒绝错误访问的输出。  
  
    2.  提供了域控制器。 检查退回的错误或 LDAP 或者域控制器可用性消息。  
  
2.  具体来说返回的错误是否提及 Rid，并且特定足够要用作指南？ 如果是这样，请遵循的指南。  
  
3.  返回专门的错误是否提及 Rid，但其他方式非特定？ 例如，"Windows 无法创建对象因为目录服务无法分配相关的标识符。"  
  
    1.  检查"传统"(预 Windows Server 2012) 的域控制器上的系统事件日志中详细描述 RID 事件[不用池请求](https://technet.microsoft.com/en-us/library/ee406152(WS.10).aspx)（16642、 16643、 16644、 16645、 16656）。  
  
    2.  检查该域控制器上的系统事件和新阻止指示事件下面详细说明 （16655、 16656） 16657 本主题中的删除母版。  
  
    3.  验证与 Repadmin.exe 和不用母版可用性、 Active Directory 复制健康**Dcdiag.exe /test:ridmanager /v**。 如果这些测试不是最终，启用个双面网络域控制器和删除母版之间的捕获。  
  
### <a name="troubleshooting-specific-problems"></a>具体问题疑难解答  
在 Windows Server 2012 域控制器上的系统事件日志记录以下新消息。 这些事件，则应监视跟踪 System Center Operations Manager，如系统，自动的广告健康所有明显，以及一些域的关键问题的指示方向移动即可。  
  
|||  
|-|-|  
|事件 ID|16653|  
|源|三千-目录-服务|  
|严重性|警告|  
|消息|已由管理员身份配置池大小帐户标识符 (Rid) 大于的受支持的最大。 域控制器 RID 主机时，将使用 %1 最大值。<br /><br />有关详细信息，请参阅[不用阻止大小限制](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_RIDBlockMaxSize)。|  
|笔记和分辨率|现在，最大取消阻止大小值为 15000 小数点 (3A98 十六进制)。 域控制器无法请求多 15000 Rid。 在每次启动，直到值设置为在或下方此最多的值此事件日志。|  
  
|||  
|-|-|  
|事件 ID|16654|  
|源|三千-目录-服务|  
|严重性|信息|  
|消息|帐户标识符 (Rid) 的池已失效。 在以下预期的情况下可能会导致该问题：<br /><br />1.从备份还原域控制器。<br /><br />2.从快照还原域控制器在虚拟机上运行。<br /><br />3.管理员手动已失效池。<br /><br />请参阅 https://go.microsoft.com/fwlink/?LinkId=226247 详细信息。|  
|笔记和分辨率|是否意外此事件，请联系所有域管理员，并确定哪个它们执行操作。 目录服务事件日志还包含进一步信息上执行以下操作之一时。|  
  
|||  
|-|-|  
|事件 ID|16655|  
|源|三千-目录-服务|  
|严重性|信息|  
|消息|已增加帐户标识符 (Rid) 的全球最大到 1%。|  
|笔记和分辨率|是否意外此事件，请联系所有域管理员，并确定哪个它们执行操作。 此事件笔记整体 RID 增加池到超过 2 默认<sup>30</sup>不会自动;仅通过管理操作。|  
  
|||  
|-|-|  
|事件 ID|16656|  
|源|三千-目录-服务|  
|严重性|警告|  
|消息|已增加帐户标识符 (Rid) 的全球最大到 1%。|  
|笔记和分辨率|所需的操作 ！ 帐户标识符 (RID) 池分配给该域控制器。 池值表示此域占用了大量总可用帐户标识符的一部分。<br /><br />域达到总提供的帐户-标识符剩余以下 threshold 时，将其激活保护机制： 1%。  保护机制手动重新启用 RID 主域控制器上的帐户标识符分配之前，将阻止帐户创建。<br /><br />请参阅 https://go.microsoft.com/fwlink/?LinkId=228610 详细信息。|  
  
|||  
|-|-|  
|事件 ID|16657|  
|源|三千-目录-服务|  
|严重性|错误|  
|消息|所需的操作 ！ 此域占用了大量总提供的帐户的标识符 (Rid) 的一部分。 因为总提供的帐户-标识符剩余已激活保护机制小于： X %[人工上限参数]。<br /><br />保护机制阻止帐户创建直到手动重新启用帐户标识符 RID 主域控制器上的分配。<br /><br />它是非常重要，某些诊断执行之前重新启用帐户创建若要确保此域就不会占用异常高速度的帐户标识符。 未发现任何问题应得到解决前重新启用创建帐户。<br /><br />来诊断并修复了导致帐户标识符消耗异常高率任何根本问题可能会导致受到帐户标识符耗尽之后帐户创建将被永久禁用此域域中。<br /><br />请参阅 https://go.microsoft.com/fwlink/?LinkId=228610 详细信息。|  
|笔记和分辨率|请联系所有域管理员，并告知他们有，可以创建此域中的任何进一步安全主体直到此保护被覆盖。 有关如何覆盖保护，可能会增加整体 RID 更多信息池，请参阅[全球不用空间大小解锁](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_GlobalRidSpaceUnlock)。|  
  
|||  
|-|-|  
|事件 ID|16658|  
|源|三千-目录-服务|  
|严重性|警告|  
|消息|此事件是剩余的可用帐户标识符 (Rid) 总量定期更新。 剩余的帐户标识符数是大约： 1%。<br /><br />创建帐户时，它们可能域中创建新帐户不用完时便会使用帐户标识符。<br /><br />请参阅 https://go.microsoft.com/fwlink/?LinkId=228745 详细信息。|  
|笔记和分辨率|请联系所有域管理员，并告知他们有 RID 消耗具有交叉主要里程碑。如果这是正常的现象，或未通过查看安全信者创建模式确定。 若要在不会看到此事件会极不寻常，因为这表示该至少达 100 万 RID 已分配。|  
  
## <a name="see-also"></a>请参阅  
[管理 RID 颁发在 Windows Server 2012](http://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx)  
  



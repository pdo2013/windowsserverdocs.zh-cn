---
ms.assetid: 399a8bbe-3375-4bb0-b55b-5f46e7050028
title: "复制错误 1396 年登录失败目标的帐户名称不正确"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 84799de26e1260f914d9b959357d5eed6fef62f6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="replication-error-1396-logon-failure-the-target-account-name-is-incorrect"></a>复制错误 1396 年登录失败目标的帐户名称不正确

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
  <introduction>
    <para>本文介绍了症状，原因，以及如何解决无法 Win32 错误 1396 年 Active Directory 复制:"登录失败：目标的帐户名称是错误。"</para><list class="bullet"><listItem><para><link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Symptoms">症状</link></para></listItem><listItem><para><link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Causes">导致</link></para></listItem><listItem><para><link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Resolutions">分辨率</link></para></listItem></list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>症状</title>
    <content>
      <para />
      <list class="ordered">
<listItem><para>Active Directory 复制测试已失败，错误 1396 DCDIAG 报告：登录失败：目标的帐户名称是错误。"</para><code>Testing server: &lt;Site name&gt;&lt;DC Name&gt;
Starting test: Replications
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: CN=&lt;DN path of naming context&gt;
<codeFeaturedElement>The replication generated an error (1396):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
XX failures have occurred since the last success</code></listItem><listItem><para>REPADMIN。EXE 上次复制尝试与状态 1396 年失败的报告。</para><para>通常告知 1396 年状态的 REPADMIN 命令包括但不是限于：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN 命令</para></listItem><listItem><para>REPADMIN /REPLSUM</para></listItem><listItem><para>REPADMIN /REHOST</para></listItem><listItem><para>REPADMIN /SHOWVECTOR /LATENCY</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>示例输出从"REPADMIN /SHOWREPS"描述 CONTOSO DC2 从的入站的复制到 CONTOSO-DC1 无法使用"登录失败：不正确的目标帐户名。"错误如下所示::</para><code>Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ &lt;date&gt; &lt;time&gt; failed, <codeFeaturedElement>result 1396 (0x574):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
&lt;#&gt; consecutive failure(s).
Last success @ &lt;date&gt; &lt;time&gt;.
</code></listItem><listItem><para><ui>立即复制</ui>Active Directory 的站点和服务中的命令返回"登录失败：不正确的目标的帐户名称。"</para><para>来源 DC 连接对象右键单击并选择<ui>立即复制</ui>失败，"登录失败：目标的帐户名称是错误。"屏幕错误消息如下所示：</para><para>对话框标题文本：</para><para>立即复制</para><para>对话框消息的文本：</para><para>以下期间出错尝试同步命名上下文&lt;分区 DNS 路径&gt;从域控制器&lt;源 DC&gt;到域控制器&lt;目标 DC&gt;：登录失败：不正确的目标的帐户名称。不会继续进行此操作。</para></listItem><listItem><para>NTDS KCC、NTDS 常规或 Microsoft-Windows ActiveDirectory_DomainService 事件 1396 年状态登录事件查看器中的目录服务日志。</para><para>通常告知 1396 年状态的 Active Directory 事件包括但不是限于：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>事件 ID</para></TD><TD><para>事件源</para></TD><TD><para>事件字符串</para></TD></tr></thead><tbody><tr><TD><para>1125</para></TD><TD><para>Microsoft 的 Windows ActiveDirectory_DomainService</para></TD><TD><para>Active Directory 域服务安装向导 (Dcpromo) 无法建立连接使用下列域控制器。</para></TD></tr><tr><TD><para>1645</para><para>此事件列出的三部分 SPN。</para></TD><TD><para>NTDS 复制</para></TD><TD><para>原因目标域控制器所需的服务主要名称 (SPN) 未注册上解决 SPN 密钥 Distribution 中心 (KDC) 域控制器，Active Directory 没有经过身份验证的远程过程调用 (RPC) 执行到另一个域控制器。</para></TD></tr><tr><TD><para>1655</para></TD><TD><para>Microsoft 的 Windows ActiveDirectory_DomainService</para></TD><TD><para>尝试使用以下全球目录通信，Active Directory 域服务并尝试未成功。</para></TD></tr><tr><TD><para>2847</para></TD><TD><para>Microsoft 的 Windows ActiveDirectory_DomainService</para></TD><TD><para>知识一致性检查位于复制本地只读目录服务连接，并尝试重新远程上的以下目录服务实例对其进行更新。 操作失败。 它将重试。</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>尝试建立复制链接的以下目录可写分区失败。</para></TD></tr><tr><TD><para>1926</para></TD><TD><para>NTDS KCC</para></TD><TD><para>尝试使用以下参数失败建立只读目录分区复制链接。</para></TD></tr><tr><TD><para>5781</para></TD><TD><para>NETLOGON</para></TD><TD><para> DNS 服务器无法注册项目名称。</para></TD></tr></tbody></table></listItem><listItem><para>DCPROMO 失败并出现在屏幕上的错误</para><para>对话框标题文本：</para><para>Active Directory 安装失败</para><para>文本消息对话框：</para><para>操作失败，因为：目录服务无法为 CN 创建服务器对象 = 各自 CN = ServerBeingPromoted，CN = 服务器，CN = 站点，CN = 站点，CN = 配置，直流 = contoso，直流 = com ReplicationSourceDC.contoso.com 服务器上。</para><para>请确保网络的凭据提供有足够的访问权限，若要添加的副本。</para><para> "登录失败：目标的帐户名称是错误。"</para><para>事件 ID 1645、1168 和 1125 年这种情况下，登录所升级的服务器。</para></listItem><listItem><para>映射驱动器使用<embeddedLabel>网络使用</embeddedLabel>:</para><code>C:&gt;net use z: &lt;server_name&gt;c$
System error 1396 has occurred.
Logon Failure: The target account name is incorrect.</code><para>在此情况下，还在系统事件日志记录事件 ID 333 server 可以和使用高虚拟内存量 SQL Server 等应用程序。</para></listItem><listItem><para>DC 时间不正确。</para></listItem><listItem><para>KDC 将不上 RODC 之后开始的 krbtgt 帐户恢复的 rodc，已经删除。例如，在进行还原之后, 1396 年会出现错误。</para><para>事件 ID 1645 登录 RODC。</para><para> Dcdiag 还将它无法更新 RODC krbtgt 帐户错误报告。</para></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Causes">
    <title>导致</title>
    <content>
      <para />
      <list class="ordered">
        <listItem>
          <para>SPN 全球目录搜索 kdc 代表尝试进行身份验证使用 Kerberos 客户端上不存在。</para>
          <para>Active Directory 复制的上下文，Kerberos 客户端是目标直流，KDC 执行 SPN 查找可能是目标 DC 本身但也可能是远程直流。</para>
        </listItem>
        <listItem>
          <para>应该包含该服务所主要名称查阅用户或服务帐户不存在上搜索 kdc 代表目标 DC 尝试复制全球目录。</para>
          <para>Active Directory 复制的上下文中, 源直流计算机帐户上不存在由 DC 搜索代表目标执行站 DC 全球目录复制。</para>
        </listItem>
        <listItem>
          <para>目标 DC 缺少 LSA 幽静源 Dc 域。</para>
        </listItem>
        <listItem>
          <para>SPN 所查阅位于其他计算机比源 DC 帐户。</para>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>分辨率</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>检查 NTDS 复制事件 1645 年目标 DC 目录服务事件登录并注意以下：</para>
          <para>目标 DC 名称</para>
          <para>SPN 所查阅 (E3514235-4B06-11D1-AB04-00C04FC2DCD2 /&lt;对象 guid 源 Dc 各自的对象&gt;/&lt;目标域&gt;。&lt;tld&gt;@&lt;目标域&gt;。&lt;tld&gt;</para>
          <para>目标 DC 正在使用的 KDC</para>
        </listItem>
        <listItem>
          <para>从控制台 KDC 在第 1 步中，键入：</para>
          <code>nltest /dsgetdc &lt;forest root DNS domain name &gt; /gc</code>
          <para>运行紧跟无法 1396 年错误代码目标直流上的复制尝试 NLTEST 定位器测试。</para>
          <para>这应该识别 KDC 执行 SPN 查询针对该 GC。</para>
          <para>GC 被搜索 kdc 也可能捕捉 Microsoft-Windows ActiveDirectory_DomainService 事件 1655 年中。</para>
        </listItem>
        <listItem>
          <para>SPN 发现上发现第 2 步中的全局目录第 1 步中的搜索。</para>
          <code>C:&gt;repadmin /showattr Server_Name DC=corp,DC=contoso,dc=com &lt;GC used by KDC&gt; &lt;DN path of forest root domain&gt; /filter:"(serviceprincipalname=&lt;SPN cited in the NTDS Replication event 1645&gt;)" /gc /subtree /atts:cn,serviceprincipalname</code>
          <para>或者</para>
          <code>C:&gt;dsquery * forestroot -scope subtree -filter "(serviceprincipalname=E3514235-4B06-11D1-AB04-00C04FC2DCD2/65cead9f-4949-46a3-a49a-f1fbfe13d2b3*)" -attr * -s Server_Name.europe.corp.contoso.com</code>
          <para>验证是否存在 SPN 的主机对象。</para>
          <para>验证主机对象包括，无论是 CNF / 冲突出错或位于丢失和找到容器 DN 路径。</para>
          <para>验证，仅源 Dc 计算机帐户上注册了 Dc Active Directory 复制 SPN 的来源。</para>
          <para>缺少复制 SPN 时，确定源直流注册其 SPN 与本身，并使用简单复制延迟或复制故障由于 KDC GC 中缺少 SPN 是否。</para>
        </listItem>
        <listItem>
          <para>检查安全通道健康并信任运行状况。</para>
        </listItem>
      </list>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>失败，错误 1396 年的疑难解答 Active Directory 操作：登录失败：不正确的目标的帐户名称。</linkText>
      <linkUri>https://support.microsoft.com/kb/2183411/en-gb</linkUri>
    </externalLink>
  </relatedTopics>
</developerConceptualDocument>



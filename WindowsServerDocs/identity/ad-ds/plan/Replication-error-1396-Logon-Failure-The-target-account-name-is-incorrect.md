---
ms.assetid: 399a8bbe-3375-4bb0-b55b-5f46e7050028
title: 复制错误 1396 - 登录失败：目标帐户名称不正确
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 55e93eb24707fb1a583d060f44131ac794b00d22
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442552"
---
# <a name="replication-error-1396-logon-failure-the-target-account-name-is-incorrect"></a>复制错误 1396 - 登录失败：目标帐户名称不正确

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd"> <introduction>
    <para>本指南介绍了症状、 原因和解决 Active Directory 复制失败，Win32 错误 1396年:&quot;登录失败：目标帐户名称不正确。&quot; </para>
    <list class="bullet">
      <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Symptoms">症状</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Causes">原因</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Resolutions">解决方法</link>
        </para>
      </listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>症状</title>
    <content>
      <para />
      <list class="ordered">
<listItem><para>Active Directory 复制测试失败，出现错误 1396 DCDIAG 报表：登录失败：目标帐户名称不正确。&quot;</para><code>Testing server: &lt;Site name&gt;&lt;DC Name&gt;
Starting test: Replications
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: CN=&lt;DN path of naming context&gt;
<codeFeaturedElement>The replication generated an error (1396):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
XX failures have occurred since the last success</code></listItem><listItem><para>REPADMIN。上次复制尝试失败，状态为 1396年的 EXE 报表。</para><para>REPADMIN 命令，通常涉及 1396年状态包括但不限于：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /ADD</para></listItem><listItem><para>REPADMIN /REPLSUM</para></listItem><listItem><para>REPADMIN /REHOST</para></listItem><listItem><para>REPADMIN /SHOWVECTOR /LATENCY</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>示例的输出&quot;REPADMIN /SHOWREPS&quot;描述了从 CONTOSO DC2 的入站的复制到 CONTOSO-DC1 而失败的&quot;登录失败：目标帐户名称不正确。&quot;错误如下所示::</para><code>Default-First-Site-NameCONTOSO-DC1
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
</code></listItem><listItem><para><ui>立即复制副本</ui>Active Directory 站点和服务中的命令将返回&quot;登录失败：目标帐户名称不正确。&quot;</para><para>中的源 DC 的连接对象上右键单击并选择<ui>立即复制副本</ui>失败，出现&quot;登录失败：目标帐户名称不正确。&quot;错误消息如下所示的屏幕：</para><para>对话框标题文本：</para><para>立即复制副本</para><para>对话框消息文本： </para><para>尝试命名上下文进行同步期间出现以下错误&lt;分区 DNS 路径&gt;从域控制器&lt;源 DC&gt;到域控制器&lt;目标 DC&gt;:登录失败：目标帐户名称不正确。 此操作将继续。 </para></listItem><listItem><para>在事件查看器中的目录服务日志中记录 NTDS KCC、 NTDS 常规或 Microsoft Windows ActiveDirectory_DomainService 1396 状态的事件。</para><para>通常涉及 1396年状态的 active Directory 事件包括但不限于：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>事件 ID</para></TD><TD><para>事件来源</para></TD><TD><para>事件字符串</para></TD></tr></thead><tbody><tr><TD><para>1125</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Active Directory 域服务安装向导 (Dcpromo) 无法与下列域控制器建立连接。</para></TD></tr><tr><TD><para>1645</para><para>此事件列出了三部分组成的 SPN。</para></TD><TD><para>NTDS 复制</para></TD><TD><para>Active Directory 没有执行到另一域控制器的身份验证远程过程调用 (RPC)，因为目标域控制器要求的服务主体名称 (SPN) 没有在主持解析 SPN 的密钥分发中心 (KDC) 域控制器上注册。</para></TD></tr><tr><TD><para>1655</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Active Directory 域服务尝试与以下全局编录进行通信，但尝试成功。</para></TD></tr><tr><TD><para>2847</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>知识一致性检查程序位于本地的只读的目录服务的复制连接，而尝试远程对以下目录服务实例对其进行更新。 操作失败。 它将重试。</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>尝试建立以下的可写目录分区的复制链接失败。</para></TD></tr><tr><TD><para>1926</para></TD><TD><para>NTDS KCC</para></TD><TD><para>尝试建立到只读目录分区的复制链接具有以下参数失败。</para></TD></tr><tr><TD><para>5781</para></TD><TD><para>NETLOGON</para></TD><TD><para> 服务器无法在 DNS 中注册其名称。</para></TD></tr></tbody></table></listItem><listItem><para>DCPROMO 失败并出现在屏幕上的错误</para><para>对话框标题文本：</para><para>Active Directory 安装失败</para><para>对话框消息文本：</para><para>操作失败，因为：目录服务无法创建服务器对象的 CN = NTDS 设置，CN = ServerBeingPromoted，CN = Servers，CN = Site、 CN = Sites，CN = Configuration，DC = contoso，DC = com 服务器 ReplicationSourceDC.contoso.com 上。 </para><para>请确保提供的网络凭据有足够的权限访问要添加副本。 </para><para>
&quot;登录失败：目标帐户名称不正确。 &quot;</para><para>在这种情况下，要升级的服务器上记录事件 ID 1645、 1168 和 1125年。</para></listItem><listItem><para>驱动器使用映射<embeddedLabel>net 使用</embeddedLabel>:</para><code>C:&gt;net use z: &lt;server_name&gt;c$
System error 1396 has occurred.
Logon Failure: The target account name is incorrect.</code><para>在这种情况下，还在系统事件日志中日志记录事件 ID 333 服务器可以和用于 SQL Server 之类的应用程序的虚拟内存量较高。</para></listItem><listItem><para>DC 时间不正确。</para></listItem><listItem><para>KDC 将无法启动在 RODC 上的 krbtgt 帐户在还原后的 rodc，已删除。 例如，在还原后 1396年会出现错误。 </para><para>
在 RODC 上记录事件 ID 1645。 </para><para>
Dcdiag 报告错误，它不能更新 RODC 的 krbtgt 帐户。 </para></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Causes">
    <title>原因</title>
    <content>
      <para />
      <list class="ordered">
        <listItem>
          <para>SPN 不存在全局编录搜索的 KDC 代表尝试使用 Kerberos 进行身份验证的客户端上。</para>
          <para>在 Active Directory 复制的上下文中，Kerberos 客户端是目标 DC，KDC 执行 SPN 查找可能是目标 DC 本身，但可以是远程的 DC。</para>
        </listItem>
        <listItem>
          <para>应包含要查找的服务主体名称的服务帐户的用户不存在全局编录搜索代表目标 DC 尝试复制的 KDC 的。</para>
          <para>在 Active Directory 复制的上下文中，源 DC 的计算机帐户不存在全局编录搜索代表目标 DC 执行入站复制 DC 上。</para>
        </listItem>
        <listItem>
          <para>目标 DC 缺少源 Dc 域到 LSA 机密。</para>
        </listItem>
        <listItem>
          <para>正在查找的 SPN 存在比源 DC 在不同的计算机帐户上。</para>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>解决方法</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>检查 NTDS 复制事件 1645年的目标 DC 上的目录服务事件日志，并请注意以下问题：</para>
          <para>目标 DC 的名称</para>
          <para>SPN 正在查找 (e3514235-4b06-11d1-ab04-00c04fc2dcd2 /&lt;对象的源域控制器的 NTDS 设置对象的 guid&gt;/&lt;目标域&amp;a m p; g t;。&amp;a m p; l t; tld&amp;a m p; g t; @&lt;目标域&gt;。&lt;tld&gt;</para>
          <para>KDC 正由目标 DC</para>
        </listItem>
        <listItem>
          <para>在步骤 1 中确定对 KDC 的控制台中，键入： </para>
          <code>nltest /dsgetdc &lt;forest root DNS domain name &gt; /gc</code>
          <para>运行 NLTEST 定位器测试紧跟目标 DC 上 1396年错误而失败的复制尝试。 </para>
          <para>这应确定 KDC 执行针对 SPN 查找该 GC。 </para>
          <para>要搜索的 KDC GC 还可能在 Microsoft Windows ActiveDirectory_DomainService 事件 1655年中捕获。</para>
        </listItem>
        <listItem>
          <para>搜索在发现在步骤 2 中对全局目录的步骤 1 中发现的 SPN。</para>
          <code>C:&gt;repadmin /showattr Server_Name DC=corp,DC=contoso,dc=com &lt;GC used by KDC&gt; &lt;DN path of forest root domain&gt; /filter:&quot;(serviceprincipalname=&lt;SPN cited in the NTDS Replication event 1645&gt;)&quot; /gc /subtree /atts:cn,serviceprincipalname</code>
          <para>或者</para>
          <code>C:&gt;dsquery * forestroot -scope subtree -filter &quot;(serviceprincipalname=E3514235-4B06-11D1-AB04-00C04FC2DCD2/65cead9f-4949-46a3-a49a-f1fbfe13d2b3*)&quot; -attr * -s Server_Name.europe.corp.contoso.com</code>
          <para>验证 SPN 的主机对象存在。</para>
          <para>验证主机的对象包括，无论对象是 CNF / 冲突出错或驻留在丢失和找到容器的 DN 路径。</para>
          <para>验证源 Dc Active Directory 复制 SPN 仅在源域控制器计算机帐户上注册。</para>
          <para>如果缺少的 SPN 的复制，如果源 DC 已注册的 SPN 与其自身，确定并确定是否 SPN 缺少由简单的复制延迟或复制失败由于 KDC 到 GC 上。</para>
        </listItem>
        <listItem>
          <para>检查安全通道运行状况和信任运行状况。</para>
        </listItem>
      </list>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>故障排除 Active Directory 操作，因错误 1396年:登录失败：目标帐户名称不正确。</linkText>
      <linkUri><a href="https://support.microsoft.com/kb/2183411/en-gb" data-raw-source="https://support.microsoft.com/kb/2183411/en-gb">https://support.microsoft.com/kb/2183411/en-gb</a></linkUri>
    </externalLink>
  </relatedTopics>
</developerConceptualDocument>



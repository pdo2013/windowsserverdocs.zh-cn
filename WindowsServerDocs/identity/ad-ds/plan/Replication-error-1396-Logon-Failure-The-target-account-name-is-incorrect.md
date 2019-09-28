---
ms.assetid: 399a8bbe-3375-4bb0-b55b-5f46e7050028
title: 复制错误 1396 - 登录失败：目标帐户名称不正确
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a8af10fd54f557e4f4a2127dbd1cc178d53d93a4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402489"
---
# <a name="replication-error-1396-logon-failure-the-target-account-name-is-incorrect"></a>复制错误 1396 - 登录失败：目标帐户名称不正确

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd"> <introduction> @ no__t-2 @ no__t<para>本文介绍了症状、原因以及如何解决 Active Directory 复制失败，并出现 Win32 错误1396：&quot;Logon 失败：目标帐户名不正确。 &quot; </para>
    <list class="bullet"> <listItem> @ no__t-2 @ no__t<para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Symptoms">症状</link>
 @ no__t-2</para>
      </listItem> <listItem> @ no__t-2 @ no__t<para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Causes">导致</link>
 @ no__t-2</para>
      </listItem> <listItem> @ no__t-2 @ no__t<para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Resolutions">分辨率</link>
 @ no__t-2</para>
      </listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>Symptoms @ no__t-1 @ no__t-2 @ no__t-3<para />
      <list class="ordered">
<listItem><para>DCDIAG 报告 Active Directory 复制测试失败，出现错误1396：登录失败：目标帐户名不正确。 &quot;</para><code>Testing server: &lt;Site name&gt;&lt;DC Name&gt;
Starting test: Replications
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: CN=&lt;DN path of naming context&gt;
<codeFeaturedElement>The replication generated an error (1396):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
XX failures have occurred since the last success</code></listItem><listItem><para>REPADMIN.EXE 报告上次复制尝试失败，状态为1396。</para><para>通常引用1396状态的 REPADMIN 命令包括但不限于：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN/ADD</para></listItem><listItem><para>REPADMIN/REPLSUM</para></listItem><listItem><para>REPADMIN/REHOST</para></listItem><listItem><para>REPADMIN/SHOWVECTOR/LATENCY</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN/SHOWREPS</para></listItem><listItem><para>REPADMIN/SHOWREPL</para></listItem><listItem><para>REPADMIN/SYNCALL</para></listItem></list></TD></tr></tbody></table><para>示例输出 &quot;REPADMIN/SHOWREPS @ no__t 描述从 CONTOSO 到 CONTOSO-DC1 的入站复制失败，并出现 &quot;Logon 失败：目标帐户名不正确。 @no__t 错误如下所示：</para><code>Default-First-Site-NameCONTOSO-DC1
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
</code></listItem><listItem><para>Active Directory 站点和服务中的 "<ui>立即复制</ui>" 命令将返回 &quot;Logon 故障：目标帐户名不正确。 &quot;</para><para>右键单击源 DC 中的连接对象，然后选择 "<ui>复制</ui>" 将失败，并出现 @no__t 1Logon 故障：目标帐户名不正确。 &quot;屏幕错误消息如下所示：</para><para>对话框标题文本：</para><para>立即复制</para><para>对话框消息文本： </para><para>尝试将命名上下文 @no__t 从域控制器 &lt;source DC @ no__t 到域控制器 &lt;destination DC @ no__t-5 同步命名上下文时出现以下错误：登录失败：目标帐户名不正确。 此操作不会继续。 </para></listItem><listItem><para>ActiveDirectory_DomainService 状态的 NTDS KCC、NTDS General 或 Microsoft-Windows-1396 事件记录在事件查看器的目录服务日志中。</para><para>Active Directory 通常引用1396状态的事件包括但不限于：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>事件 ID</para></TD><TD><para>事件来源</para></TD><TD><para>事件字符串</para></TD></tr></thead><tbody><tr><TD><para>1125</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Active Directory 域服务安装向导（Dcpromo）无法与下列域控制器建立连接。</para></TD></tr><tr><TD><para>1645</para><para>此事件列出由三个部分构成的 SPN。</para></TD><TD><para>NTDS 复制</para></TD><TD><para>Active Directory 没有执行到另一域控制器的身份验证远程过程调用 (RPC)，因为目标域控制器要求的服务主体名称 (SPN) 没有在主持解析 SPN 的密钥分发中心 (KDC) 域控制器上注册。</para></TD></tr><tr><TD><para>1655</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Active Directory 域服务尝试与以下全局编录通信，尝试未成功。</para></TD></tr><tr><TD><para>2847</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>知识一致性检查器找到本地只读目录服务的复制连接，并尝试在以下目录服务实例上远程更新该连接。 操作失败。 它将重试。</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>尝试为以下可写目录分区建立复制链接失败。</para></TD></tr><tr><TD><para>1926</para></TD><TD><para>NTDS KCC</para></TD><TD><para>尝试使用以下参数与只读目录分区建立复制链接失败。</para></TD></tr><tr><TD><para>5781</para></TD><TD><para>NETLOGON</para></TD><TD><para> 服务器无法在 DNS 中注册其名称。</para></TD></tr></tbody></table></listItem><listItem><para>DCPROMO 出现错误，出现屏幕错误</para><para>对话框标题文本：</para><para>Active Directory 安装失败</para><para>对话框消息文本：</para><para>操作失败，原因是：在服务器 ReplicationSourceDC.contoso.com 上，目录服务无法为 CN = NTDS Settings，CN = ServerBeingPromoted，CN = Servers，CN = Site，CN = Sites，CN = Configuration，DC = contoso，DC = com 创建服务器对象。 </para><para>请确保提供的网络凭据具有足够的权限来添加副本。 </para><para>
&quot;Logon 失败：目标帐户名不正确。 &quot;</para><para>在这种情况下，将在要升级的服务器上记录事件 ID 1645、1168和1125。</para></listItem><listItem><para>使用<embeddedLabel>net use</embeddedLabel>映射驱动器：</para><code>C:&gt;net use z: &lt;server_name&gt;c$
System error 1396 has occurred.
Logon Failure: The target account name is incorrect.</code><para>在这种情况下，服务器还可以将事件 ID 333 记录在系统事件日志中，并为应用程序（如 SQL Server）使用大量虚拟内存。</para></listItem><listItem><para>DC 时间不正确。</para></listItem><listItem><para>还原 RODC 的 krbtgt 帐户之后，KDC 将不会在 RODC 上启动，该帐户已被删除。 例如，还原后，出现错误1396。 </para><para>
事件 ID 1645 记录在 RODC 上。 </para><para>
Dcdiag 还报告错误，指出它无法更新 RODC krbtgt 帐户。 </para></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Causes">
    <title>Causes @ no__t-1 @ no__t-2 @ no__t-3<para />
      <list class="ordered">
        <listItem>
          <para>由 KDC 代表客户端尝试使用 Kerberos 进行身份验证的全局编录上不存在 SPN。</para>
          <para>在 Active Directory 复制的上下文中，Kerberos 客户端是目标 DC，执行 SPN 查找的 KDC 可能是目标 DC 本身，但也可能是远程 DC。</para>
        </listItem>
        <listItem>
          <para>应包含正在查找的服务主体名称的用户或服务帐户不存在于由 KDC 代表尝试复制的目标 DC 搜索的全局目录中。</para>
          <para>在 Active Directory 复制的上下文中，DC 代表执行入站复制的目标 DC 在 DC 上搜索的全局目录中不存在源 DC 计算机帐户。</para>
        </listItem>
        <listItem>
          <para>目标 DC 对于源 Dc 域缺少 LSA 机密。</para>
        </listItem>
        <listItem>
          <para>正在查找的 SPN 与源 DC 的计算机帐户不同。</para>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Resolutions @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5<para>在目标 DC 上检查 NTDS 复制事件1645的目录服务事件日志，并注意以下事项：</para>
          <para>目标 DC 的名称</para>
          <para>正在查找的 SPN （E3514235-4B06-11D1-AB04-00C04FC2DCD2/&lt;object guid 用于源 Dc NTDS 设置对象 @ no__t-1 @ no__t-2 @ no__t-3target 域 @ no__t-4amp; g t;。&amp;amp; lt; tld @ no__t-6amp; gt; @ &lt;target domain @ no__t-8. &lt;tld @ no__t-10</para>
          <para>目标 DC 使用的 KDC</para>
        </listItem>
        <listItem>
          <para>在步骤1中确定的 KDC 控制台中，键入： </para>
          <code>nltest /dsgetdc &lt;forest root DNS domain name &gt; /gc</code>
          <para>紧跟在目标 DC 上出现1396错误的复制尝试之后立即运行 NLTEST 定位符测试。 </para>
          <para>这应该确定 KDC 正在对其执行 SPN 查找的 GC。 </para>
          <para>还可以在 ActiveDirectory_DomainService 事件1655中捕获 KDC 正在搜索的 GC。</para>
        </listItem>
        <listItem>
          <para>在步骤2中发现的全局编录上搜索在步骤1中发现的 SPN。</para>
          <code>C:&gt;repadmin /showattr Server_Name DC=corp,DC=contoso,dc=com &lt;GC used by KDC&gt; &lt;DN path of forest root domain&gt; /filter:&quot;(serviceprincipalname=&lt;SPN cited in the NTDS Replication event 1645&gt;)&quot; /gc /subtree /atts:cn,serviceprincipalname</code>
          <para>或者</para>
          <code>C:&gt;dsquery * forestroot -scope subtree -filter &quot;(serviceprincipalname=E3514235-4B06-11D1-AB04-00C04FC2DCD2/65cead9f-4949-46a3-a49a-f1fbfe13d2b3*)&quot; -attr * -s Server_Name.europe.corp.contoso.com</code>
          <para>验证 SPN 的宿主对象是否存在。</para>
          <para>验证主机对象的 DN 路径，其中包括对象是否已损坏或驻留在丢失和找到的容器中。</para>
          <para>验证是否仅在源 Dc 计算机帐户上注册了源 Dc Active Directory 复制 SPN。</para>
          <para>如果缺少复制 SPN，则确定源 DC 是否已向其自身注册了 SPN，以及由于简单复制延迟或复制失败导致 KDC 所使用的 GC 上缺少 SPN。</para>
        </listItem>
        <listItem>
          <para>检查安全通道运行状况和信任运行状况。</para>
        </listItem>
      </list>
    </content>
  </section>
  <relatedTopics> @ no__t-1 @ no__t-2Troubleshooting Active Directory 操作失败并出现错误1396：登录失败：目标帐户名不正确。 @no__t 0
      <linkUri><a href="https://support.microsoft.com/kb/2183411/en-gb" data-raw-source="https://support.microsoft.com/kb/2183411/en-gb">https://support.microsoft.com/kb/2183411/en-gb</a></linkUri>
    </externalLink>
  </relatedTopics> @ no__t-6



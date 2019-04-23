---
ms.assetid: 0f21951c-b1bf-43bb-a329-bbb40c58c876
title: 复制错误 1753：端点映射程序中没有更多可用的端点
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e429c87a2194ecfaf02c3d6c579eda75293250d4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827508"
---
# <a name="replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper"></a>复制错误 1753：端点映射程序中没有更多可用的端点

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd"> <introduction>
    <para>本主题介绍症状、 原因以及如何解析 Active Directory 复制错误 8524 DSA 操作无法继续由于 DNS 查找失败。</para>
    <list class="bullet">
      <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Symptoms">症状</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Cause">原因</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Resolutions">解决方法</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_MoreInfo">详细信息</link>
        </para>
      </listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>症状</title>
    <content>
      <para>本指南介绍了症状、 原因以及解决方法步骤失败，出现 Win32 错误 1753年的 Active Directory 操作："没有更多的终结点可从终结点映射程序。"</para>
      <list class="ordered">
        <listItem>
          <para>连接测试、 Active Directory 复制测试或 KnowsOfRoleHolders 测试失败，出现错误 1753 DCDIAG 报表："没有更多的终结点可从终结点映射程序。"</para>
          <code>Testing server: &lt;site&gt;&lt;DC Name&gt;
Starting test: Connectivity
* Active Directory LDAP Services Check
* Active Directory RPC Services Check
[&lt;DC Name&gt;] <codeFeaturedElement>DsBindWithSpnEx() failed with error 1753,
There are no more endpoints available from the endpoint mapper..</codeFeaturedElement>
Printing RPC Extended Error Info:
Error Record 1, ProcessID is &lt;process ID&gt; (DcDiag) 
System Time is: &lt;date&gt; &lt;time&gt;
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper. Detection location is 500
NumberOfParameters is 4
Unicode string: ncacn_ip_tcp
Unicode string: &lt;source DC object GUID&gt;._msdcs.contoso.com
Long val: -481213899
Long val: 65537
Error Record 2, ProcessID is 700 (DcDiag) 
System Time is: &lt;date&gt; &lt;time&gt;
Generating component is 2 (RPC runtime)
<codeFeaturedElement>Status is 1753: There are no more endpoints available from the endpoint mapper.</codeFeaturedElement>
NumberOfParameters is 1
Unicode string: 1025
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: &lt;DN path of directory partition&gt;
The replication generated an error <codeFeaturedElement>(1753):
There are no more endpoints available from the endpoint mapper.</codeFeaturedElement> 
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
3 failures have occurred since the last success.
The directory on &lt;DC name&gt; is in the process.
of starting up or shutting down, and is not available.
Verify machine is not hung during boot.
</code>
        </listItem>
<listItem><para>REPADMIN。EXE 报告该复制尝试失败，状态为 1753年。</para><para>REPADMIN 命令，通常涉及 1753年状态包括但不限于：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /REPLSUM</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>从"REPADMIN /SHOWREPS"进行描述从 CONTOSO DC2 到 CONTOSO-DC1 失败，"复制访问被拒绝"错误的入站的复制的示例输出如下所示：</para><code>Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ &lt;date&gt; &lt;time&gt; failed, <codeFeaturedElement>result 1753 (0x6d9):
There are no more endpoints available from the endpoint mapper.</codeFeaturedElement>
&lt;#&gt; consecutive failure(s).
Last success @ &lt;date&gt; &lt;time&gt;.

</code></listItem><listItem><para><ui>检查复制拓扑</ui>Active Directory 站点和服务中的命令将返回"有没有更多的终结点可从终结点映射程序。"</para><para>中的源 DC 的连接对象上右键单击并选择<ui>检查复制拓扑</ui>失败，出现"没有更多终结点可从终结点映射程序。" 错误消息如下所示的屏幕：</para><para>对话框标题文本：检查复制拓扑</para><para>对话框消息文本： </para><para>在尝试联系域控制器期间出现以下错误：端点映射程序中未提供更多端点。</para></listItem><listItem><para><ui>立即复制副本</ui>Active Directory 站点和服务中的命令将返回"有没有更多的终结点可从终结点映射程序。"</para><para>中的源 DC 的连接对象上右键单击并选择<ui>立即复制副本</ui>失败，出现"没有更多终结点可从终结点映射程序。" 错误消息如下所示的屏幕：</para><para>对话框标题文本：立即复制副本</para><para>对话框消息文本：尝试命名上下文进行同步期间出现以下错误&lt;%目录分区名称 %&gt;从域控制器&lt;源 DC&gt;到域控制器&lt;目标 DC&gt;:</para><para>

端点映射程序中未提供更多端点。</para><para>将继续操作</para></listItem><listItem><para>在事件查看器中的目录服务日志记录-2146893022： 状态 NTDS KCC、 NTDS 常规或 Microsoft Windows ActiveDirectory_DomainService 事件。</para><para>Active Directory 事件通常涉及-2146893022： 状态，包括但不限于：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>事件 ID</para></TD><TD><para>事件来源</para></TD><TD><para>事件字符串</para></TD></tr></thead><tbody><tr><TD><para>1655</para></TD><TD><para>NTDS 常规</para></TD><TD><para>Active Directory 尝试与以下全局编录进行通信，但尝试成功。</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>尝试建立以下的可写目录分区的复制链接失败。</para></TD></tr><tr><TD><para>1265</para></TD><TD><para>NTDS KCC</para></TD><TD><para>尝试通过知识一致性检查器 (KCC) 将添加下列目录分区和源域控制器的复制协议失败。</para></TD></tr></tbody></table></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Cause">
    <title>原因</title>
    <content>
      <para>下图显示 RPC 工作流从 RPC 客户端到步骤 7 中的客户端应用程序的数据传递到开始与服务器应用程序在步骤 1 中使用 RPC 终结点映射器 (EPM) 的注册。 </para>
      <para>&lt;ADDS_RPCWorkflow&gt;</para>
      <para>步骤 1 到步骤 7 映射到以下操作：</para>
      <list class="ordered">
        <listItem>
          <para>服务器应用程序注册其终结点与 RPC 终结点映射器 (EPM) </para>
        </listItem>
        <listItem>
          <para>客户端发出的 RPC 调用 （代表用户，OS 或应用程序启动的操作） </para>
        </listItem>
        <listItem>
          <para>客户端 RPC 联系目标计算机 EPM，并请求以完成客户端调用的终结点 </para>
        </listItem>
        <listItem>
          <para>与终结点的服务器计算机的企业项目管理响应 </para>
        </listItem>
        <listItem>
          <para>客户端 RPC 联系服务器应用 </para>
        </listItem>
        <listItem>
          <para>服务器应用程序执行调用，将结果返回到客户端 RPC </para>
        </listItem>
        <listItem>
          <para>客户端 RPC 将结果传递回客户端应用程序</para>
        </listItem>
      </list>
      <para>失败而导致 1753年生成通过失败步骤 #3 和 4 之间。 具体而言，错误 1753： 意味着 RPC 客户端 （目标 DC） 是能够通过端口 135 联系 RPC 服务器 （源 DC），但是 EPM RPC 服务器 （源 DC） 上的找不到感兴趣的 RPC 应用程序，并且返回了服务器端错误 1753年。 1753 错误存在指示，RPC 客户端 （目标 DC） 通过网络从 RPC 服务器 （AD 复制源 DC） 接收服务器端错误响应。 </para>
      <para>1753 错误的特定根本原因包括： </para>
      <list class="ordered">
        <listItem>
          <para>永远不会启动服务器应用 （即如果从未尝试在上方的"详细信息"关系图中的步骤 #1）。</para>
        </listItem>
        <listItem>
          <para>服务器应用程序启动，但阻止其注册到 RPC 终点映射程序 （即在上面的"详细信息"关系图中的步骤 #1 已尝试但失败） 的初始化期间出现一些错误。</para>
        </listItem>
        <listItem>
          <para>服务器应用程序启动，但随后死了。 （即在上面的"详细信息"关系图中的步骤 #1 已成功完成，但因为撤消更高版本服务器已停机）。</para>
        </listItem>
        <listItem>
          <para>服务器应用手动注销其终结点 （类似于但有意 3。 不太可能，但包含出于完整性的考虑。）</para>
        </listItem>
        <listItem>
          <para>RPC 客户端 （目标 DC） 联系其他 RPC 服务器与预期由于名称到 IP 在 DNS、 WINS 或主机/Lmhosts 文件中的映射错误。</para>
        </listItem>
      </list>
      <para>不被因错误 1753年: </para>
      <list class="bullet">
        <listItem>
          <para>缺少的 RPC 客户端 （目标 DC） 和 RPC 服务器 （源 DC） 之间通过端口 135 的网络连接</para>
        </listItem>
        <listItem>
          <para>缺少的 RPC 服务器 （源 DC） 之间的网络连接通过临时端口使用端口 135 和 RPC 客户端 （目标 DC）。 </para>
        </listItem>
        <listItem>
          <para>密码不匹配或源 DC 无法解密 Kerberos 加密的数据包 </para>
        </listItem>
      </list>
      <para> </para>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>解决方法</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>
            <embeddedLabel>验证注册其服务终结点映射程序服务已启动</embeddedLabel>
          </para>
          <para>对于 Windows 2000 和 Windows Server 2003 Dc： 请确保源 DC 将启动进入正常模式。 </para>
          <para>
对于 Windows Server 2008 或 Windows Server 2008 R2： 在源 DC 的控制台中，启动服务管理器 (services.msc) 并确认<embeddedLabel>Active Directory 域服务</embeddedLabel>服务是否正在运行。 </para>
        </listItem>
        <listItem>
          <para>
            <embeddedLabel>验证连接到预期的 RPC 服务器 （源 DC） 该 RPC 客户端 （目标 DC）</embeddedLabel>
          </para>
          <para>常见的 Active Directory 林中的所有域控制器在 _msdcs 中注册的域控制器的 CNAME 记录。&lt;林根级域&gt;而不考虑驻留在林中哪些域的 DNS 区域。 DC CNAME 记录派生自<embeddedLabel>objectGUID</embeddedLabel>的每个域控制器的 NTDS 设置对象的属性。 </para>
          <para>执行基于复制的操作时，目标 DC 将 DNS 查询的源 Dc CNAME 记录。 该 CNAME 记录包含用于派生的源 Dc 的 IP 地址的源 DC 完全限定的计算机名称通过 DNS 客户端的缓存查找，托管 / LMHost 文件查找，主机 A / AAAA 记录在 DNS 或 WINS 中。 </para>
          <para>过时的 NTDS 设置对象和 DNS、 WINS、 主机和 LMHOST 文件中的错误名称到 IP 映射可能会导致 RPC 客户端 （目标 DC） 连接到错误的 RPC 服务器 (源 DC)。 此外，不正确的名称到 IP 映射可能会导致 RPC 客户端 （目标 DC） 连接到的计算机，甚至没有 RPC 服务器应用程序感兴趣 （在本例中为 Active Directory 角色） 安装。 (示例： DC2 的过时主机记录包含 DC3 或成员计算机的 IP 地址)。 </para>
          <para>验证存在于目标域控制器复制 Active Directory 的源 DC objectGUID 匹配源 DC objectGUID 存储在源域控制器复制 Active directory 中。 如果存在不一致，使用 repadmin /showobjmeta ntds 设置对象上以查看哪一个对应于源 DC 的最后一个促销 (提示： 比较日期戳的 NTDS 设置对象从 /showobjmeta 针对中的最后升级日期创建日期源 Dc dcpromo.log 文件。 您可能需要使用最后一个修改 / DCPROMO 的创建日期。日志文件本身）。 如果对象 Guid 不同，目标 DC 可能有其 CNAME 记录是指主机记录具有错误名称到 IP 映射的过时源 DC 的 NTDS 设置对象。 </para>
          <para>目标 DC 上运行 IPCONFIG /ALL 来确定哪些 DNS 服务器进行名称解析使用 DC 的目标：</para>
          <code>c:&gt;ipconfig /all</code>
          <para>目标 DC 上对源 Dc 完全限定的 DC CNAME 记录运行 NSLOOKUP:</para>
          <code>c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs primary DNS Server IP &gt;
c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs secondary DNS Server IP&gt;</code>
          <para>验证是否返回 NSLOOKUP 的 IP 地址"拥有"的主机名 / 源 DC 的安全标识：</para>

          <code>C:&gt;NBTSTAT -A &lt;IP address returned by NSLOOKUP in the step above&gt;</code>
          <para>或</para>
          <para>登录到源 DC 的控制台，请在命令提示符处运行"IPCONFIG"并验证源 DC 拥有上述 NSLOOKUP 命令返回的 IP 地址</para>
          <para>检查过时 / 重复主机 IP 在 DNS 中映射到</para>
          <code>NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;secondary DNS Server IP on destination DC&gt;

NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;secondary DNS Server IP on dest. DC&gt;</code>
<para>如果主机记录中存在无效的 IP 地址，请调查是否 DNS 清理已启用并正确配置。 </para><para>如果上面测试或网络跟踪不会显示返回无效的 IP 地址的名称查询，请考虑在主机文件、 LMHOSTS 文件和 WINS 服务器中的过时条目。 请注意，DNS 服务器可以还配置为执行 WINS 回退名称解析。</para>
</listItem>
        <listItem>
          <para>
            <embeddedLabel>验证服务器应用程序 (Active Directory et al) 已向 RPC 服务器 （源 DC） 上的终结点映射器注册</embeddedLabel>
          </para>
          <para>Active Directory 使用的已知和动态注册端口的组合。 此表列出了由 Active Directory 域控制器使用众所周知的端口和协议。</para>
          <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
            <thead>
              <tr>
                <TD>
                  <para>RPC 服务器应用程序</para>
                </TD>
                <TD>
                  <para>端口</para>
                </TD>
                <TD>
                  <para>TCP</para>
                </TD>
                <TD>
                  <para>UDP</para>
                </TD>
              </tr>
            </thead>
            <tbody>
              <tr>
                <TD>
                  <para>DNS 服务器</para>
                </TD>
                <TD>
                  <para>53</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Kerberos</para>
                </TD>
                <TD>
                  <para>88</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>LDAP 服务器</para>
                </TD>
                <TD>
                  <para>389</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Microsoft-DS</para>
                </TD>
                <TD>
                  <para>445</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>LDAP SSL</para>
                </TD>
                <TD>
                  <para>636</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>全局编录服务器</para>
                </TD>
                <TD>
                  <para>3268</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para />
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>全局编录服务器</para>
                </TD>
                <TD>
                  <para>3269</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para />
                </TD>
              </tr>
            </tbody>
          </table>
          <para>终结点映射程序未向注册已知端口。 </para>
          <para>Active Directory 和其他应用程序也注册接收动态分配的端口的 RPC 临时端口范围的服务。 此类 RPC 服务器应用程序动态分配 1024年和 Windows 2000 和 Windows Server 2003 计算机上的 5000 之间的 TCP 端口以及 Windows Server 2008 和 Windows Server 2008 R2 计算机上的 49152 和 65535 范围之间的端口。 复制使用的 RPC 端口可以是硬编码在注册表中所述的步骤<externalLink><linkText>知识库文章 224196</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink>。 Active Directory 会继续向 EPM 配置为使用硬编码的端口注册。 </para>
          <para>验证的 RPC 服务器应用程序所需的注册其自身的 RPC 端点映射程序 RPC 服务器 （源 DC 在 AD 复制的情况下） 上。 </para>
          <para>有多种方法来完成此任务，但其中一个是安装并在控制台上的源 DC 使用语法从特权的管理员命令提示符处运行 PORTQRY: </para>
          <code>c:\&gt;portquery -n &lt;source DC&gt; -e 135 &gt;file.txt</code>
          <para>在 portqry 输出中，请注意动态注册的"MS NT 目录 DRS 接口"的端口号 (UUID = 351...) 用于<embeddedLabel>ncacn_ip_tcp 协议</embeddedLabel>。 下面的代码段显示了从 Windows Server 2008 R2 DC 和 UUID 的示例是 portquery 输出 / 协议对，专门用于由 Active Directory 中突出显示<embeddedLabel>粗体</embeddedLabel>: </para>
          <code>UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\pipe\lsass] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\PIPE\protected_storage] 
<codeFeaturedElement>UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_ip_tcp:CONTOSO-DC01[49156]</codeFeaturedElement> 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[49157] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[6004]</code>
          <para />
        </listItem>
        <listItem>
          <para>若要解决此错误的其他可能方式：</para>
          <list class="ordered">
            <listItem>
              <para>验证在正常模式下启动时源 DC 和源 DC 上的 OS 和 DC 角色已完全启动。</para>
            </listItem>
            <listItem>
              <para>验证 Active Directory 域服务正在运行。 如果该服务当前已停止或未配置默认启动值，默认启动值重置，重新启动已修改的 DC，然后重试该操作。</para>
            </listItem>
            <listItem>
              <para>验证为 RPC 服务和 RPC Locator 的启动值和服务状态是正确的 OS 版本的 RPC 客户端 （目标 DC） 和 RPC 服务器 （源 DC）。 如果该服务当前已停止或未配置默认启动值，默认启动值重置，重新启动已修改的 DC，然后重试该操作。</para>
              <para>此外，请确保服务上下文与下表中列出的默认设置相匹配。</para>
              <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
                <thead>
                  <tr>
                    <TD>
                      <para>服务</para>
                    </TD>
                    <TD>
                      <para>默认状态 （启动类型） 在 Windows Server 2003 及更高版本 </para>
                    </TD>
                    <TD>
                      <para>在 Windows Server 2000 中的默认状态 （启动类型）</para>
                    </TD>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <TD>
                      <para>远程过程调用</para>
                    </TD>
                    <TD>
                      <para>启动 （自动）</para>
                    </TD>
                    <TD>
                      <para>启动 （自动）</para>
                    </TD>
                  </tr>
                  <tr>
                    <TD>
                      <para>远程过程调用定位符</para>
                    </TD>
                    <TD>
                      <para>Null 或停止 （手动）</para>
                    </TD>
                    <TD>
                      <para>启动 （自动）</para>
                    </TD>
                  </tr>
                </tbody>
              </table>
            </listItem>
            <listItem>
              <para>验证动态端口范围的大小不具有已受约束。 若要枚举的 RPC 端口范围的 Windows Server 2008 和 Windows Server 2008 R2 NETSH 语法如下所示：</para>
              <code>&gt;netsh int ipv4 show dynamicport tcp
&gt;netsh int ipv4 show dynamicport udp
&gt;netsh int ipv6 show dynamicport tcp
&gt;netsh int ipv6 show dynamicport udp</code>
            </listItem>
            <listItem>
              <para>验证 KB 224196 中定义的硬编码的端口定义在动态端口范围内，对于源 Dc OS 版本。</para>
              <para>审阅<externalLink><linkText>知识库文章 224196</linkText> <linkUri> https://support.microsoft.com/kb/224196 </linkUri> </externalLink> ，并确保硬编码的端口处于临时端口范围内，对于源 DC 的操作系统版本。</para>
            </listItem>
            <listItem>
              <para>验证 ClientProtocols 密钥下 HKLM\Software\Microsoft\Rpc 存在并且包含以下 5 个默认值：</para>
              <code>ncacn_http REG_SZ rpcrt4.dll
ncacn_ip_tcp REG_SZ rpcrt4.dll
<codeFeaturedElement>ncacn_nb_tcp REG_SZ rpcrt4.dll</codeFeaturedElement>
ncacn_np REG_SZ rpcrt4.dll
ncacn_ip_udp REG_SZ rpcrt4.dll</code>
            </listItem>
          </list>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_MoreInfo">
    <title>详细信息</title>
    <content>
      <para>
        <embeddedLabel>映射导致 RPC 错误 1753年-2146893022： 与 IP 名称不正确的示例： 目标主体名称不正确</embeddedLabel>
      </para>
      <para>Contoso.com 域包含具有 IP 地址 x.x.1.1 和 x.x.1.2 的 DC1 和 DC2。 "A"的主机 / DC2 的"AAAA"记录正确注册所有针对 DC1 配置的 DNS 服务器上。 此外，在 DC1 上的主机文件包含将 dc2 完全限定的主机名映射到 IP 地址 x.x.1.2 的条目。 更高版本，DC2 的 IP 地址从变为 X.X.1.2 X.X.1.3 和新的成员计算机加入到与 IP 地址 x.x.1.2 域。 AD 复制由触发尝试<ui>立即复制副本</ui>在 Active Directory 站点和服务管理单元中的命令失败，出现错误 1753： 下面的跟踪中所示：</para>
      <code>F# SRC    DEST    Operation 
1 x.x.1.1 x.x.1.2 ARP:Request, x.x.1.1 asks for x.x.1.2
2 x.x.1.2 x.x.1.1 ARP:Response, x.x.1.2 at 00-13-72-28-C8-5E
3 x.x.1.1 x.x.1.2 TCP:Flags=......S., SrcPort=50206, DstPort=DCE endpoint resolution(135)
4 x.x.1.2 x.x.1.1 ARP:Request, x.x.1.2 asks for x.x.1.1
5 x.x.1.1 x.x.1.2 ARP:Response, x.x.1.1 at 00-15-5D-42-2E-00
6 x.x.1.2 x.x.1.1 TCP:Flags=...A..S., SrcPort=DCE endpoint resolution(135)
7 x.x.1.1 x.x.1.2 TCP:Flags=...A...., SrcPort=50206, DstPort=DCE endpoint resolution(135)
8 x.x.1.1 x.x.1.2 MSRPC:c/o Bind: UUID{E1AF8308-5D1F-11C9-91A4-08002B14A0FA} EPT(EPMP) 
9 x.x.1.2 x.x.1.1 MSRPC:c/o Bind Ack: Call=0x2 Assoc Grp=0x5E68 Xmit=0x16D0 Recv=0x16D0 
<codeFeaturedElement>10</codeFeaturedElement> x.x.1.1 x.x.1.2 EPM:Request: ept_map: NDR, DRSR(DRSR) {E3514235-4B06-11D1-AB04-00C04FC2DCD2} [DCE endpoint resolution(135)]
<codeFeaturedElement>11</codeFeaturedElement> x.x.1.2 x.x.1.1 EPM:Response: ept_map: 0x16C9A0D6 - EP_S_NOT_REGISTERED
</code>
      <para>在帧<embeddedLabel>10</embeddedLabel>，目标 DC 通过端口 135 的 Active Directory 复制服务类 UUID E351 查询源 Dc 终结点映射程序... </para>
      <para>在帧<embeddedLabel>11</embeddedLabel>，源 DC，在这种情况下不托管 DC 角色，因此尚未注册 E351 的成员计算机...复制服务使用其本地企业项目管理的 UUID 使用符号错误 EP_S_NOT_REGISTERED 映射到十进制错误 1753年、 十六进制错误 0x6d9 和友好的错误响应"没有更多终结点可从终结点映射程序"。</para>
      <para>更高版本，具有 IP 地址 x.x.1.2 的成员计算机获取升级为 contoso.com 域中的"MayberryDC"的副本。 同样，<ui>立即复制副本</ui>命令用于触发复制，但这一次失败并且具有屏幕上错误"的目标主体名称不正确。" 计算机的网络适配器分配 IP 地址 x.x.1.2<placeholder>是</placeholder>是域控制器时，当前启动进入正常模式并注册了 E351...复制服务使用其本地企业项目管理，但它的 UUID 不拥有 DC2 的名称或安全标识，而且不能解密从 DC1 Kerberos 请求，因此请求现在会失败，错误"的目标主体名称不正确。" 错误映射到十进制错误-2146893022： 十六进制错误 0x80090322 /。 </para>
      <para>此类无效的主机 IP 映射可能由主机中的过时条目 / lmhost 文件主机 A / AAAA DNS 或 WINS 中的注册。 </para>
      <para>摘要：此示例中失败，因为 （在本例中为主机文件） 的无效主机 IP 映射导致目标 DC 来解析到"源"没有 Active Directory 域服务的 DC 服务正在运行 （或甚至安装就此而言） 因此复制不尚未注册 SPN，源 DC 返回错误 1753:。 在第二种情况下，（再次在主机文件中） 的无效主机 IP 映射导致目标 DC 连接到的 DC，必须注册 E351...复制 SPN，但该源有比预期的源 DC 不同的主机名和安全标识，因此这些尝试失败，出现错误-2146893022::目标主体名称不正确。</para>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>故障排除 Active Directory 操作，因错误 1753年:提供的终结点映射程序没有更多的终结点。</linkText> 
      <linkUri> https://support.microsoft.com/kb/2089874 </linkUri> 
    </externalLink> 
<externalLink> <linkText>KB 文章 839880 故障排除 RPC 端点映射程序错误，使用 Windows Server 2003 支持工具从产品 CD</linkText> <linkUri> https://support.microsoft.com/kb/839880 </linkUri> </externalLink> 
<externalLink> <linkText>KB 文章 832017 服务概述和网络端口要求 Windows Server 系统</linkText><linkUri>https://support.microsoft.com/kb/832017/ </linkUri> </externalLink> 
<externalLink> <linkText>KB 文章 224196 限制 Active Directory 复制流量和客户端 RPC 流量流向特定端口</linkText><linkUri> https://support.microsoft.com/kb/224196/ </linkUri> </externalLink> 
<externalLink><linkText>知识库文章 154596 如何配置与防火墙一起使用的 RPC 动态端口分配</linkText><linkUri> https://support.microsoft.com/kb/154596 </linkUri> </externalLink> <externalLink> <linkText>RPC 的工作原理</linkText><linkUri>https://msdn.microsoft.com/library/aa373935(VS.85).aspx</linkUri></externalLink><externalLink><linkText>服务器用于为连接的准备</linkText><linkUri> https://msdn.microsoft.com/library/aa373938(VS.85).aspx </linkUri> </externalLink> 
<externalLink><linkText>客户端如何建立的连接</linkText><linkUri> https://msdn.microsoft.com/library/aa373937(VS.85).aspx </linkUri> </externalLink><externalLink><linkText>注册界面</linkText><linkUri>https://msdn.microsoft.com/library/aa375357(VS.85).aspx</linkUri></externalLink><externalLink><linkText>从而使该服务器在网络上可用</linkText><linkUri>https://msdn.microsoft.com/library/aa373974(VS.85).aspx</linkUri></externalLink><externalLink><linkText>注册终结点</linkText><linkUri>https://msdn.microsoft.com/library/aa375255(VS.85).aspx </linkUri> </externalLink> <externalLink><linkText>侦听客户端调用</linkText><linkUri> https://msdn.microsoft.com/library/aa373966(VS.85).aspx </linkUri> </externalLink><externalLink><linkText>客户端如何建立的连接</linkText><linkUri>https://msdn.microsoft.com/library/aa373937(VS.85).aspx</linkUri></externalLink><externalLink><linkText>限制Active Directory 复制流量和客户端 RPC 流量到特定端口</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink><externalLink><linkText>对于在目标 DC 的 SPNAD DS</linkText><linkUri>https://msdn.microsoft.com/library/dd207688(PROT.13).aspx</linkUri></externalLink></relatedTopics>
</developerConceptualDocument>



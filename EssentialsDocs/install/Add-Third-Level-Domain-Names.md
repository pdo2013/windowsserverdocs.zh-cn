---
title: 添加三级域名
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5b4a362-1881-4024-ae4e-cc3b05e50103
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 64bf24e45155fdd981e2061b3de7ebce1c53b36c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833318"
---
# <a name="add-third-level-domain-names"></a>添加三级域名

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

你可以在“设置域名”向导中为用户添加请求三级域名的功能。 此操作可通过创建和安装由操作系统中的域管理器使用的代码程序集来完成。  
  
## <a name="create-a-provider-of-third-level-domain-names"></a>创建三级域名的提供程序  
 可通过创建和安装为向导提供域名的代码程序集来提供三级域名。 若要执行此操作，请完成以下任务：  
  
-   [对程序集添加 IDomainSignupProvider 接口的实现](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup)  
  
-   [对程序集添加 IDomainMaintenanceProvider 接口的实现](Add-Third-Level-Domain-Names.md#BKMK_DomainMaintenance)  
  
-   [使用验证码签名程序集签名](Add-Third-Level-Domain-Names.md#BKMK_SignAssembly)  
  
-   [在引用计算机上安装该程序集](Add-Third-Level-Domain-Names.md#BKMK_InstallAssembly)  
  
-   [重新启动 Windows Server Domain Name Management 服务](Add-Third-Level-Domain-Names.md#BKMK_RestartService)  
  
###  <a name="BKMK_DomainSignup"></a> 对程序集添加 IDomainSignupProvider 接口的实现  
 IDomainSignupProvider 接口用于为向导添加域服务。  
  
##### <a name="to-add-the-idomainsignupprovider-code-to-the-assembly"></a>向程序集添加 IDomainSignupProvider 代码  
  
1.  通过在“开始”  菜单中右键单击 Visual Studio 2008 并选择“以管理员身份运行” ，以管理员身份打开该程序。  
  
2.  依次单击 **“文件”**、 **“新建”** 和 **“项目”**。  
  
3.  在 **“新建项目”** 对话框中，依次单击 **“Visual C#”** 和 **“类库”**，输入解决方案名称，然后单击 **“确定”**。  
  
4.  重命名 Class1.cs 文件。 例如，MyDomainNameProvider.cs  
  
5.  添加对 Wssg.Web.DomainManagerObjectModel.dll、CertManaged.dll、WssgCertMgmt.dll 和 WssgCommon.dll 文件的引用。  
  
6.  使用语句添加以下内容。  
  
    ```c#  
  
    using System.Collections.ObjectModel;  
    using System.Net;  
    using System.Net.Sockets;  
    using Microsoft.WindowsServerSolutions.Certificates;  
    using Microsoft.WindowsServerSolutions.CertificateManagement;  
    using Microsoft.WindowsServerSolutions.Common;  
    using Microsoft.Win32;  
    ```  
  
7.  更改命名空间和类头以与以下示例匹配。  
  
    ```c#  
    namespace Microsoft.WindowsServerSolutions.RemoteAccess.Domains  
    {  
        public class MyDomainNameProvider : IDomainSignupProvider  
        {  
        }  
    }  
    ```  
  
8.  向类添加 Initialize 方法和所需的变量，该类定义在向导中显示的服务。  
  
    > [!NOTE]
    >  Initialize 方法定义必须唯一的域提供程序标识符。 一种用于执行该操作的典型方法是将 GUID 定义为标识符。 有关创建 GUID 的详细信息，请参阅 [创建 GUID (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098)。  
  
     以下代码示例演示了 Initialize 方法。  
  
    ```c#  
  
    static readonly Guid MyID = new Guid("8C999DF5-696A-47af-822D-94F1673D3397");  
    public Guid ID { get { return MyID; } }  
    public string Name { get { return "My Provider"; } }  
    List<Offering> offerings = new List<Offering>();  
  
    public void Initialize(DomainProviderSettings config)  
    {  
       var offer1 = new Offering()  
       {  
          Description = "My Domain Provider",  
          Name = "Offering 1",  
          ProviderID = ID,  
          MoreInfoUrl = new Uri("http://www.contoso.com"),  
          MembershipServiceName = "My Membership",  
          EulaUrl = new Uri("http://www.contoso.com"),  
       };  
  
       this.offerings.Add(offer1);  
       RegistryKey key =   
          Registry.LocalMachine.CreateSubKey(@"Software\Microsoft\Windows Server\Domain Manager\Settings");  
       key.Close();  
    }  
    ```  
  
9. 添加 GetOfferings 方法，该方法用于返回在上一步中初始化的服务的列表。 以下代码示例演示了 GetOfferings 方法。  
  
    ```c#  
  
    public ReadOnlyCollection<Offering> GetOfferings()  
    {  
       return this.offerings.AsReadOnly();  
    }  
    ```  
  
10. 添加 FindOfferingForDomain 方法，该方法用于从列表中返回服务。 以下代码示例演示了 FindOfferingForDomain 方法。  
  
    ```  
  
    public Offering FindOfferingForDomain(string domain)  
    {  
       // Return the offering that has the domain name.  
       return offerings[0];  
    }  
  
    ```  
  
11. 添加 SetCredentials 方法，该方法用于定义访问服务时所需的凭据。 以下代码示例演示了 SetCredentials 方法。  
  
    ```c#  
  
    private string currentUser { get; set; }  
    private string currentPassword { get; set; }  
  
    public bool SetCredentials(DomainNameRequest request,   
       DomainProviderCredentials credentials, bool validate)  
    {  
       currentUser = credentials.UserName;  
       currentPassword = credentials.Password;  
       if (validate)  
       {  
          return ValidateCredentials();  
       }  
  
       return true;  
    }  
    ```  
  
12. 添加 ValidateCredentials 方法，该方法用于验证由 SetCredentials 定义的凭据。 以下代码示例演示了 ValidateCredentials 方法。  
  
    ```c#  
  
    public static readonly string offerUser = "user1@contoso.com";  
    public static readonly string offerPassword = "password1";  
  
    public bool ValidateCredentials()  
    {  
       if (IsUser())  
          return string.Equals(currentPassword, offerPassword);  
       else  
          return false;  
    }   
  
    private bool IsUser()  
    {  
       return string.Equals(currentUser, offerUser, StringComparison.OrdinalIgnoreCase);  
    }  
    ```  
  
13. 添加 GetAvailableDomainRoots 方法，该方法用于返回在请求中指定的服务所支持的根域名的列表。 此根域名列表不能为空。 以下代码示例演示了 GetAvailableDomainRoots 方法。  
  
    ```c#  
  
    public ReadOnlyCollection<string> GetAvailableDomainRoots(DomainNameRequest request)  
    {  
       List<string> list = new List<string>();  
       list.Add("domain1.com");  
       list.Add("domain1.org");  
  
       return list.AsReadOnly();  
    }  
    ```  
  
14. 添加 GetUserDomainNames 方法，该方法用于返回相对于现有服务当前用户已拥有的域名的列表。 此列表可能为空。 以下代码示例演示了 GetUserDomainNames 方法。  
  
    ```c#  
  
    public static readonly string AvailableDomain1 = "available.domain1.com",  
       AvailableDomain2 = "available.domain2.com";  
    public static readonly string OccupiedDomain1 = "occupied.domain1.com",  
       OccupiedDomain2 = "occupied.domain2.com";  
  
    public ReadOnlyCollection<string> GetUserDomainNames(DomainNameRequest request)  
    {  
       var userDomains = new List<string>();  
       userDomains.Add(OccupiedDomain1);  
       userDomains.Add(AvailableDomain1);  
  
       return userDomains.AsReadOnly();  
    }  
    ```  
  
15. 添加 GetUserDomainQuota 方法，该方法用于返回指定服务所允许的最大域数。 如果最大值不适用，则此方法应返回 0。 以下代码示例演示了 GetUserDomainQuota 方法。  
  
    ```c#  
  
    public int GetUserDomainQuota(DomainNameRequest request)  
    {  
       return 0;  
    }  
    ```  
  
16. 添加 CheckDomainAvailability 方法，该方法用于检查所请求的域名的可用性，并且可返回一组建议。 以下代码示例演示了 CheckDomainAvailability 方法。  
  
    ```c#  
  
    public bool CheckDomainAvailability(DomainNameRequest request,   
       out ReadOnlyCollection<string> suggestions)  
    {  
       suggestions = null;  
  
       return true;  
    }  
    ```  
  
17. 添加 CommitDomain 方法，该方法用于提交请求的域名。 成功完成此方法意味着域名与用户帐户相关联，当状态为 FullyOperational 时将立即调用维护提供程序以检索证书，并且提供程序和服务将激活。 以下代码示例演示了 CommitDomain 方法。  
  
    ```c#  
  
    public DomainStatus CommitDomain(DomainNameRequest request)  
    {              
       ReadOnlyCollection<string> suggestions;  
       if (!CheckDomainAvailability(request, out suggestions))  
       {  
          throw new DomainException(FailureReason.InvalidDomainName, null, null);  
       }  
  
       return DomainStatus.Ready;  
    }  
    ```  
  
18. 添加 ReleaseDomain 方法，该方法用于通知提供程序用户要释放域名。 以下代码示例演示了 ReleaseDomain 方法。  
  
    ```c#  
  
    public bool ReleaseDomain(DomainNameRequest request)  
    {  
       return true;  
    }  
    ```  
  
19. 添加 GetProviderLandingUrl 方法，该方法用于返回域注册工作流程中的登录页面的 URL。 以下代码示例演示了 GetProviderLandingUrl 方法。  
  
    ```c#  
  
    public Url GetProviderLandingUrl(DomainNameRequest request)  
    {  
       return new Url("www.contoso.com");  
    }  
    ```  
  
20. 添加 GetDomainMaintenanceProvider 方法，该方法用于返回域维护任务使用的 IDomainMaintenanceProvider 实例。 将在成功完成 CommitDomain 方法后启动域管理器时调用此方法。 以下代码示例演示了 GetDomainMaintenanceProvider 方法。  
  
    ```c#  
  
    public IDomainMaintenanceProvider GetDomainMaintenanceProvider()  
    {  
       return new MyDomainMaintenanceProvider();  
    }  
    ```  
  
21. 保存但不关闭该项目，因为你将在下一个步骤中添加该项目。 在完成下一个步骤之前，将不能生成项目。  
  
###  <a name="BKMK_DomainMaintenance"></a> 对程序集添加 IDomainMaintenanceProvider 接口的实现  
 IDomainMaintenanceProvider 用于在创建域之后维护域。  
  
##### <a name="to-add-the-idomainmaintenanceprovider-code-to-the-assembly"></a>向程序集添加 IDomainMaintenanceProvider 代码  
  
1.  为域维护提供程序添加类头。 确保为该提供程序定义的名称与之前在 GetDomainMaintenanceProvider 方法中定义的名称匹配。  
  
    ```c#  
  
    public class MyDomainMaintenanceProvider : IDomainMaintenanceProvider  
    {  
    }  
    ```  
  
2.  添加 Activate 方法，该方法用于设置活动提供程序。 以下代码示例演示了 Activate 方法。  
  
    ```c#  
  
    string DomainName { get; set; }  
    protected DomainProviderSettings Settings { get; set; }  
  
    public void Activate(DomainProviderSettings settings,   
       DomainNameConfiguration config, DomainProviderCredentials credentials)  
    {  
       Settings = settings;  
       SetCredentials(credentials);  
       DomainName = config.AutoConfiguredAnywhereAccessFullName.Punycode;  
    }  
    ```  
  
3.  添加 Deactivate 方法，该方法用于停用所有操作。 以下代码示例演示了 Deactivate 方法。  
  
    ```  
  
    public void Deactivate()  
    {  
       //Deactivate all actions  
    }  
  
    ```  
  
4.  添加 SetCredentials 方法，该方法用于更新用户凭据。 例如，可调用此方法以更新已失效的凭据。 以下代码示例演示了 SetCredentials 方法。  
  
    ```c#  
  
    protected DomainProviderCredentials Credentials { get; set; }  
  
    public bool SetCredentials(DomainProviderCredentials credentials)  
    {  
       this.Credentials = credentials;  
  
       return true;  
    }  
    ```  
  
5.  添加 ValidateCredentials 方法，该方法用于验证指定的凭据。 以下代码示例演示了 ValidateCredentials 方法。  
  
    ```c#  
  
    public static readonly string offerUser = "user1@contoso.com";  
    public static readonly string offerPassword = "password1";  
  
    public bool ValidateCredentials()  
    {  
       if (string.Equals(this.Credentials.UserName,   
          offerUser,   
          StringComparison.OrdinalIgnoreCase) &&   
             string.Equals(this.Credentials.Password, offerPassword))  
          return true;  
  
       return false;  
    }  
    ```  
  
6.  添加 GetPublicAddress 方法，该方法用于返回服务器的外部 IP 地址。 以下代码示例演示了 GetPublicAddress 方法。  
  
    ```c#  
  
    public IPAddress GetPublicIPAddress()  
    {  
       string PublicIP = "0.0.0.0";  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          PublicIP = (key == null) ? "0.0.0.0" : key.GetValue("PublicIP", "0.0.0.0").ToString();  
       }  
       IPAddress ip = IPAddress.Parse(PublicIP);  
  
       if (PublicIP == "0.0.0.0")  
       {  
          string strHostName = Dns.GetHostName();  
          IPHostEntry ipEntry = Dns.GetHostEntry(strHostName);  
  
          IPAddress[] addr = ipEntry.AddressList;  
          foreach (IPAddress add in addr)  
          {  
             if (add.AddressFamily == AddressFamily.InterNetwork)  
             {  
                return add;  
             }  
          }  
       }  
       else  
       {  
          return IPAddress.Parse(PublicIP);  
       }  
  
       return null;    
    }  
    ```  
  
7.  添加 SubmitCertificateRequest 方法，该方法用于提交对当前配置的域名的证书申请。  
  
    ```c#  
  
    string cert=null;  
  
    public void SubmitCertificateRequest(string certificateRequest)  
    {  
       cert = CertManaged.SubmitRequest(certificateRequest, CertCommon.CAServerFQDN + "\\" +      
          CertCommon.CAName,   
          Microsoft.WindowsServerSolutions.CertificateManagement.CRFlags.Base64Header,   
          CertCommon.CATemplate,   
          EncodingFlags.Base64);  
    }  
    ```  
  
8.  添加 GetCertificateResponse 方法，该方法用于返回域状态为 FullyOperational 时的证书响应。 可为新证书请求和证书续订请求调用此方法。 以下代码示例演示了 GetCertificateResponse 方法。  
  
    ```c#  
  
    public string GetCertificateResponse(bool renew)  
    {  
       return cert;  
    }  
    ```  
  
9. 添加 SubmitRenewCertificateRequest 方法，该方法用于处理证书续订。 以下代码示例演示了 SubmitRenewCertificateRequest 方法。  
  
    ```c#  
  
    public void SubmitRenewCertificateRequest()  
    {  
       // Add certificate renewal code   
    }  
    ```  
  
10. 添加 UpdateDNSRecords 方法，该方法用于更新提供程序存储的 DNS 记录。 以下代码示例演示了 UpdateDNS 方法。  
  
    ```c#  
  
    public bool UpdateDnsRecords(IList<DnsRecord> records)  
    {  
       string UpdateDNS = "true";  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          UpdateDNS = (key == null) ? "true" : key.GetValue("UpdateDNS", "true").ToString();  
       }  
  
       return UpdateDNS == "true";  
    }  
  
    ```  
  
11. 添加 TestConnection 方法，该方法用于尝试建立与后端服务的连接。 如果此方法要求执行身份验证操作，会引发相应的异常。 以下代码示例演示了 TestConnection 方法。  
  
    ```c#  
  
    public bool TestConnection()  
    {  
       // Add code to test connection  
  
       return true;  
    }  
    ```  
  
12. 添加 GetDomainState 方法，该方法用于返回域的当前状态。 以下代码示例演示了 GetDomainState 方法。  
  
    ```c#  
  
    public DomainState GetDomainState()  
    {  
       string domainstatus = "FullyOperational";  
       long expirationDate = 365;  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          domainstatus = (key == null) ? "Ready" : key.GetValue("DomainStatus", "Ready").ToString();  
          expirationDate = Int64.Parse(key.GetValue("ExpirationDate", "365").ToString());  
       }  
  
       switch (domainstatus)  
       {  
          case "Failed":  
             return new DomainState(DomainStatus.Failed,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "Ready":  
             return new DomainState(DomainStatus.Ready,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "InRenewal":  
             return new DomainState(DomainStatus.InRenewal,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "InRenewalCustomerInterventionRequired":  
             return new DomainState(DomainStatus.InRenewalCustomerInterventionRequired,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "Pending":  
             return new DomainState(DomainStatus.Pending,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "PendingCustomerInterventionRequired":  
             return new DomainState(DomainStatus.PendingCustomerInterventionRequired,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "RenewalFailed":  
             return new DomainState(DomainStatus.RenewalFailed,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          default:  
             return new DomainState(DomainStatus.Unknown,   
                null,   
                DateTime.Now.AddDays(expirationDate));                   
          }  
    }  
    ```  
  
13. 添加 GetCertificateState 方法，该方法用于返回证书的当前状态。 以下代码示例演示了 GetCertificateState 方法。  
  
    ```c#  
  
    public CertificateState GetCertificateState(bool renew)  
    {  
       return new CertificateState(CertificateStatus.Ready, string.Empty);  
    }  
    ```  
  
14. 保存并生成解决方案。  
  
###  <a name="BKMK_SignAssembly"></a> 使用验证码签名程序集签名  
 你必须使用验证码签名进行程序集签名，因为该签名将在操作系统中使用。 有关对程序集签名的详细信息，请参阅 [Signing and Checking Code with Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode)（使用验证码对代码进行签名和检查）。  
  
###  <a name="BKMK_InstallAssembly"></a> 在引用计算机上安装该程序集  
 将程序集放置在引用计算机上的某个文件夹中。 记下该文件夹的路径，因为需要在下一步中将其输入注册表中。  
  
### <a name="add-a-key-to-the-registry"></a>向注册表添加项  
 可通过添加注册表项来定义程序集的特征和位置。  
  
##### <a name="to-add-a-key-to-the-registry"></a>向注册表添加项  
  
1.  在引用计算机上，单击 **“开始”**，输入 **regedit**，然后按 **Enter**。  
  
2.  在左侧窗格中，依次展开 **“HKEY_LOCAL_MACHINE”**、**“SOFTWARE”**、**“Microsoft”**、**“Windows Server”**、**“Domain Managers”** 和 **“Providers”**。  
  
3.  右键单击 **“Providers”**，指向 **“新建”**，然后单击 **“项”**。  
  
4.  输入提供程序的标识符，用作该项的名称。 该标识符是你在 [向程序集添加 IDomainSignupProvider 接口的实现](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup)的步骤 8 中为提供程序定义的 GUID。  
  
5.  右键单击刚刚创建的项，然后单击 **“字符串值”**。  
  
6.  输入 **Assembly** 作为字符串名称，然后按 **Enter**。  
  
7.  右键单击右侧窗格中新的 **Assembly** 字符串，然后单击 **“修改”**。  
  
8.  输入之前创建的程序集文件的完整路径，然后单击 **“确定”**。  
  
9. 再次右键单击该项，然后单击 **“字符串值”**。  
  
10. 输入 **Enabled** 作为字符串名称，然后按 **Enter**。  
  
11. 右键单击右侧窗格中新的 **Enabled** 字符串，然后单击 **“修改”**。  
  
12. 键入 **True**，然后单击 **“确定”**。  
  
13. 再次右键单击该项，然后单击 **“字符串值”**。  
  
14. 输入 **Type** 作为字符串名称，然后按 **Enter**。  
  
15. 右键单击右侧窗格中新的 **Type** 字符串，然后单击 **“修改”**。  
  
16. 输入在程序集中定义的提供程序的完整类名，然后单击 **“确定”**。  
  
###  <a name="BKMK_RestartService"></a> 重新启动 Windows Server Domain Name Management 服务  
 你必须重新启动 Windows Server Domain Management 服务，以便该提供程序对操作系统可用。  
  
##### <a name="restart-the-service"></a>重新启动服务。  
  
1.  单击 **“开始”**，键入 **mmc**，然后按 **Enter**。  
  
2.  如果没有在控制台中列出服务管理单元，请通过完成以下步骤添加该管理单元：  
  
    1.  单击 **“文件”**，然后单击 **“添加/删除管理单元”**。  
  
    2.  在 **“可用的管理单元”** 列表中单击 **“服务”**，然后单击 **“添加”**。  
  
    3.  在 **“服务”** 对话框中，确保选中 **“本地计算机”** ，然后单击 **“完成”**。  
  
    4.  单击 **“确定”** 关闭 **“添加或删除管理单元”** 对话框。  
  
3.  双击 **“服务”**，向下滚动并选择 **“Windows Server Domain Management”**，然后单击 **“重新启动服务”**。  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [部署准备的映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)
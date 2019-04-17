---
title: "添加第级别域名"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="add-third-level-domain-names"></a>添加第级别域名

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

你可以添加的用户请求设置域名称向导中的第三个级别域名功能。 通过创建并安装使用通过域管理器中操作系统代码组件执行此操作。  
  
## <a name="create-a-provider-of-third-level-domain-names"></a>创建第三个级别域名提供的程序  
 通过创建并安装到向导提供域名代码部件可以使第级别域名可用。 若要执行此操作，你完成以下任务：  
  
-   [添加到组件 IDomainSignupProvider 接口的实现](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup)  
  
-   [添加到组件 IDomainMaintenanceProvider 接口的实现](Add-Third-Level-Domain-Names.md#BKMK_DomainMaintenance)  
  
-   [登录验证码签名的装配](Add-Third-Level-Domain-Names.md#BKMK_SignAssembly)  
  
-   [在参考计算机上安装程序集](Add-Third-Level-Domain-Names.md#BKMK_InstallAssembly)  
  
-   [重启 Windows Server 域名称管理服务](Add-Third-Level-Domain-Names.md#BKMK_RestartService)  
  
###  <a name="BKMK_DomainSignup"></a>添加到组件 IDomainSignupProvider 接口的实现  
 IDomainSignupProvider 界面用于在向导添加域产品/服务。  
  
##### <a name="to-add-the-idomainsignupprovider-code-to-the-assembly"></a>若要添加到组件 IDomainSignupProvider 代码  
  
1.  以 administrator 身份打开 Visual Studio 2008，方法是右键单击在计划**开始**菜单并选择**以管理员身份运行**。  
  
2.  单击**文件**，单击**新建**，然后单击**项目**。  
  
3.  在**新项目**对话框中，单击**C#**，单击**类库**，为解决方案，输入一个名称，然后单击**确定**。  
  
4.  重命名 Class1.cs 文件。 例如，MyDomainNameProvider.cs  
  
5.  添加所提到的 Wssg.Web.DomainManagerObjectModel.dll、CertManaged.dll、WssgCertMgmt.dll 和 WssgCommon.dll 文件。  
  
6.  添加以下使用明细表。  
  
    ```c#  
  
    using System.Collections.ObjectModel;  
    using System.Net;  
    using System.Net.Sockets;  
    using Microsoft.WindowsServerSolutions.Certificates;  
    using Microsoft.WindowsServerSolutions.CertificateManagement;  
    using Microsoft.WindowsServerSolutions.Common;  
    using Microsoft.Win32;  
    ```  
  
7.  更改命名空间并类标头匹配下面的示例。  
  
    ```c#  
    namespace Microsoft.WindowsServerSolutions.RemoteAccess.Domains  
    {  
        public class MyDomainNameProvider : IDomainSignupProvider  
        {  
        }  
    }  
    ```  
  
8.  为类别，定义向导中的所提供的产品/服务添加初始化方法和所需的变量。  
  
    > [!NOTE]
    >  初始化方法定义域服务提供商，必须唯一标识符。 若要执行此操作典型方法是作为标识符定义 GUID。 有关创建 GUID 的详细信息，请参阅[创建 Guid (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098)。  
  
     下面的示例代码显示初始化方法。  
  
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
  
9. 添加 GetOfferings 方法，在上一步中返回初始化产品/服务的列表。 下面的示例代码显示 GetOfferings 方法。  
  
    ```c#  
  
    public ReadOnlyCollection<Offering> GetOfferings()  
    {  
       return this.offerings.AsReadOnly();  
    }  
    ```  
  
10. 添加 FindOfferingForDomain 方法，从列表中返回提供的内容。 下面的示例代码显示 FindOfferingForDomain 方法。  
  
    ```  
  
    public Offering FindOfferingForDomain(string domain)  
    {  
       // Return the offering that has the domain name.  
       return offerings[0];  
    }  
  
    ```  
  
11. 添加 SetCredentials 方法，定义访问产品/服务所需的凭据。 下面的示例代码显示 SetCredentials 方法。  
  
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
  
12. 添加 ValidateCredentials 方法，验证定义 SetCredentials 的凭据。 下面的示例代码显示 ValidateCredentials 方法。  
  
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
  
13. 添加 GetAvailableDomainRoots 方法，返回根域名支持的产品，请求中指定的列表。 根域名此列表不为空。 下面的示例代码显示 GetAvailableDomainRoots 方法。  
  
    ```c#  
  
    public ReadOnlyCollection<string> GetAvailableDomainRoots(DomainNameRequest request)  
    {  
       List<string> list = new List<string>();  
       list.Add("domain1.com");  
       list.Add("domain1.org");  
  
       return list.AsReadOnly();  
    }  
    ```  
  
14. 添加 GetUserDomainNames 方法，返回域名的当前用户已经拥有，相对于当前的产品的列表。 此列表可能为空。 下面的示例代码显示 GetUserDomainNames 方法。  
  
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
  
15. 添加返回指定的提供允许的域的最大数目 GetUserDomainQuota 方法。 如果最多不适用，此方法应返回 0。 下面的示例显示了 GetUserDomainQuota 方法。  
  
    ```c#  
  
    public int GetUserDomainQuota(DomainNameRequest request)  
    {  
       return 0;  
    }  
    ```  
  
16. 添加 CheckDomainAvailability 方法，检查请求的域名提供，并且可以返回建议的列表。 下面的示例代码显示 CheckDomainAvailability 方法。  
  
    ```c#  
  
    public bool CheckDomainAvailability(DomainNameRequest request,   
       out ReadOnlyCollection<string> suggestions)  
    {  
       suggestions = null;  
  
       return true;  
    }  
    ```  
  
17. 添加 CommitDomain 方法，提交该请求的域名。 成功完成此方法意味着域名与用户帐户相关联，将立即调用维护提供检索证书，如果状态 FullyOperational，并且提供程序和服务处于活动状态。 下面的示例代码显示 CommitDomain 方法。  
  
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
  
18. 添加 ReleaseDomain 方法，通知用户希望发布域名提供商。 下面的示例代码显示 ReleaseDomain 方法。  
  
    ```c#  
  
    public bool ReleaseDomain(DomainNameRequest request)  
    {  
       return true;  
    }  
    ```  
  
19. 添加 GetProviderLandingUrl 方法，返回登录域注册工作流中的页面的 URL。 下面的示例代码显示 GetProviderLandingUrl 方法。  
  
    ```c#  
  
    public Url GetProviderLandingUrl(DomainNameRequest request)  
    {  
       return new Url("www.contoso.com");  
    }  
    ```  
  
20. 添加 GetDomainMaintenanceProvider 方法，返回 IDomainMaintenanceProvider 用于域维护任务的实例。 和 CommitDomain 方法成功后，域 Manager 启动时，将此方法。 下面的示例代码显示 GetDomainMaintenanceProvider 方法。  
  
    ```c#  
  
    public IDomainMaintenanceProvider GetDomainMaintenanceProvider()  
    {  
       return new MyDomainMaintenanceProvider();  
    }  
    ```  
  
21. 保存项目，不要将其关闭因为将向其添加的下一个过程。 你将无法生成项目，直到你完成下面的过程。  
  
###  <a name="BKMK_DomainMaintenance"></a>添加到组件 IDomainMaintenanceProvider 接口的实现  
 IDomainMaintenanceProvider 用于创建后维护域。  
  
##### <a name="to-add-the-idomainmaintenanceprovider-code-to-the-assembly"></a>若要添加到组件 IDomainMaintenanceProvider 代码  
  
1.  添加域维护服务提供商类标题。 确保您提供程序定义的名称匹配在之前定义 GetDomainMaintenanceProvider 方法的名称。  
  
    ```c#  
  
    public class MyDomainMaintenanceProvider : IDomainMaintenanceProvider  
    {  
    }  
    ```  
  
2.  添加激活方法，它将设置活动的提供商。 下面的示例代码显示激活方法。  
  
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
  
3.  添加停用方法，用于停用的所有操作。 下面的示例代码显示停用的方法。  
  
    ```  
  
    public void Deactivate()  
    {  
       //Deactivate all actions  
    }  
  
    ```  
  
4.  添加 SetCredentials 方法，更新的用户的凭据。 例如，可能会称为此方法，更新将不再有效的凭据。 下面的示例代码显示 SetCredentials 方法。  
  
    ```c#  
  
    protected DomainProviderCredentials Credentials { get; set; }  
  
    public bool SetCredentials(DomainProviderCredentials credentials)  
    {  
       this.Credentials = credentials;  
  
       return true;  
    }  
    ```  
  
5.  添加 ValidateCredentials 方法，验证指定的凭据。 下面的示例代码显示 ValidateCredentials 方法。  
  
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
  
6.  添加 GetPublicAddress 方法，返回服务器外部 IP 地址。 下面的示例代码显示 GetPublicAddress 方法。  
  
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
  
7.  添加 SubmitCertificateRequest 方法，提交当前配置的域名证书申请。  
  
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
  
8.  添加 GetCertificateResponse 方法，如果域状态 FullyOperational 返回证书响应。 这两个新证书请求针对和证书续订请求此方法。 下面的示例代码显示 GetCertificateResponse 方法。  
  
    ```c#  
  
    public string GetCertificateResponse(bool renew)  
    {  
       return cert;  
    }  
    ```  
  
9. 添加 SubmitRenewCertificateRequest 方法，处理证书续订。 下面的示例代码显示 SubmitRenewCertificateRequest 方法。  
  
    ```c#  
  
    public void SubmitRenewCertificateRequest()  
    {  
       // Add certificate renewal code   
    }  
    ```  
  
10. 添加 UpdateDNSRecords 方法，更新 DNS 记录存储提供商。 下面的示例代码显示 UpdateDNS 方法。  
  
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
  
11. 添加 TestConnection 方法，尝试重新建立连接到后端服务。 如果此方法需要身份验证，相应异常应该。 下面的示例代码显示 TestConnection 方法。  
  
    ```c#  
  
    public bool TestConnection()  
    {  
       // Add code to test connection  
  
       return true;  
    }  
    ```  
  
12. 添加 GetDomainState 方法，其中返回的域的当前状态。 下面的示例代码显示 GetDomainState 方法。  
  
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
  
13. 添加 GetCertificateState 方法，它返回证书的当前状态。 下面的示例代码显示 GetCertificateState 方法。  
  
    ```c#  
  
    public CertificateState GetCertificateState(bool renew)  
    {  
       return new CertificateState(CertificateStatus.Ready, string.Empty);  
    }  
    ```  
  
14. 保存和版本解决方案。  
  
###  <a name="BKMK_SignAssembly"></a>登录验证码签名的装配  
 你必须为其用于操作系统中的验证码登录集。 有关集签名的详细信息，请参阅[签名和的验证码检查代码](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode)。  
  
###  <a name="BKMK_InstallAssembly"></a>在参考计算机上安装程序集  
 在参考电脑上的文件夹位置集。 记下的文件夹路径因为您会将其输入注册表中的下一步。  
  
### <a name="add-a-key-to-the-registry"></a>添加注册表项  
 添加注册表项以定义特性和集的位置。  
  
##### <a name="to-add-a-key-to-the-registry"></a>若要添加对注册表项  
  
1.  在参考计算机上，单击**开始**，输入**regedit**，然后按**Enter**。  
  
2.  在左侧窗格中，展开**HKEY_LOCAL_MACHINE**，展开**软件**，展开**Microsoft**，展开**Windows Server**，展开**域经理**，然后展开**提供商**。  
  
3.  右键单击**提供商**，指向**新建**，然后单击**键**。  
  
4.  键名称为提供程序键入的标识符。 标识符为您的第 8 步中提供程序定义的 GUID[组件中添加的 IDomainSignupProvider 界面实现](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup)。  
  
5.  创建，右键单击你刚刚键，然后单击**字符串值**。  
  
6.  键入**装配**字符串，并按的名称**Enter**。  
  
7.  右键单击新**装配**字符串的右窗格中，然后单击**修改**。  
  
8.  键入，你之前创建了，然后单击装配文件的完整路径**确定**。  
  
9. 再次右键单击键，然后单击**字符串值**。  
  
10. 键入**启用**字符串，并按的名称**Enter**。  
  
11. 右键单击新**启用**字符串的右窗格中，然后单击**修改**。  
  
12. 键入**如此**，然后单击**确定**。  
  
13. 再次右键单击键，然后单击**字符串值**。  
  
14. 键入**类型**字符串，并按的名称**Enter**。  
  
15. 右键单击新**类型**字符串的右窗格中，然后单击**修改**。  
  
16. 键入你组件中, 定义的提供商的完整类名称，然后单击**确定**。  
  
###  <a name="BKMK_RestartService"></a>重启 Windows Server 域名称管理服务  
 你必须重新启动的操作系统可用提供商的 Windows Server 域管理服务。  
  
##### <a name="restart-the-service"></a>重启该服务  
  
1.  单击**开始**，类型**mmc**，然后按**Enter**。  
  
2.  如果服务贴靠中未列出控制台中，请将其添加通过完成以下步骤：  
  
    1.  单击**文件**，然后单击**添加/删除管理单元在**。  
  
    2.  在**可用管理单元**列表中，单击**服务**，然后单击**添加**。  
  
    3.  在**服务**对话框框中，请确保**本地计算机**选中，则，然后单击**完成**。  
  
    4.  单击**确定**关闭**添加/删除管理单元**对话框。  
  
3.  双击**服务**、向下滚动并选择**Windows Server 域管理**，然后单击**重启该服务**。  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)
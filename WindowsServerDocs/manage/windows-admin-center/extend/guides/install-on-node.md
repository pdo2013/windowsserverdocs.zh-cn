---
title: 开发工具扩展
description: 开发工具扩展 Windows 管理中心 SDK （Project Honolulu）
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: c5c87be882a32958946198eb6ff1b9d7000577e7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385287"
---
# <a name="install-extension-payload-on-a-managed-node"></a>在托管节点上安装扩展负载

>适用于：Windows Admin Center、Windows Admin Center 预览版

## <a name="setup"></a>安装
> [!NOTE]
> 若要遵循本指南，你将需要生成1.2.1904.02001 或更高版本。 若要检查生成号，请打开 Windows 管理中心并单击右上方的问号。

如果尚未这样做，请创建用于 Windows 管理中心的[工具扩展](../develop-tool.md)。 完成此操作后，请记下创建扩展时使用的值：

| ReplTest1 | 说明 | 示例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 公司名称（包含空格） | ```Contoso``` |
| ```{!Tool Name}``` | 工具名称（包含空格） | ```InstallOnNode``` |

在工具扩展文件夹内创建一个 ```Node``` 文件夹（```{!Tool Name}\Node```）。 使用此 API 时，此文件夹中的任何内容都将复制到托管节点。 添加用例所需的任何文件。 

另外，创建 ```{!Tool Name}\Node\installNode.ps1``` 脚本。 将所有文件从 ```{!Tool Name}\Node``` 文件夹复制到托管节点后，将在托管节点上运行此脚本。 为用例添加任何其他逻辑。 @No__t-0 文件的示例：

``` ps1
# Add logic for installing payload on managed node
echo 'Success'
```

> [!NOTE]
> ```{!Tool Name}\Node\installNode.ps1``` 具有 API 将查找的特定名称。 更改此文件的名称将导致错误。


## <a name="integration-with-ui"></a>与 UI 集成

将 ```\src\app\default.component.ts``` 更新为以下内容：

``` ts
import { Component } from '@angular/core';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Observable } from 'rxjs';

@Component({
  selector: 'default-component',
  templateUrl: './default.component.html',
  styleUrls: ['./default.component.css']
})

export class DefaultComponent {
  constructor(private appContextService: AppContextService) { }

  public response: any;
  public loading = false;

  public installOnNode() {
    this.loading = true;
    this.post('{!Company Name}.{!Tool Name}', '1.0.0',
      this.appContextService.activeConnection.nodeName).subscribe(
        (response: any) => {
          console.log(response);
          this.response = response;
          this.loading = false;
        },
        (error) => {
          console.log(error);
          this.response = error;
          this.loading = false;
        }
      );
  }

  public post(id: string, version: string, targetNode: string): Observable<any> {
    return this.appContextService.node.post(targetNode,
      `features/extensions/${id}/versions/${version}/install`);
  }

}
```
将占位符更新为创建扩展时使用的值：
``` ts
this.post('contoso.install-on-node', '1.0.0',
      this.appContextService.activeConnection.nodeName).subscribe(
        (response: any) => {
          console.log(response);
          this.response = response;
          this.loading = false;
        },
        (error) => {
          console.log(error);
          this.response = error;
          this.loading = false;
        }
      );
```

还要更新 ```\src\app\default.component.html``` 到：
``` html
<button (click)="installOnNode()">Click to install</button>
<sme-loading-wheel *ngIf="loading" size="large"></sme-loading-wheel>
<p *ngIf="response">{{response}}</p>
```
最后 ```\src\app\default.module.ts```：
``` ts
import { CommonModule } from '@angular/common';
import { NgModule } from '@angular/core';

import { LoadingWheelModule } from '@microsoft/windows-admin-center-sdk/angular';
import { DefaultComponent } from './default.component';
import { Routing } from './default.routing';

@NgModule({
  imports: [
    CommonModule,
    LoadingWheelModule,
    Routing
  ],
  declarations: [DefaultComponent]
})
export class DefaultModule { }

```

## <a name="creating-and-installing-a-nuget-package"></a>创建和安装 NuGet 包

最后一步是使用已添加的文件生成 NuGet 包，然后在 Windows 管理中心中安装该程序包。

如果你之前未创建扩展包，请按照[发布扩展](../publish-extensions.md)指南进行操作。 
> [!IMPORTANT]
> 在此扩展的 nuspec 文件中，```<id>``` 值与项目的 @no__t 中的名称匹配，```<version>``` 匹配添加到 @no__t 3 的内容，这一点非常重要。 另外，在 @no__t 下添加一个条目： 
> 
> ```<file src="Node\**\*.*" target="Node" />```。

``` xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
  <metadata>
    <id>contoso.install-on-node</id>
    <version>1.0.0</version>
    <authors>Contoso</authors>
    <owners>Contoso</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <projectUrl>https://msft-sme.myget.org/feed/windows-admin-center-feed/package/nuget/contoso.sme.install-on-node-extension</projectUrl>
    <licenseUrl>http://YourLicenseLink</licenseUrl>
    <iconUrl>http://YourLogoLink</iconUrl>
    <description>Install on node extension by Contoso</description>
    <copyright>(c) Contoso. All rights reserved.</copyright> 
  </metadata>
    <files>
    <file src="bundle\**\*.*" target="ux" />
    <file src="package\**\*.*" target="gateway" />
    <file src="Node\**\*.*" target="Node" />
  </files>
</package>
```

创建此包后，请向该源添加一个路径。 在 Windows 管理中心中转到 "设置" > 扩展 > 源 "，并将路径添加到包所在的位置。 完成安装扩展后，应该可以单击 ```install``` 按钮，将调用 API。  
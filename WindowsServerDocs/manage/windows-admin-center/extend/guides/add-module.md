---
title: 将模块添加到工具扩展
description: 开发工具扩展 Windows Admin Center SDK (Project Honolulu)-将模块添加到工具扩展
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: e6978ce20a7c6da8addb217de8d30f733b40d261
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081204"
---
# 将模块添加到工具扩展

>适用于：Windows Admin Center、Windows Admin Center 预览版

本文中，我们将为我们创建了使用 Windows Admin Center CLI 的工具扩展添加一个空的模块。

## 准备你的环境

如果你尚未这样做，请按照中的说明开发一种[工具](..\develop-tool.md)（或[解决方案](..\develop-solution.md)） 的扩展，可准备环境，并创建新的空工具扩展。

## 使用 Angular CLI 创建模块 （和组件）

如果你不熟悉 Angular，强烈建议你阅读 Angular.Io 网站上的文档，以了解 Angular 和 NgModule。 有关 NgModule 的详细信息，请转至：https://angular.io/guide/ngmodule

* 有关使用 Angular CLI 生成新模块的详细信息，请访问：https://github.com/angular/angular-cli/wiki/generate-module
* 有关使用 Angular CLI 生成新组件的详细信息，请访问：https://github.com/angular/angular-cli/wiki/generate-component


打开命令提示符下，将目录更改为 \src\app 在项目中，然后运行以下命令，替换```{!ModuleName}```你的模块名称 （删除空格）：

```
cd \src\app
ng generate module {!ModuleName}
ng generate component {!ModuleName}
```

| 值 | 说明 | 示例 |
| ----- | ----------- | ------- |
| ```{!ModuleName}``` | 模块名称（删除空格） | ```ManageFooWorksPortal``` |

示例用法：
```
cd \src\app
ng generate module ManageFooWorksPortal
ng generate component ManageFooWorksPortal
```


## 添加路由信息

如果你不熟悉 Angular，强烈推荐你了解 Angular 路由和导航。 以下各节定义了必要的路由元素，可使 Windows Admin Center 导航到扩展并在扩展之间的视图间进行导航，以响应用户活动。 要了解详细信息，请转至：https://angular.io/guide/router

使用上述步骤中使用相同的模块名称。

### 将内容添加到新的路由文件

* 浏览到上一步中由 ``` ng generate ``` 创建的模块文件夹。

* 按照此命名约定，创建新文件 ```{!module-name}.routing.ts```：

    | 值 | 说明 | 示例文件名 |
    | ----- | ----------- | ------- |
    | ```{!module-name}``` | 模块名称（小写，空格替换为短划线） | ```manage-foo-works-portal.routing.ts``` |

* 将此内容添加到刚创建的文件：

    ``` ts
    import { NgModule } from '@angular/core';
    import { RouterModule, Routes } from '@angular/router';
    import { {!ModuleName}Component } from './{!module-name}.component';

    const routes: Routes = [
        {
            path: '',
            component: {!ModuleName}Component,
            // if the component has child components that need to be routed to, include them in the children array.
            children: [
                {
                    path: '', 
                    redirectTo: 'base',
                    pathMatch: 'full'
                }
            ]
    }];

    @NgModule({
        imports: [
            RouterModule.forChild(routes)
        ],
        exports: [
            RouterModule
        ]
    })
    export class Routing { }
    ```

* 将刚创建的文件中的值替换为所需的值：

    | 值 | 说明 | 示例 |
    | ----- | ----------- | ------- |
    | ```{!ModuleName}``` | 模块名称（删除空格） | ```ManageFooWorksPortal``` |
    | ```{!module-name}``` | 模块名称（小写，空格替换为短划线） | ```manage-foo-works-portal``` |

### 将内容添加到新模块文件

打开使用以下命名约定找到的文件 ```{!module-name}.module.ts```：

| 值 | 说明 | 示例文件名 |
| ----- | ----------- | ------- |
| ```{!module-name}``` | 模块名称（小写，空格替换为短划线） | ```manage-foo-works-portal.module.ts``` |

* 将内容添加到文件：

    ``` ts
    import { Routing } from './{!module-name}.routing';
    ```

* 将刚添加的内容中的值替换为所需的值：

    | 值 | 说明 | 示例 |
    | ----- | ----------- | ------- |
    | ```{!module-name}``` | 模块名称（小写，空格替换为短划线） | ```manage-foo-works-portal``` |

* 修改导入语句以导入路由：

    | 原始值 | 新值 |
    | -------------- | --------- |
    | ```imports: [ CommonModule ]``` | ```imports: [ CommonModule, Routing ]``` |

* 确保 ```import``` 语句按源的字母顺序排序。

### 将内容添加到新组件 typescript 文件

打开使用以下命名约定找到的文件 ```{!module-name}.component.ts```：

| 值 | 说明 | 示例文件名 |
| ----- | ----------- | ------- |
| ```{!module-name}``` | 模块名称（小写，空格替换为短划线） | ```manage-foo-works-portal.component.ts``` |
    
将文件中的内容修改为以下内容：

``` ts
constructor() {
    // TODO
}

public ngOnInit() {
    // TODO
}
```
### 更新应用 routing.module.ts

打开文件```app-routing.module.ts```，然后修改默认路径，因此，它会加载你刚创建的新模块。  找到的项```path: ''```，并更新```loadChildren```来加载而不是默认模块模块：

| 值 | 说明 | 示例 |
| ----- | ----------- | ------- |
| ```{!ModuleName}``` | 模块名称（删除空格） | ```ManageFooWorksPortal``` |
| ```{!module-name}``` | 模块名称（小写，空格替换为短划线） | ```manage-foo-works-portal``` |

``` ts
    {
        path: '', 
        loadChildren: 'app/{!module-name}/{!module-name}.module#{!ModuleName}Module'
    },
```
下面是已更新的默认路径的示例：
``` ts
    {
        path: '', 
        loadChildren: 'app/manage-foo-works-portal/manage-foo-works-portal.module#ManageFooWorksPortalModule'
    },
```


## 构建并旁加载你的扩展

你现在已添加到你的扩展的模块。  接下来，你可以[构建并旁加载](..\develop-tool.md#build-and-side-load-your-extension)你的扩展 Windows Admin Center，以查看结果中。

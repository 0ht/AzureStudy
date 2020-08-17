# API Managementを試してみる

https://docs.microsoft.com/ja-jp/learn/modules/publish-manage-apis-with-azure-api-management/3-exercise-create-an-api-gateway

## Web APIをデプロイ

```
git clone https://github.com/MicrosoftDocs/mslearn-publish-manage-apis-with-azure-api-management.git

cd mslearn-publish-manage-apis-with-azure-api-management

bash setup.sh
./setup.sh: line 1: ﻿#!/bin/bash: No such file or directory
Setting username and password for Git ... (1/7)


Creating App Service plan in FREE tier ... (2/7)

{- Finished ..
  "freeOfferExpirationTime": null,
  "geoRegion": "Central US",
  "hostingEnvironmentProfile": null,
  "hyperV": false,
  "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/AzureDevOps-rg/providers/Microsoft.Web/serverfarms/ShoeCoAPI82f79cd6cd",
  "isSpot": false,
  "isXenon": false,
  "kind": "app",
  "location": "Central US",
  "maximumElasticWorkerCount": 1,
  "maximumNumberOfWorkers": 1,
  "name": "ShoeCoAPI82f79cd6cd",
  "numberOfSites": 0,
  "perSiteScaling": false,
  "provisioningState": "Succeeded",
  "reserved": false,
  "resourceGroup": "AzureDevOps-rg",
  "sku": {
    "capabilities": null,
    "capacity": 0,
    "family": "F",
    "locations": null,
    "name": "F1",
    "size": "F1",
    "skuCapacity": null,
    "tier": "Free"
  },
  "spotExpirationTime": null,
  "status": "Ready",
  "subscription": "cacb158e-adcf-4815-b7d4-1dd87212e9a0",
  "tags": null,
  "targetWorkerCount": 0,
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
Command ran in 5.964 seconds (init: 0.047, invoke: 5.917)

Creating API App ... (3/7)

Will set appsetting {'additional_properties': {}, 'name': 'WEBSITE_NODE_DEFAULT_VERSION', 'value': '10.14'}
Local git is configured with url of 'https://myName1c743c62412@shoecoapi82f79cd6cd.scm.azurewebsites.net/ShoeCoAPI82f79cd6cd.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "clientCertExclusionPaths": null,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "shoecoapi82f79cd6cd.azurewebsites.net",
  "deploymentLocalGitUrl": "https://myName1c743c62412@shoecoapi82f79cd6cd.scm.azurewebsites.net/ShoeCoAPI82f79cd6cd.git",
  "enabled": true,
  "enabledHostNames": [
    "shoecoapi82f79cd6cd.azurewebsites.net",
    "shoecoapi82f79cd6cd.scm.azurewebsites.net"
  ],
  "ftpPublishingUrl": "ftp://waws-prod-dm1-013.ftp.azurewebsites.windows.net/site/wwwroot",
  "hostNameSslStates": [
    {
      "hostType": "Standard",
      "ipBasedSslResult": null,
      "ipBasedSslState": "NotConfigured",
      "name": "shoecoapi82f79cd6cd.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "toUpdateIpBasedSsl": null,
      "virtualIp": null
    },
    {
      "hostType": "Repository",
      "ipBasedSslResult": null,
      "ipBasedSslState": "NotConfigured",
      "name": "shoecoapi82f79cd6cd.scm.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "toUpdateIpBasedSsl": null,
      "virtualIp": null
    }
  ],
  "hostNames": [
    "shoecoapi82f79cd6cd.azurewebsites.net"
  ],
  "hostNamesDisabled": false,
  "hostingEnvironmentProfile": null,
  "httpsOnly": false,
  "hyperV": false,
  "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/AzureDevOps-rg/providers/Microsoft.Web/sites/ShoeCoAPI82f79cd6cd",
  "identity": null,
  "inProgressOperationId": null,
  "isDefaultContainer": null,
  "isXenon": false,
  "kind": "app",
  "lastModifiedTimeUtc": "2020-07-31T00:56:22.007000",
  "location": "Central US",
  "maxNumberOfWorkers": null,
  "name": "ShoeCoAPI82f79cd6cd",
  "outboundIpAddresses": "13.89.50.122,13.67.227.115,13.67.227.230,13.67.227.227",
  "possibleOutboundIpAddresses": "13.89.50.122,13.67.227.115,13.67.227.230,13.67.227.227,23.99.136.91,40.122.119.157,40.78.132.166,40.78.129.17,40.122.52.167,40.86.99.202,13.89.172.12",
  "redundancyMode": "None",
  "repositorySiteName": "ShoeCoAPI82f79cd6cd",
  "reserved": false,
  "resourceGroup": "AzureDevOps-rg",
  "scmSiteAlsoStopped": false,
  "serverFarmId": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/AzureDevOps-rg/providers/Microsoft.Web/serverfarms/ShoeCoAPI82f79cd6cd",
  "siteConfig": {
    "acrUseManagedIdentityCreds": false,
    "acrUserManagedIdentityID": null,
    "alwaysOn": null,
    "apiDefinition": null,
    "apiManagementConfig": null,
    "appCommandLine": null,
    "appSettings": null,
    "autoHealEnabled": null,
    "autoHealRules": null,
    "autoSwapSlotName": null,
    "azureMonitorLogCategories": null,
    "connectionStrings": null,
    "cors": null,
    "customAppPoolIdentityAdminState": null,
    "customAppPoolIdentityTenantState": null,
    "defaultDocuments": null,
    "detailedErrorLoggingEnabled": null,
    "documentRoot": null,
    "experiments": null,
    "fileChangeAuditEnabled": null,
    "ftpsState": null,
    "functionsRuntimeScaleMonitoringEnabled": null,
    "handlerMappings": null,
    "healthCheckPath": null,
    "http20Enabled": null,
    "httpLoggingEnabled": null,
    "ipSecurityRestrictions": [
      {
        "action": "Allow",
        "description": "Allow all access",
        "ipAddress": "Any",
        "name": "Allow all",
        "priority": 1,
        "subnetMask": null,
        "subnetTrafficTag": null,
        "tag": null,
        "vnetSubnetResourceId": null,
        "vnetTrafficTag": null
      }
    ],
    "javaContainer": null,
    "javaContainerVersion": null,
    "javaVersion": null,
    "limits": null,
    "linuxFxVersion": null,
    "loadBalancing": null,
    "localMySqlEnabled": null,
    "logsDirectorySizeLimit": null,
    "machineKey": null,
    "managedPipelineMode": null,
    "managedServiceIdentityId": null,
    "metadata": null,
    "minTlsVersion": null,
    "minimumElasticInstanceCount": 0,
    "netFrameworkVersion": null,
    "nodeVersion": null,
    "numberOfWorkers": null,
    "phpVersion": null,
    "powerShellVersion": null,
    "preWarmedInstanceCount": null,
    "publishingPassword": null,
    "publishingUsername": null,
    "push": null,
    "pythonVersion": null,
    "remoteDebuggingEnabled": null,
    "remoteDebuggingVersion": null,
    "requestTracingEnabled": null,
    "requestTracingExpirationTime": null,
    "routingRules": null,
    "runtimeADUser": null,
    "runtimeADUserPassword": null,
    "scmIpSecurityRestrictions": [
      {
        "action": "Allow",
        "description": "Allow all access",
        "ipAddress": "Any",
        "name": "Allow all",
        "priority": 1,
        "subnetMask": null,
        "subnetTrafficTag": null,
        "tag": null,
        "vnetSubnetResourceId": null,
        "vnetTrafficTag": null
      }
    ],
    "scmIpSecurityRestrictionsUseMain": null,
    "scmType": null,
    "tracingOptions": null,
    "use32BitWorkerProcess": null,
    "virtualApplications": null,
    "vnetName": null,
    "webSocketsEnabled": null,
    "websiteTimeZone": null,
    "winAuthAdminState": null,
    "winAuthTenantState": null,
    "windowsFxVersion": null,
    "xManagedServiceIdentityId": null
  },
  "slotSwapStatus": null,
  "state": "Running",
  "suspendedTill": null,
  "tags": null,
  "targetSwapSlot": null,
  "trafficManagerHostNames": null,
  "type": "Microsoft.Web/sites",
  "usageState": "Normal"
}
Command ran in 28.065 seconds (init: 0.046, invoke: 28.018)

Setting the account-level deployment credentials ...(4/7)

{
  "id": null,
  "kind": null,
  "name": "web",
  "publishingPassword": null,
  "publishingPasswordHash": null,
  "publishingPasswordHashSalt": null,
  "publishingUserName": "myName18217eb6fef",
  "scmUri": null,
  "type": "Microsoft.Web/publishingUsers/web"
}
Command ran in 12.404 seconds (init: 0.047, invoke: 12.357)

Setting Git remote...(5/7)

fatal: remote production already exists.

Git add...(6/7)

On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean

Git push... (7/7)

warning: push.default is unset; its implicit value has changed in
Git 2.0 from 'matching' to 'simple'. To squelch this message
and maintain the traditional behavior, use:

  git config --global push.default matching

To squelch this message and adopt the new behavior now, use:

  git config --global push.default simple

When push.default is set to 'matching', git will push local branches
to the remote branches that already exist with the same name.

Since Git 2.0, Git defaults to the more conservative 'simple'
behavior, which only pushes the current branch to the corresponding
remote branch that 'git pull' uses to update the current branch.

See 'git help config' and search for 'push.default' for further information.
(the 'simple' mode was introduced in Git 1.7.11. Use the similar mode
'current' instead of 'simple' if you sometimes use older versions of Git)

Counting objects: 32, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (31/31), done.
Writing objects: 100% (32/32), 17.12 KiB | 0 bytes/s, done.
Total 32 (delta 6), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '2659ff13d1'.
remote: Generating deployment script.
remote: Project file path: .\ShoeCompany\ShoeCompany.csproj
remote: Solution file path: .\ShoeCompany.sln
remote: Generating deployment script for ASP.NET MSBuild16 App
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling ASP.NET Core Web Application deployment with MSBuild16.
remote: Microsoft (R) Build Engine version 16.4.0+e901037fe for .NET Framework
remote: Copyright (C) Microsoft Corporation. All rights reserved.
remote:
remote: Build started 7/31/2020 12:57:38 AM.
remote: ......
remote: Project "D:\home\site\repository\ShoeCompany\ShoeCompany.csproj" on node 1 (Restore target(s)).
remote: Restore:
remote:   Restoring packages for D:\home\site\repository\ShoeCompany\ShoeCompany.csproj...
remote:     GET https://api.nuget.org/v3-flatcontainer/microsoft.visualstudio.azure.containers.tools.targets/index.json
remote:     GET https://api.nuget.org/v3-flatcontainer/microsoft.visualstudio.web.codegeneration.design/index.json
remote:     GET https://api.nuget.org/v3-flatcontainer/swashbuckle.aspnetcore/index.json
remote:     OK https://api.nuget.org/v3-flatcontainer/microsoft.visualstudio.web.codegeneration.design/index.json 65ms
remote:     OK https://api.nuget.org/v3-flatcontainer/microsoft.visualstudio.azure.containers.tools.targets/index.json 90ms
：
remote:   Installing Microsoft.CSharp 4.4.0.
remote:   Installing Microsoft.Extensions.Primitives 2.0.0.
remote: .......................
remote:   Committing restore...
remote:   Generating MSBuild file D:\home\site\repository\ShoeCompany\obj\ShoeCompany.csproj.nuget.g.props.
remote:   Generating MSBuild file D:\home\site\repository\ShoeCompany\obj\ShoeCompany.csproj.nuget.g.targets.
remote:   Writing assets file to disk. Path: D:\home\site\repository\ShoeCompany\obj\project.assets.json
remote:   Restore completed in 58.43 sec for D:\home\site\repository\ShoeCompany\ShoeCompany.csproj.
remote:
remote:   NuGet Config files used:
remote:       C:\DWASFiles\Sites\#1ShoeCoAPI82f79cd6cd\AppData\NuGet\NuGet.Config
remote:
remote:   Feeds used:
remote:       https://api.nuget.org/v3/index.json
remote:
remote:   Installed:
remote:       55 package(s) to D:\home\site\repository\ShoeCompany\ShoeCompany.csproj
remote: Done Building Project "D:\home\site\repository\ShoeCompany\ShoeCompany.csproj" (Restore target(s)).
remote: Project "D:\home\site\repository\ShoeCompany\ShoeCompany.csproj" on node 1 (default targets).
remote: PrepareForBuild:
remote:   Creating directory "bin\Release\netcoreapp2.1\".
remote:   Creating directory "obj\Release\netcoreapp2.1\".
remote: ............
remote: CoreCompile:
remote:   D:\Program Files (x86)\MSBuild-16.4\MSBuild\Current\Bin\Roslyn\csc.exe /noconfig /unsafe- /
：
remote: Done Building Project "D:\home\site\repository\ShoeCompany\ShoeCompany.csproj" (default targets).
remote:
remote: Build succeeded.
remote:     0 Warning(s)
remote:     0 Error(s)
remote:
remote: Time Elapsed 00:01:52.36
remote: Creating app_offline.htm
remote: KuduSync.NET from: 'D:\local\Temp\8d834ecb3f064e7' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: 'appsettings.Development.json'
：
remote: Copying file: 'web.config'
remote: Deleting app_offline.htm
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Triggering recycle (preview mode disabled).
remote: Deployment successful.
To https://myName18217eb6fef:Pw1c58356dc099db58b641b@ShoeCoAPI82f79cd6cd.scm.azurewebsites.net/ShoeCoAPI82f79cd6cd.git
 * [new branch]      master -> master
Setup complete!

***********************    IMPORTANT INFO  *********************

Swagger URL: https://ShoeCoAPI82f79cd6cd.azurewebsites.net/swagger
Swagger JSON URL: https://ShoeCoAPI82f79cd6cd.azurewebsites.net/swagger/v1/swagger.json

```

以下の通り、APIが作成された。素晴らしい。

![picture 17](images/1465e86ef0277ef34b7a353650daee94e7eb3519868955173c67d8c027f237f1.png)  


## API Managementサービスの作成

作成時のスクリーンショット取り忘れ
ポイントは、Consumptionプランはプレビューで、一部のリージョンでしか利用できない。東日本はまだ。

以下、メニュー

![picture 18](images/31fb59d5eda9c430f3a1a52c884f97de191ea13ef9985058b2d81e6df0372262.png)  
![picture 19](images/97757e5c5f2e83f8d054391c07459916fef3807abd102a06b988156f31c42541.png)  

概要
![picture 20](images/ca975a62b4aa62ff6f74899cce79af1972eef1b2461ff2c934466d3b8cec7647.png)  

クイックスタート
![picture 21](images/7e5f013d8d64daa9fe52b88e7d5e41afac4b53390f2ee03cd23ba6411c5a287b.png)  

### APIをインポートする

APIの作成は、以下のように選択してウィザード形式で作成できる。
![picture 22](images/a1edca3848481d46a3bcd5537060a947f6fbaa29cc8295c28e1bac02af1a69bd.png)  

種類 | 詳細
---|---
Blank API | 空の API 定義を使って API をインポートすることができます。 その後、必要なすべてのパラメーターを手動で指定します。
Open API | Open API は、RESTful API 用のすべてのエンドポイントおよび操作と、すべての入力および出力パラメーターを記載した仕様です。 OpenAPI はもともと Swagger と呼ばれていました。
WADL | Web アプリケーション記述言語は、HTTP ベースの Web サービスに関する XML の記述です。 これは WSDL よりも形式が簡単で、より軽量です。
WSDL | Web サービス記述言語は、HTTP だけでなく、あらゆるネットワーク サービスに関する XML の記述です。
ロジック アプリ | ロジック アプリは、ワークフローやさまざまなデータ ソースとの統合を調整および自動化するために使われます。
API アプリ | Azure の API アプリ サービス内でホストされる API です。
関数アプリ | トリガーを介して呼び出すことができるサーバーレスのコードです。

ここでは、先にOpenAPI仕様に準拠したAPI定義込みのアプリを用意しているので、メニューからOpenAPIを選択する。

以下の入力画面が開くが、OpenAPIのURLを指定すると、その他の項目が埋められる。
https://ShoeCoAPI82f79cd6cd.azurewebsites.net/swagger/v1/swagger.json

![picture 23](images/20f7e07097484aa4f7aafaa2707e0ef0a7431f9a22498af9b7764e9e028dc803.png)  

構成後は、以下のような画面になる。必然そうなるのかも知れないが、AWSのそれと似ている。

![picture 24](images/31372c2404a34cfdd226901ed7f79fc26fcca53a4be5bc9bf1eaa5469120ef59.png)  

### ポリシー

ポリシーによって、APIの振る舞いに手を加えることができる。
上記の構成画面では、`base` となっている部分がポリシー。

XML形式で記述する。テンプレートも多数用意されている。

![picture 25](images/47712afecf4cebd8f5c864616cf5a8551a663c4f42ccd27191b524ec13452d91.png)  

### Product

Productは、APIの集合、グループのようなもの。APIは、複数のProductに関連付けることができる。

### APIのテストと視覚化

テストタブを開いてproductIDに値を入力する、send をクリック

![picture 26](images/2fd297c305705f3cbc0579eb3cb5053505cc83e81eee10b3d3deab2dae8bf373.png)  

以下のように結果が返る。

![picture 27](images/1fbe17cd54c8d9bd704b9c936c3b1ef60e5416525eda495dc15ecd32c70d1265.png)  

## サブスクリプションキーを使ってAPIw呼び出す

- サブスクリプション キーは一意の自動生成された文字列で、クライアント要求のヘッダーで渡す必要がある
- このキーは、さまざまな領域にスコープを指定できるサブスクリプションと直接関連する
  - これにより、アクセス許可とポリシーをきめ細かく制御可能
- 3 つの主なサブスクリプションのスコープは次のとおり
  - すべての API
  - 1 つの API
  - プロダクト
- 保護された API を呼び出すアプリケーションでは、要求の呼び出しにキーを含める必要がある
- キーはいつでも再作成可能
- キーはプライマリとセカンダリの2つが用意される
  - 主キーを変更しつつダウンタイムを回避したい場合はセカンダリを利用する
- キーは、リクエストヘッダーもしくはクエリで送信する
  
### サブスクリプションキーを取得する

メニュー：サブスクリプションから確認

![picture 28](images/550282210d3ce06e4a6e8d7d0e986e25868c03c352f1b711ddc212367ac7f28f.png)  

普通にアクセスすると401が返る

```
$ curl https://apim-northwindshoes-toohta.azure-api.net/api/Products
{ "statusCode": 401, "message": "Access denied due to missing subscription key. Make sure to include subscription key when making requests to an API." }
```

キーを指定してアクセス
```
curl --header "Ocp-Apim-Subscription-Key: 625e6ab74cf342e793ddc9c40b40e8a6" https://apim-northwindshoes-toohta.azure-api.net/api/Products
[{"productID":625,"productName":"E660JtSWkKvXUz6M","productLaunchDate":"2020-08-29T02:38:39.9224992+00:00","productLaunchPrice":87},{"productID":163,"productName":"1Aid++5BRehNpRq/",
：
```

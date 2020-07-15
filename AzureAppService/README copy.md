# Azure App Service

## Azure App Serviceとは

AzureのPaaS

### デプロイスロット

その名の通り、アプリの実行のコンテキスト？のようなもの。
例えば、ステージング、本番、などを用意する。

### CI/CDのサポート

 Azure DevOps、GitHub、Bitbucket、FTP、またはローカル Git リポジトり  と統合可能
 AzureDevOpsでは、独自のビルドとリリースプロセスを定義可能

### 自動でスケール

 スケールアップ、スケールアウト、スケールダウン可能

## Webアプリケーションの作成

アプリケーションの作成  
基本  
![picture 3](images/42cb44ddcd9792e07bc1115516cb3b100d6e772cf112e74f5f286b6afc151ee1.png)  

監視  
![picture 4](images/dfde440def95e8f6be0781e7fed6ea6e73b76dc796b6eacaf60fcc95a9c8f4e1.png)  
Azure Insightsは、Azure Monitorの一機能であり、Application Performance Managementサービスを提供する。パフォーマンスの問題個所を特定したり、ユーザーのどのようにアプリケーションを利用しているかを調査したりするのに利用できる。  
言語は .NET、node.js、Java、Python、オンプレ、ハイブリッド、どのパブリッククラウドでも利用できる。
この段階でApplication Insightを有効化できるのは、agent basedのInstrumentationをサポートしている .NET、.ASPを選択したときのみ。  
node.jsはPreview。その他は手動でのInstrumentationが必要。





## 継続的なデプロイの構成

https://support.atlassian.com/bitbucket-cloud/docs/create-a-repository-in-bitbucket-cloud/



### BitBucketリポジトリを用意する

![picture 1](images/40fd837d0e49158a9e294487766fb1f48a7c3cbb1f60b3641988611564caf94f.png)  

![picture 2](images/bf9a72b93f5399076d4c99c2d64c4ad7cded26293601210529bee62b11a4734d.png)  

![picture 3](images/edce965e017c18fad3cd2f6364fbc70cbb669f862d1cea6c65721e18ea6551ac.png)  

![picture 4](images/0c8178efe1347e4cae7b535322cca20daede390220a15372974297a1d3374549.png)  

![picture 5](images/ad154e6ec397074e8c4c50edf29d3f0f0810542f633a8c1b469f2a0997f03cf6.png)  

![picture 6](images/8c39ad402c7a4cb284207c2daacfdf7079656722defe8f78e923d02c1be86516.png)  

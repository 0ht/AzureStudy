# Azure App Service を知る

## 概要

Web アプリケーション、REST API、およびモバイル バックエンドをホストするための HTTP ベースのサービス。  
開発言語は以下を利用可能。

- .NET
- .NET Core
- Java
- Ruby
- Node.js
- PHP
- Python
を利用可能



WindowsベースとLinuxベースがある。
セキュリティ、負荷分散、自動スケーリング、自動管理などの Microsoft Azure の機能を、アプリケーションに追加
Azure DevOps、GitHub、Docker Hub およびその他のソースからの継続的デプロイ、パッケージ管理、ステージング環境、カスタム ドメイン、TLS/SSL 証明書など、DevOps 機能も利用可能。

使用した Azure コンピューティング リソースに応じて課金、アプリが実行されている "App Service プラン" によって決定される。

## App Service プラン

- VMを抽象化したもの。パッケージ、環境、等の運用機能をまとめたものとも考えることができる。
- App Service プランでは、Web アプリを実行するための一連のコンピューティング リソースを定義
- このプランに導入したアプリは、そのプラン定義にしたがって実行される。プランには、以下が定義される
  - リージョン (米国西部、米国東部など)
  - VM インスタンスの数
  - VM インスタンスのサイズ (小、中、大)
  - 価格レベル (Free、Shared、Basic、Standard、Premium、PremiumV2、Isolated)

https://azure.microsoft.com/ja-jp/pricing/details/app-service/windows/

- Free と Shared のレベルでは、アプリは共有 VM インスタンス上の CPU 時間 (分) を受け取り、スケールアウト不可
- アプリを実行すると、App Service プランで構成されているすべての VM インスタンスで実行
  - 複数のアプリが同じ App Service プランにある場合、これらはすべて同じ VM インスタンスを共有
  - App Service プランは App Service アプリのスケール ユニット


### 価格レベルでの分類

- 共有コンピューティング
  - 2 つの基本レベルである Free と Shared
  - 他のお客様のアプリを含む他の App Service アプリと同じ Azure VM 上でアプリを実行します。
  - これらのレベルは共有リソースで実行される各アプリに CPU クォータを割り当て、リソースはスケールアウトできません。
- 専用のコンピューティング
  - Basic、Standard、Premium、PremiumV2
  - アプリを専用の Azure VM 上で実行します。
  - 同じ App Service プラン内のアプリのみが同じコンピューティング リソースを共有します。 レベルが高いほど、スケールアウトに使用できる VM インスタンスが多くなります。
- Isolated
  - 専用の Azure 仮想ネットワーク上で専用の Azure VM が実行されます。
  - アプリに対して、コンピューティングの分離の上にネットワークの分離を提供されます。 最大のスケールアウト機能を提供します。

### コスト

- Shared レベル
  - それぞれのアプリが CPU の分単位のクォータを受け取るので、"各アプリ" は **CPU クォータの時間単位で課金**
- 専用コンピューティング レベル (Basic、Standard、Premium、PremiumV2)
  - App Service プランはアプリがスケールされる VM インスタンスの数を定義するので、App Service プランの "各 VM インスタンス" には時間単位の料金があります。
  - これらの VM インスタンスには、実行されているアプリの数にかかわらず**同じ料金が課金**されます。 予期しない課金を避けるには、App Service プランのクリーンアップに関するページをご覧ください。
- Isolated レベル
  - App Service Environment は、アプリを実行する分離された worker の数を定義し、**"各 worker" は時間単位で課金**されます。
  - さらに、App Service Environment 自体の実行に時間単位の基本料金があります。

使用可能な App Service 機能 (カスタム ドメインの構成、TLS/SSL 証明書、デプロイ スロット、バックアップなど) の使用には**課金されません**。ただし、次のような例外があります。

- App Service ドメイン - Azure での購入時と毎年の更新時に支払います。
- App Service 証明書 - Azure での購入時と毎年の更新時に支払います。
- IP ベースの TLS 接続 - IP ベースの TLS 接続ごとに時間単位の課金がありますが、Standard 以上の一部のレベルでは、1 つの IP ベースの TLS 接続が無料で提供されます。 SNI ベースの TLS 接続は無料です。

### プランの変更

- App Service プランは、いつでもスケールアップまたはスケールダウン可能
- アプリが他のアプリと同じ App Service プランにある場合は、コンピューティング リソースを分離すると、アプリのパフォーマンスが向上
- 課金はプラン単位なので、複数のアプリがある場合、同じプランにするとコスト節約の可能性あり
- 分離の指針は以下の通り
  - アプリが多くのリソースを消費している。
  - 既存のプランの他のアプリから独立してアプリをスケーリングする必要がある。
  - アプリに別の地理的リージョン内のリソースが必要である。

## 使用可能なOS機能

https://docs.microsoft.com/ja-jp/azure/app-service/operating-system-functionality

FreeとShared以外の価格プランでは、専用のVMが払いだされる。
以下は、利用可能なOS機能

### ファイルアクセス

### ネットワークアクセス

### コードの実行・プロセス・メモリ

### 診断ログとイベント

### レジストリアクセス

## デプロイ

https://docs.microsoft.com/ja-jp/azure/app-service/deploy-best-practices

### デプロイのコンポーネント

- ソース
- ビルドパイプライン
- デプロイメカニズム

### デプロイスロット

アプリの実行のコンテキスト？のようなもの。例えば、ステージング、本番、などを用意する。
自動でのスワップも可能
スロット間での割り振りの割合を設定可能

az webapp deployment create / swap



### コードの継続的なデプロイ

コード管理リポジトリのブランチとスロットを対応付ける。（GitFlowブランチ戦略）
本番には直接デプロイせず、ステージングなどにデプロイした後に、**スワップ**することで無停止で切り替えを行える。ロールバックも同じくスワップするだけ。
コンテナ―の場合は、イメージをレジストリに登録し、レジストリからPULLする（タグ書き換えがトリガー）
  
CI/CDのフレームワークとしては以下がある。

- Azure DevOps
- GitHub Actions
- Bitbucket
- FTP またはローカル Git リポジトり  と統合可能

### 言語固有の考慮事項

- Java
  - JAR アプリケーションのデプロイには Kudu zipdeploy/ API を使用し、WAR アプリには wardeploy/ を使用
- Node
  - 既定では、Kudu は Node アプリケーションのビルド ステップ (npm install) を実行
  - Azure DevOps などのビルド サービスを使用している場合、Kudu ビルドは不要
  - Kudu ビルドを無効にするには、false の値を指定して SCM_DO_BUILD_DURING_DEPLOYMENT というアプリ設定を作成
- .NET
  - 既定では、Kudu は .NET アプリケーションのビルド ステップ (dotnet build) を実行
  - Azure DevOps などのビルド サービスを使用している場合、Kudu ビルドは不要
  - Kudu ビルドを無効にするには、false の値を指定して SCM_DO_BUILD_DURING_DEPLOYMENT というアプリ設定を作成

### その他の考慮事項

- ローカルキャッシュ
  - 高可用で実行可能な高パフォーマンスの読み取り専用コンテンツ ストアのみが必要になるような場合は、ローカル キャッシュを使うことでメリットが得られる
- CPU・メモリの高使用率
  - 高負荷時のデプロイは失敗する可能性があるので、デプロイ時に一時的にインスタンスをスケールアップするとよい（デプロイ完了したら元に戻す）

### 自動でスケール

 スケールアップ、スケールアウト、スケールダウン可能
クールダウンは、その期間はスケールが行われないという期間を指定する。
スケールアウトのルールを設定したら、スケールインのルールも必ず作成する。
期間を指定してルールを適用することも可能。

Microsoft.insightが登録されていない、というようなエラーが出た場合、サブスクリプションから、リソース・プロバイダーを開き、そこからMicrosoft.Insightsを有効化（登録）する必要がある。



## セキュリティ


## ネットワーク機能

## VNet統合

## ハイブリッド接続 Azure Relay

## Trafiic Manager 統合




## マネージドIDを使用したSQL Database接続のセキュリティ保護

https://docs.microsoft.com/ja-jp/azure/app-service/app-service-web-tutorial-connect-msi


## 継続的なデプロイの構成

https://support.atlassian.com/bitbucket-cloud/docs/create-a-repository-in-bitbucket-cloud/


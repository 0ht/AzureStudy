# Azure Application InsightsでWebアプリを分析

## 概要

以下のようなケースで利用する。

- アプリケーションの正常性に影響を与える課題と問題を分析し、それに対処する必要がある
- アプリケーションの開発ライフサイクルを改善する必要がある
- ユーザーのアクティビティを分析して、より適切に理解する必要がある

![picture 5](images/5822fff91f0b10962ffedefd83a2f170598cf9795de00b1c5fc9de52ef49aa73.png)  

## App Insightsをアプリケーションと統合する

- Azure PortalでApp Insightsリソースを設定する
- アプリケーションにinstrumentation package をインストールする
  - パッケージによってアプリが監視されてLog Analyticsワークスペースにログデータが送信される
  - インストルメンテーションキーが必要
  - ![picture 18](images/c310090e9499b2e23a4c55465814116480559dcf1b93eaaf6e94914ead1dac6e.png)  
- APPINSIGHTS_INSTRUMENTATIONKEY 環境変数を使用してキーを保持

## アプリケーションへのinstrumentation

node.jsの場合

ライブラリを組み込み

```sh
npm install applicationinsights --save
```

コードで読み込み

```sh
const appInsights = require("applicationinsights");
appInsights.setup("<your-instrumentation_key>");
appInsights.start();
```

メトリックや統計情報が確認できる
![picture 19](images/4c574868b9ac41c9bc455b4be31e0035472131790d6394084d46afcf06a3f9df.png)  

アプリケーションマップで遅いリクエストを確認できる
![picture 20](images/2c2ace5bbc12645e799b241251257d6b9f68bab4242dd7ba86e85feb6f9aefd2.png)  

## 継続的な監視

- Application Insights からは、エラーや、アプリケーションの利用不可などの問題に関するアラートを送信することが可能
- **可用性テスト**を作成して、アプリケーションの正常性を継続的に監視
  - 可用性テストを使用すると、さまざまな地域からアプリケーションの正常性を確認
  - 異なる地域からのアクセス結果が確認できる
- メールまたはテキスト メッセージ、Runbook と Webhook を使用して、自動化された方法でアラートに対応

![picture 21](images/00666603adac595dbff0322b88880534595ef6ab7ea1d89b4eca8ab6497a6e2e.png)  

![picture 22](images/a131316c7452c043ec976c83158aa01c5866be420fce982306f44d7d0556fd57.png)  

### 選択可能な アクション

アクションは結構たくさん選べる

![picture 23](images/13bf139f2344f2da9446e07a5a302ffd7cd1a7555443c19aafefe393dc490044.png)  

## リリース パイプラインを継続的に監視する

- Azureパイプラインと連携
  - [継続的監視を使用した Azure App Service の配置] テンプレートを使用してパイプラインに適用
- Application Insightからのアラートでビルドを停止したり、リリース前にアラートを確認したりすることができる。


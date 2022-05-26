# Azure Logic Appsを知る

## 概要

### Logic Apps を使用して行えること

Logic Apps を使用すると、複数のシステムに接続し、単一のワークフローに調整することができます。 たとえば、ワークフローでは次のことを実行できます。

- イベントが発生したときにメール通知を送信します。
- 組織内のサーバーからファイルを Azure Storage に移動します。
- ソーシャル メディア アクティビティを監視し、感情分析を実行し、投稿が自社のビジネスに重要なものである場合にアラートを作成します。
- 組み込みのコネクタを使用して一般的なビジネス システムに接続します。
- 作成したカスタム コネクタを使用して、一般的ではない、または一意のシステムに接続します。
- Logic Apps では、これらのすべてをコードの再構築や再デプロイを行うことなく実行することができます。

### Logic Apps 内の操作

Logic Apps のコネクタを使用すると、外部サービスと通信することができます。 各コネクタではさまざまな操作が利用可能です。 ワークフローを開始する操作は、トリガーと呼ばれています。 あるタスクを実行する操作は、アクションと呼ばれています。

### Logic Apps トリガー

トリガーにより Logic Apps ワークフローが開始されます。 たとえば、HTTP トリガーでは、電子メールの送信、データ ストアに対する操作の実行、ソーシャル メディア サイトへのコンテンツの投稿にアクションを使用するワークフローが開始されます。

トリガーには、さまざまなシナリオでの使用状況を反映した、いくつかのカテゴリがあります。

- **ポーリング トリガー**
  - 指定された頻度でサービスを呼び出して、新しいデータをチェックします。 新しいデータを利用できる場合、ワークフローが新たに実行されます。
- **プッシュ トリガー**
  - エンドポイント上のデータをリッスンしてイベント発生を待機します。 発生すると、ワークフロー インスタンスが新たに実行されます。
- **定期実行のトリガー**
  - スケジュールに基づいてロジック アプリのインスタンスの作成と実行を行います。

#### Logic Apps コネクタ

Logic Apps には、200 を超える組み込みコネクタが用意されています。 これらは、さまざまなカテゴリに分類されます。

- **組み込みコネクタ**
  - Azure アプリと機能と統合するアクションとトリガーが含まれています。
- **マネージド コネクタ**
  - 他のサービスやシステムを呼び出すトリガーとアクションが含まれています。 これらのコネクタは、サブカテゴリに編成されます。
- **マネージド API コネクタ**
  - Azure Blob Storage、Office 365、Dynamics、Power BI などと統合するアクションとトリガーが含まれています。
- **オンプレミス コネクタ**
  - SQL Server、SharePoint Server、Oracle、ファイル共有のオンプレミス インストールと統合するアクションとトリガーが含まれています。
- **統合アカウント コネクタ**
  - XML を変換および検証したり、フラット ファイルをエンコードおよびデコードしたり、AS2、EDIFACT、および X12 プロトコルを使用して企業間 (B2B) メッセージを処理したりします。
- **エンタープライズ コネクタ**
  - SAP や IBM Message Queue などのエンタープライズ システムへのアクセスを提供します。

https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-gateway-connection



tomoyuki@Azure:~$ bash ./setup-textanalytics.sh
Creating Text Analytics account...
{
  "etag": "\"e900245c-0000-2000-0000-5f23e05c0000\"",
  "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/AzureDevOps-rg/providers/Microsoft.CognitiveServices/accounts/TweetAnalytics",
  "identity": null,
  "kind": "TextAnalytics",
  "location": "CENTRALINDIA",
  "name": "TweetAnalytics",
  "properties": {
    "apiProperties": null,
    "capabilities": [
      {
        "name": "VirtualNetworks",
        "value": null
      }
    ],
    "customSubDomainName": null,
    "dateCreated": "2020-07-31T09:11:56.444178Z",
    "encryption": null,
    "endpoint": "https://centralindia.api.cognitive.microsoft.com/",
    "internalId": "d839ab48b0f040a29e13dcae01f95657",
    "networkAcls": null,
    "privateEndpointConnections": [],
    "provisioningState": "Succeeded",
    "publicNetworkAccess": "Enabled",
    "userOwnedStorage": null
  },
  "resourceGroup": "AzureDevOps-rg",
  "sku": {
    "name": "F0",
    "tier": null
  },
  "tags": null,
  "type": "Microsoft.CognitiveServices/accounts"
}
Done!

Make a note of the following values for use in the rest of this exercise:

Cognitive Services account key: 688dc4a13ee54238bd1214b13b02d883

Cognitive Services account endpoint:

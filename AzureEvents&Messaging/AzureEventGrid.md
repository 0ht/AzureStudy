# Azure Event Grid

- Azure Event Grid は、Azure Service Fabric を基盤として実行されるフル マネージドのイベント ルーティング サービス
- Event Grid は、Azure Blob Storage アカウントや Azure Media Services などのさまざまなソースから Azure Functions や Webhook などのさまざまなハンドラーにイベントを配布
- Event Grid はほとんどの Azure サービスを発行者またはサブスクライバーとしてサポート
  - また、サードパーティのサービスでも使用可能

## Azure Event Grid における、ソースをサブスクライバーに接続する概念

- **イベント**: 発生内容
- **イベント ソース**: イベントの発生場所。
- **トピック**: 発行者がイベントを送信するエンドポイント。
- **イベント サブスクリプション**: イベントを (時には複数のハンドラーに) ルーティングするエンドポイントまたは組み込みメカニズム。 サブスクリプションは、受信イベントをインテリジェントにフィルター処理する目的でハンドラーによっても使用されます。
- **イベント ハンドラー**: イベントに反応するアプリまたはサービス

以下のようなイメージ

![picture 30](images/65fb2f9bc664aa8d9414ebc9a4aec787814e35d481aad92ee8a71cf8c43c4911.png)  

## イベントとは？

以下のようなもの

```Json
[
  {
    "topic": string,
    "subject": string,
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```

イベントとは
フィールド | 説明
------|---
topic | イベント ソースの完全なリソース パス。 この値は Event Grid によって指定されます。
subject | 発行者が定義した、イベントの対象のパス。
id | イベントの一意の識別子。
eventType | このイベント ソース用に登録されたいずれかのイベントの種類。 これは CustomerCreated、BlobDeleted、HttpRequestReceived などに対してフィルターを作成できる値です。
eventTime | プロバイダーの UTC 時刻に基づく、イベントが生成された時刻。
data | イベントの種類に関連する特定の情報。 たとえば、Azure Storage で作成されている新しいファイルに関するイベントには、lastTimeModified 値など、ファイルの詳細が含まれます。 または、Event Hubs イベントには、キャプチャ ファイルの URL が含まれます。 このフィールドは省略可能です。
dataVersion | データ オブジェクトのスキーマ バージョン。 スキーマ バージョンは発行者によって定義されます。
metadataVersion | イベント メタデータのスキーマ バージョン。 最上位プロパティのスキーマは Event Grid によって定義されます。 この値は Event Grid によって指定されます。

## EventGridを使用する場合

- **簡単**: Event Grid では、ソースをサブスクライバーに簡単に接続できます。
- **高度なフィルタリング**: サブスクリプションでは、トピックから受け取るイベントを厳しく制限できます。
- **展開**: 同じイベントやトピックにエンドポイントを無制限にサブスクライブさせることができます。
- **信頼性**: Event Grid では、イベントの配信を、各サブスクリプションに対して最大で 24 時間再試行します。
- **イベントあたり従量課金**: 送信するイベント数のみに対して支払いが行われます。

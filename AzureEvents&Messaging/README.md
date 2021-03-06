# イベントとメッセージ

通信の特性によって、イベントなのか、メッセージのいずれとして扱えばよいのかが規定される。
いずれとして扱うかの判断として、以下の問いを考えてみる。

1. その通信では、送信先に何か処理することを期待しているか
2. その通信では、何かしらのデータやペイロードを送信する必要があるか

いずれもいいえ、であればイベント、いずれかがはいであればメッセージ、と考えるとよい。

## メッセージとは

- メッセージには、1 つのコンポーネントによって生成され、別のコンポーネントで使用される生のデータが含まれる
- メッセージには、データへの参照だけでなく、データ自体が含まれる
- 送信側コンポーネントは、メッセージのコンテンツが宛先コンポーネントによって特定の方法で処理されることを想定している
  - システム全体の整合性は、特定のジョブを実行する送信側と受信側の両方によって決まる場合がある

## イベントとは

- イベントは、何かが発生したことを示す軽量の通知
- 送信されたイベントの受信者が複数のこともあれば、まったくいないこともある
- 多くの場合、イベントは "ファンアウト" を意図
  - つまり、パブリッシャーのそれぞれに対して多数のサブスクライバーが存在
- イベントのパブリッシャーは、受信側コンポーネントがどのようなアクションを実行するかについて何も想定していない
- 一部のイベントは個別のユニットであり、他のイベントに関連しない
- 一部のイベントは、一連の関連する順序付けられたイベントに属する

## キューを使用するメッセージベースの配信選択肢

### Azure Queue Storage

- Queue Storage は、Azure Storage を使用して、REST ベースの単純なインターフェイスを介して世界中のどこからでも安全にアクセスできる多数のメッセージを格納するサービス
- キューには数百万のメッセージを含めることができ、それを所有するストレージ アカウントの容量によってのみ制限される

### Azure Service Bus

#### キュー

- Service Bus は、エンタープライズ アプリケーションを対象とするメッセージ ブローカー システム
- 多くの場合、これらのアプリは複数の通信プロトコルを利用し、さまざまなデータ コントラクトと、より高いセキュリティ要件を持ち、クラウドとオンプレミスの両方のサービスを含むことが可能
- Service Bus は、こうしたシナリオに的を絞って設計された専用のメッセージング インフラストラクチャの上に構築される

#### トピック

- Azure Service Bus トピックはキューと似ていますが、複数のサブスクライバーが存在できる
- キューではなくトピックにメッセージが送信されると、複数のコンポーネントが作業を行うようトリガーできる
- 内部的には、トピックではキューが使用される
- トピックに投稿すると、メッセージがコピーされ、各サブスクリプションのキューにドロップされる
- キューでは、サブスクリプションを処理するコンポーネントがビジー状態で対応できない場合でも、メッセージのコピーは各サブスクリプション ブランチによって処理されるまでキュー内に存在し続ける

### キューのメリット

#### 信頼性の向上

- キューは、分散アプリケーションによって、宛先コンポーネントへの配信が保留されているメッセージの一時保管場所として使用
- ソース コンポーネントでは、メッセージをキューに追加でき、宛先コンポーネントでは、そのメッセージを処理するためにキューの先頭で取得
- キューにより、メッセージ交換の信頼性が向上する
  - 混み合っているときに、宛先コンポーネント側でメッセージを処理する準備が整うまで、メッセージを単純に待機させることができる

#### メッセージの配信保証

配信処理には以下のようなパターンがある。

- **At-Least-Once 配信**
  - このアプローチでは、キューからメッセージを取得するコンポーネントの少なくとも 1 つへ、各メッセージが配信されることを保証
  - ただし、特定の状況では、同じメッセージが複数回配信される可能性がある。たとえば、キューからメッセージを取得する Web アプリのインスタンスが 2 つある場合、通常、各メッセージはこれらのインスタンスの 1 つだけに送信されます。 ただし、一方のインスタンスでメッセージの処理に時間がかかり、タイムアウトになった場合、もう一方のインスタンスにもメッセージが送信される可能性があります。 自分の Web アプリ コードは、この可能性を考慮して設計する必要があります。
- **At-Most-Once 配信**
  - このアプローチでは、各メッセージの配信は保証されず、各メッセージが届かない可能性がごくわずかにあります。
  - ただし、At-Least-Once 配信の場合とは異なり、同じメッセージが 2 回配信されることはありません。 これは、"自動重複データ検出" と呼ばれる場合もあります。
- **先入れ先出し法 (FIFO)**
  - ほとんどのメッセージング システムでは、通常、メッセージはキューに入れられたのと同じ順番でキューから出されますが、この順番が保証されているかどうかを検討する必要があります。 分散アプリケーションにおいてメッセージが正確に正しい順序で処理される必要がある場合、FIFO 保証を含むキュー システムを選択する必要がある

#### トランザクションのサポート

- 一連の処理が、すべて成功するか、そうでない場合はすべて失敗にする、といったトランザクションがサポートされる。

### どのサービスを選択すべきか

以下の場合はQueue Storageを使用する

- キューを通ったすべてのメッセージの監査証跡が必要である。
- キューのサイズが 80 GB を超えると予想される。
- キュー内のメッセージ処理の進行状況を追跡する必要がある。

## イベント処理の選択肢


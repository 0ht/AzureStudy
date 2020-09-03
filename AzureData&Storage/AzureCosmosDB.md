# Azure CosmosDBを知る

## 概要

- DBの中にコンテナ―を作成する（≒テーブルのようなもの）
- スループットをRUとして作成時に指定・予約できる。（400~10万）
  - オートスケールの設定も可能
  - DB単位、コンテナ―単位で設定可能DB単位で設定すると、コンテナ―間で共有される
- パーティションキーとして、データがばらけるようなものを指定する

CosmodDBはこれで学習する
file:///C:/Users/oh_to/OneDrive/%E3%83%89%E3%82%AD%E3%83%A5%E3%83%A1%E3%83%B3%E3%83%88/self-study_Azure_Cosmos_DB_introduction.pdf

格納するデータによって、コールするAPIを使い分ける。

- リージョンを跨ってレプリケートすることができる
  - 複数書き込み可能なマルチマスター構成も可能

整合性レベル
- 有界整合性制約

## Azure Cosmos DBアカウントとは

- Azure Cosmos DB アカウントは、データベースの組織エンティティとして機能する Azure リソース
- 課金の目的で、使用状況が Azure サブスクリプションに関連付けられている
- 各 Azure Cosmos DB アカウントは、Azure Cosmos DB がサポートするさまざまなデータ モデルの 1 つに関連付けられており、アカウントを必要な数だけ作成可能
- データはコンテナーに格納する


## 要求ユニットとは

- 適切なスループットは、ビジネス ニーズに合ったトランザクションの量を確実に処理できるようにするために重要
- Azure Cosmos DB では、書き込み、読み取り、更新、削除を実行するコンテナーのスループットをプロビジョニング
- データベース全体のスループットをプロビジョニングし、それをデータベース内の複数のコンテナーで共有することが可能
  - また、特定のコンテナー専用のスループットをプロビジョニングすることも可能
- Azure Cosmos DB では、要求ユニット (RU) と呼ばれるものを使ってスループットを測定
  - 要求ユニットの使用は 1 秒ごとに測定されるため、単位は 1 秒あたりの要求ユニット (RU/秒) 

### 要求ユニットの基本

- 単一の要求ユニット (1 RU) とは、ドキュメントの ID を使用した、1 KB のドキュメントにおける単一の GET 要求を実行する概算コストと同じ
- 操作に消費する要求ユニット数は、ドキュメント サイズ、ドキュメントのプロパティの数、実行されている操作、追加の概念 (一貫性やインデックス作成ポリシーなど) によって異なる
  
### 要求ユニットの考慮事項

- アイテムのサイズ
  - アイテムのサイズが増加するにつれて、アイテムの読み取りまたは書き込みに消費される RU 数も増加します。
- アイテムのインデックス作成
  - 既定では、アイテムごとにインデックスが自動的に作成されます。 コンテナー内のアイテムの一部にインデックスを作成しないことを選択すると、消費される RU が減ります。
- アイテム プロパティの数
  - すべてのプロパティで既定のインデックス作成を実行した場合、アイテム プロパティの数が増加すると、アイテムを書き込むために消費される RU の数が増加します。
- インデックス付きプロパティ
  - 各コンテナーのインデックス ポリシーによって、既定でインデックスが作成されるプロパティが決まります。 書き込み操作の RU 消費量を減らすには、インデックス付きプロパティの数を制限します。
- データの整合性
  - 強力な有界整合性制約の整合性レベルでは、他の緩やかな整合性レベルと比較して、読み取り操作に約 2 倍の RU を消費します。
- クエリ パターン
  - クエリの複雑さは、操作のために消費される RU の数に影響を与えます。 クエリ操作のコストに影響する要因は次のとおりです。
- クエリ結果の数
  - 述語の数
  - 述語の性質
  - ユーザー定義関数の数
  - ソース データのサイズ
  - 結果セットのサイズ
  - プロジェクション
  - Azure Cosmos DB では、同じデータに対して同じクエリが実行される場合は常に、繰り返しの実行で同じ数の RU を要することが保証されています。
- スクリプトの使用
  - クエリと同様に、ストアド プロシージャとトリガーによって、操作の複雑さに基づいて RU が消費されます。 アプリケーションを開発する場合は、要求使用量ヘッダーを調べて、各操作で消費される RU 量をより深く理解してください。

### スループット上限の超過

- プロビジョニングされたスループットよりも高いスループットを使用しようとすると、要求の使用回数が制限される
- 要求の使用回数が制限された場合、指定した間隔後にもう一度要求を再試行する必要がある
  - .NET SDK を使用する場合は、retry-after ヘッダーに指定した時間を待機してから要求は自動的に再試行される


## Azure Cosmos DB でパーティション キーを選択する

### パーティション戦略とは



### パーティションキーとは

- パーティション キーは、ご利用のデータが Azure によって論理的な区分に編成される際に基準になる値
  - ホット パーティションを避けるために、操作をデータベース全体に均等に分散することを目標とする必要がある
- 各パーティション キーに関連付けられているデータの記憶域スペースは、20 GB を超えることはできない

## グローバル分散

- グローバル配布を利用することで、1つのリージョンから複数のリージョンへデータをレプリケートすることができる
- レプリケートされるリージョンはいつでも追加・削除可能
  - しかも100TB以下だったら30分以内に利用可能になる
- レプリケートの際には、スループットとストレージ容量も同じくレプリケートされる
  - よって、レプリケートした分、倍々でコストがかかる

## 複数のリージョンへの書き込み

- 1 桁の書き込み待機時間
  - マルチマスター アカウントでは書き込み待機時間が改善され、**99% の書き込みでは 10 ミリ秒未満**となりました。マルチマスター以外のアカウントの場合は 15 ミリ秒未満です。
- **99.999% の読み取り/書き込み可用性**
  - マルチマスター アカウントでは書き込み可用性が向上し、99.999% となりました。マルチ マスター以外のアカウントの場合は 99.99% です。
- 無制限の書き込みスケーラビリティとスループット
  - マルチマスター アカウントでは、数十億ものデバイスをサポートするために**無制限の書き込みスケーラビリティとスループット**が提供され、すべてのリージョンに書き込むことができます。
- 組み込みの競合解決
  - マルチマスター アカウントには、グローバルなデータ整合性と一貫性を確保するために**競合解決の 3 つの方法**が用意されています。

### 競合解決

- **Last-Writer-Wins (LWW)**
  - ドキュメント内のユーザー定義の整数プロパティの値に基づいて、競合が解決されます。 既定では、最後に書き込まれたドキュメントを判別するために _ts が使用されます。 Last-Writer-Wins は、既定の競合処理メカニズムです。
- **カスタム - ユーザー定義関数**
  - **コレクションにユーザー定義関数を登録**することで、競合解決を完全に制御できます。 ユーザー定義関数は、特殊な署名による特殊な種類のストアド プロシージャです。 ユーザー定義関数が失敗したか、存在しない場合、Azure Cosmos DB によって、すべての競合が読み取り専用の競合フィードに追加され、非同期的に処理することができます。
- **カスタム - 非同期**
  - Azure Cosmos DB によって**すべての競合がコミット対象から除外され、ユーザーのアプリケーションによる遅延解決のために読み取り専用の競合フィードに登録**されます。 アプリケーションでは、競合解決を非同期で実行し、任意のロジックを使用するか、外部ソース、アプリケーション、またはサービスを参照して競合を解決することができます。

## リージョンのフェールオーバー

- 複数リージョンでデータをレプリケートすると、自動フェールオーバー機構を利用可能
  - リージョン単位でオフラインになった場合に発生する


## 整合性レベル

5つの整合性レベル


整合性レベル | 保証内容
-------|-----
強固 | 線形化可能性。 読み取りでは、項目の最新バージョンを返すことが保証されます。
有界整合性制約 | 一貫性のあるプレフィックス。 書き込みに対して読み取りが、最大でプレフィックス k または間隔 t だけ遅延します。
セッション | 一貫性のあるプレフィックス。 単調読み取り、単調書き込み、自己の書き込みの読み取り、読み取り後の書き込み。
一貫性のあるプレフィックス | 返される更新が、それ以外の全更新の一部のプレフィックスとなります (ギャップなし)。
最終的 | 読み取りは順不同です。

Azure Cosmos DB では、整合性が "セッション"、"一貫性のあるプレフィックス"、"最終的" の場合の読み取りは、整合性が "強固" または "有界整合性制約" の場合の読み取りよりも、要求ユニットでの消費量を 2 倍節約できます。

### "強固" 整合性

"強固" 整合性では、線形化可能性が保証されます。ここでは読み取りに対して最新バージョンのアイテムを返すことが保証されています。
レプリカのマジョリティ クォーラムによってコミットされた後でのみ、書き込み結果が見える状態になることが保証されます。 書き込みは、プライマリとセカンダリのクォーラムとの両方によって同期的かつ永続的にコミットされるか、または中止されるか、のどちらかとなります。 読み取りは常に、マジョリティ リード クォーラムによって認識されます。未コミットの書き込みや部分的な書き込みがクライアントから見えることはなく、読み取りの内容は常に、認識された最新の書き込み結果であることが保証されます。

### "有界整合性制約" 整合性

"有界整合性制約" 整合性では、最大でアイテムの K 個のバージョンまたはプレフィックスあるいは期間 t の分だけ、読み取りが書き込みに対し遅れてもかまわないことが保証されます。
このため、"有界整合性制約" を選択する場合、"遅延" は、アイテムのバージョンの数 K (書き込みに対する読み取りの遅れ) と、期間 t の 2 つの方法で構成できます。
"有界整合性制約" では、"遅延の枠" の中以外では、総合的にグローバルな順序が提供されます。 リージョン内では、"整合性制約期間" 内外の両方で単調読み取りの保証が実現されます。
"有界整合性制約" では、整合性が "セッション" または "最終的" の場合よりも、強力な一貫性が保証されます。 グローバルに配布されたアプリケーションの場合、強固な整合性だけでなく 99.99% の可用性と短い待機時間も求められるシナリオでは、有界整合性制約を使用することをお勧めします。

### "セッション" 整合性

"強固" と "有界整合性制約" 整合性レベルで得られるグローバルな整合性モデルとは異なり、"セッション" 整合性はクライアント セッションを対象とします。
Session 一貫性は、デバイスまたはユーザーのセッションが関係するすべてのシナリオに最適です。これは、モノトニックな読み取り、モノトニックな書き込み、自己の書き込みの読み取り (Read your writes, Write follows reads) が保証されるためです。
Session 一貫性では、セッションの予測可能な一貫性が確保されます。また、書き込みと読み取りの待機時間が最も短い一方で、読み取りスループットは最大です。
"セッション" 整合性が構成された Azure Cosmos DB アカウントでは、任意の数の Azure リージョンをその Azure Cosmos DB アカウントと関連付けることができます。

### "一貫性のあるプレフィックス" 整合性

"一貫性のあるプレフィックス" では、長期間書き込みがない場合にグループ内のレプリカが最終的には収束することが保証されます。
また、読み取りでは決して順不同の書き込みが確認されないことが保証されます。 書き込みが A, B, C の順で実行された場合、クライアントでは A、A,B、または A,B,C が確認されますが、A,C または B,A,C などの順不同が確認されることはありません。
"一貫性のあるプレフィックス" で構成された Azure Cosmos DB アカウントでは、任意の数の Azure リージョンをその Azure Cosmos DB アカウントと関連付けることができます。

### "最終的" 整合性

"最終的" 整合性では、長期間書き込みがない場合にグループ内のレプリカが最終的には収束することが保証されます。
Eventual 一貫性は最も弱い形態の一貫性であり、クライアントは、過去に認識した値よりも古い値を取得する可能性があります。
Eventual の一貫性は、読み取りの一貫性という点は最も弱いですが、読み取りと書き込みの待機時間は最短となります。
Eventual 一貫性が構成された Azure Cosmos DB アカウントでは、任意の数の Azure リージョンをその Azure Cosmos DB アカウントと関連付けることができます。

# Azure Virtual Machineを理解する

## Azure Virtual Machines の概要

VMの事を知る前に、VMがおかれる周辺のネットワーク、ストレージなどの理解が必要。
特に、ネットワークは構成後の変更が困難なので、ネットワークの検討から始めるとよい。

### ネットワークから開始する
  
- 仮想ネットワーク (VNet) は、Azure において、Azure Virtual Machines と他の Azure サービスの間をプライベート接続で結ぶために使用される
- 仮想ネットワーク外部のサービスから、仮想ネットワーク内部のサービスへの接続は**デフォルトで不可**なので、外部サービスへのアクセスを許可するように構成する必要あり
- *VM を導入する前にまずは必ずトポロジを考慮すること！*
- 考慮点
  - 仮想ネットワークに 1 つまたは複数のサブネットを作成可能
  - 各サブネットの先頭から 4 個のアドレスと最後のアドレスは、Azure による使用のために予約されている
  - 既定では、サブネット間にセキュリティ境界がないため、各サブネットのサービスは互いに通信可能
  - ネットワーク セキュリティ グループ (NSG) を設定して、**サブネット間**と **VM 間**のトラフィック フローを制御

### 各 VM デプロイの計画

デプロイ前に以下の様な項目について明確にしておく。

- サーバーの通信相手
- 開いているポート
- 使用される OS
- 使用中のディスク領域の量
- 使用するデータの種類 オンプレミスに存在しないことによる (法的またはその他の) 制約の有無
- サーバーの CPU、メモリ、ディスク I/O の負荷の種類 考慮する必要があるバースト トラフィックの有無

### VM に名前を付ける

- VM 名は、オペレーティング システムの一部として構成されるコンピューター名として使用される
- Windows VM には最大 **15 文字**、Linux VM には**最大 64 文字**の名前を指定

#### ネーミングのサンプル

要素 | 例 | メモ
---|---|---
環境 | dev、prod、QA | リソースの環境を識別
場所 | uw (米国西部)、ue (米国東部) | リソースのデプロイ先のリージョンを識別
インスタンス | 01、02 | 複数の名前付きインスタンスが存在するリソースの場合 (Web サーバーなど)
製品またはサービス | service | リソースがサポートする製品、アプリケーション、サービスを識別
ロール | sql、web、messaging | 関連付けられているリソースのロールを識別

貼り付け元  <https://docs.microsoft.com/ja-jp/learn/modules/intro-to-azure-virtual-machines/2-compile-a-checklist-for-creating-a-vm>  

- Azure リソースとは Azure で管理可能な項目
- データセンターにある物理コンピューターと同じように、VM には処理を実行するために必要な複数の要素がある
  - VM 自体
  - ディスク用のストレージ アカウント
  - 仮想ネットワーク (他の VM およびサービスとの共有)
  - ネットワークで通信するためのネットワーク インターフェイス
  - ネットワーク トラフィックをセキュリティで保護するためのネットワーク セキュリティ グループ
  - パブリック インターネット アドレス (オプション)

### VM の場所（リージョン）を決定する

- 場所の選択に関しては、次の 2 点**も**考慮
  - 第 1 に、**場所によって、使用可能なオプションが制限**される場合がある
    - 各リージョンは、使用可能なさまざまなハードウェアを備えており、一部の構成はすべてのリージョンで使用できるわけではない
  - 第 2 に、**場所によって価格差**がある
    - ワークロードが特定の場所にバインドされない場合は、必要な構成を複数のリージョンでチェックして、最低価格の場所を見つけると、非常にコスト効率が高くなる可能性がある

### VM のサイズを決定する

これがどうしても覚えられない・・・・

- ワークロードの種類を考慮して、利用可能な VM サイズのサブセットから選択
- 基本的なテストと運用向けの B シリーズから、大規模なコンピューティング タスク向けの H シリーズ

Type | サイズ | 説明
-----|-----|---
汎用 | B、Dsv3、Dv3、Dasv4、Dav4、DSv2、Dv2、Av2、DC、DCv2 | バランスのとれた CPU 対メモリ比。 テストと開発、小～中規模のデータベース、および低～中程度のトラフィックの Web サーバーに最適です。
コンピューティングの最適化 | Fsv2 | 高い CPU 対メモリ比。 トラフィックが中程度の Web サーバー、ネットワーク アプライアンス、バッチ処理、アプリケーション サーバーに適しています。
メモリの最適化 | Esv3、Ev3、Easv4、Eav4、Mv2、M、DSv2、Dv2 | 高いメモリ対 CPU 比。 リレーショナル データベース サーバー、中～大規模のキャッシュ、およびメモリ内分析に適しています。
ストレージの最適化 | Lsv2 | ビッグ データ、SQL、NoSQL データベース、データ ウェアハウス、および大規模なトランザクション データベースに最適な、高いディスク スループットと IO。
GPU | NC、NCv2、NCv3、ND、NDv2 (プレビュー)、NV、NVv3、NVv4 | 負荷の高いグラフィックスのレンダリングやビデオ編集、ディープ ラーニングを使用したモデル トレーニングと推論 (ND) に特化した仮想マシン。 1 つまたは複数の GPU で利用できます。
ハイ パフォーマンス コンピューティング | HB、HBv2、HC、H | 高スループットのネットワーク インターフェイス (RDMA) のオプションを備えた、最も高速かつ強力な CPU 仮想マシン。

貼り付け元  <https://docs.microsoft.com/ja-jp/azure/virtual-machines/linux/sizes?toc=/azure/virtual-machines/linux/toc.json> 

- 既存の VM サイズがニーズに合わない場合、サイズを変更

### 価格モデルについて理解する

VMの利用には、コンピューティングとストレージという 2 種類の別々のコストがかかる

#### コンピューティング コスト

- コンピューティングの費用は、時間単位で課金されますが、請求は分単位で行われる
- Windowsはライセンスも料金に含まれるのでちょっと高い
- Azure ハイブリッド特典により、Windows の既存のライセンスを再利用してコストを削減できる場合があり

#### ストレージ コスト

- VM で使用するストレージは別途課金
- VM が停止または割り当て解除されていて、実行中の VM に対する課金が発生しない場合でも、ディスクに使用しているストレージは課金対象になる

オプション | 説明
------|---
従量課金制 | 従量課金制オプションでは、コンピューティング容量に対して秒単位で支払います。長期契約や事前の支払いは不要です。 いつでも起動または停止できるだけでなく、需要に応じてコンピューティング容量を増やしたり減らしたりできます。 このオプションは、実行するアプリケーションのワークロードが短期間である場合、または予想不可能で中断が許容されない場合に適しています。 これは、たとえば VM で簡単なテストを行う場合やアプリを開発する場合に適したオプションです。
Reserved Virtual Machine Instances | Reserved Virtual Machine Instances (RI) オプションは、特定のリージョンにおける 1 年間から 3 年間分の仮想マシンの予約購入です。 前払いでの契約により、重量課金制の料金に比べて最大 72% の割引が受けられます。 RI は柔軟性があり、簡単に交換することや、早期解約料金を支払って返却することができます。 VM を継続して実行する必要がある場合、または予算を予測できるようにする必要があり、しかも、少なくとも 1 年間は VM を使用する契約ができる場合は、このオプションが適しています。

貼り付け元  <https://docs.microsoft.com/ja-jp/learn/modules/intro-to-azure-virtual-machines/2-compile-a-checklist-for-creating-a-vm> 

### VM 用のストレージ

- すべての Azure 仮想マシンには、少なくとも 2 個の仮想ハード ディスク (VHD) があり
  - 1 つ目のディスクには**オペレーティング システム**が格納され
  - 2 つ目のディスクは**一時ストレージ**として使用
  - 最大数は、選択した VM サイズによって異なります (通常は CPU ごとに 2 個)
- 各 VHD のデータは、**Azure Storage にページ BLOB として保持**
- ストレージ アカウントは、特定のサブスクリプション向けに、Azure Storage 内のオブジェクトへのアクセスを提供
  
- Standard または Premium ストレージ
  - **Azure Premium Storage**
    - ソリッド ステート ドライブ (SSD) を利用して、I/O を集中的に行うワークロードを実行する VM の高パフォーマンス、低遅延アクセスを提供
    - Azure Premium Storage は運用環境のワークロードに使用
    - 特に、パフォーマンスの変動の影響を受けやすいワークロードや、I/O を集中的に行うワークロードに向く
  - **Azure Standard Storage**
    - 開発またはテスト用には、Standard ストレージで十分

- アンマネージド ディスクまたはマネージド ディスク

オプション | 説明
------|---
アンマネージド ディスク | アンマネージド ディスクを使用する場合、VM ディスクに対応する VHD を保持するために使用するストレージ アカウントを自分で管理する必要があります。 使用する領域の容量に対するストレージ アカウントの料金を支払います。 1 つのストレージ アカウントには、I/O 操作数 20,000/秒という固定レート制限があります。これは、1 つのストレージ アカウントが最大使用率で 40 個の標準仮想ハード ディスクをサポートできることを意味します。 ディスクを増加してスケールアウトする必要がある場合は、ストレージ アカウントを増やす必要があり、複雑になる可能性があります。
マネージド ディスク | マネージド ディスクは、より新しい、お勧めのディスク ストレージ モデルです。 ストレージ アカウントを管理する負担を Azure に移すことで、この複雑さを適切に解決します。 4 TB までのディスク サイズを指定します。Azure により、ディスクとストレージの "両方" が作成、管理されます。 ストレージ アカウント制限について気にする必要がないため、マネージド ディスクは簡単にスケールアウトできます。

貼り付け元  <https://docs.microsoft.com/ja-jp/learn/modules/intro-to-azure-virtual-machines/2-compile-a-checklist-for-creating-a-vm> 

### オペレーティング システムを選択する

- Azure Marketplace で、OS と特定のシナリオ用にインストールされた一般的なソフトウェア ツールを含む、より高度なインストール イメージを検索
- 適切な OS イメージが見つからない場合は、必要なものを含む自分のディスク イメージを作成して Azure ストレージにアップロードし、それを使用して Azure VM を作成可能
- Azure では 64 ビット オペレーティング システムのみがサポート

### Azure 仮想マシンを作成および管理するためのオプションについて

#### Azure Resource Manager

- Azure Resource Manager によって、関連リソースをより効率的に操作できるようになる
  - リソースが名前付きの**リソース グループ**にまとめられるため、ユーザーはすべてのリソースを、まとめてデプロイ、更新、または削除できる
- Resource Manager では、特定の構成を作成およびデプロイするために使用できる**テンプレート** も作成可能
- スクリプトではなく宣言型のテンプレートを使用してインフラストラクチャを管理
- ソリューションのリソースを個別に処理するのではなく、すべてのリソースをグループとしてデプロイ、管理、監視
- ソリューションを開発のライフサイクル全体でデプロイ
  - リソースは、必ず一貫した状態でデプロイされる。
- 正しい順序でデプロイされるように、**リソース間の依存関係**を定義可能
- ロールベースのアクセス制御 (RBAC) が管理プラットフォームにネイティブ統合されるため、すべてのサービスにアクセス制御を適用可能
- タグをリソースに適用し、サブスクリプションのすべてのリソースを論理的に整理
- 同じタグを共有するリソース グループのコストを表示することで、組織の課金をわかりやすくする

Resource Manager テンプレートの詳細は[こちら](./../AzureResourceManager/ARMTemplate.md)

#### Azure PowerShell

- PowerShell とは、シェル ウィンドウやコマンド解析などのサービスを提供する、クロスプラットフォームのシェル  
- Azure PowerShell は、(コマンドレットと呼ばれる) Azure 固有のコマンドを追加する、オプションのアドオン パッケージ

#### Azure CLI

- クロスプラットフォーム コマンド ライン ツール
  - macOS、Linux、および Windows で使用可能
- ブラウザーで Cloud Shell として使用可能
  - Azure PowerShell のように、Azure CLI は、管理ワークフローを効率化する強力な方法です。 Azure CLI では、Azure PowerShell とは異なり、機能に PowerShell は不要
- Azure CLI は、Ruby や Python などのその他のスクリプト言語で使用可能

#### プログラム (API)

- Azure クライアント SDK。Azure クライアント SDK では、Azure REST API をカプセル化
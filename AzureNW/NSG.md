# Network Security Group を知る

## 概要

ネットワーク セキュリティ グループでは、Azure リソースとの間のネットワーク トラフィックがフィルター処理されます。 ネットワーク セキュリティ グループにはセキュリティ規則が含まれ、これを設定してインバウンドまたはアウトバウンドのトラフィックを許可または拒否します。 ネットワーク セキュリティ グループを使って、仮想ネットワーク内およびインターネットからの、仮想マシンやサブネット間のトラフィックをフィルター処理できます。

## 割り当てと評価

- NSGはネットワーク インターフェイスかサブネットに割り当て
- サブネットに割り当てると、その規則はそのサブネット内にあるすべてのネットワーク インターフェイスに適用
  - ネットワーク セキュリティ グループを仮想マシンのネットワーク インターフェイスに関連付けることで、トラフィックをさらに制限
- サブネットとネットワーク インターフェイスの両方にネットワーク セキュリティ グループを適用すると、各ネットワーク セキュリティ グループが個別に評価される
  - インバウンド トラフィックは、サブネットネットワーク インターフェイスの順
  - アウトバウンド トラフィックは、まずネットワーク インターフェイスに適用されたネットワーク セキュリティ グループによって評価され、次にサブネット
- 各サブネットとネットワーク インターフェイスには、**それぞれ 1 つのネットワーク セキュリティ グループを適用**
- ネットワーク セキュリティ グループは **TCP、UDP、ICMP に対応**しており、OSI モデルのレイヤー 4 で動作

## セキュリティ規則

以下のプロパティを持つ

プロパティ | 説明
------|---
名前 | ネットワーク セキュリティ グループ内で一意の名前。
優先度 | 100 から 4096 の数値。
送信元または送信先 | すべて、または個別の IP アドレス、クラスレス ドメイン間ルーティング (CIDR) ブロック (例: 10.0.0.0/24)、サービス タグ、またはアプリケーション セキュリティ グループ。
プロトコル | TCP、UDP、またはすべて。
方向 | 規則が受信トラフィックまたは送信トラフィックに適用されるかどうか。
ポート範囲 | 個々のポートまたはポートの範囲。
アクション | トラフィックの許可または拒否。

### その他の特徴

- 接続はステートフルになります。 同じ TCP/UDP セッションでは、戻りのトラフィックは自動的に許可

### 既定のセキュリティ規則

#### インバウンド

優先度 | 規則名 | 説明
----|-----|---
65000 | AllowVnetInbound | サブネット内の VM から VM に送信されるインバウンドを許可します。
65001 | AllowAzureLoadBalancerInbound | 既定のロード バランサーからサブネット内の VM へのトラフィックを許可します。
65500 | DenyAllInBound | 外部の送信元から VM へのトラフィックを拒否します。

#### アウトバウンド

優先度 | 規則名 | 説明
----|-----|---
65000 | AllowVnetOutbound | サブネット内の VM から VM に送信されるアウトバウンドを許可します。
65001 | AllowInternetOutbound | VM からインターネットに送信されるアウトバウンド トラフィックを許可します。
65500 | DenyAllOutBound | 内部の VM から仮想ネットワークの外部のシステムへのトラフィックを拒否します。

## 拡張セキュリティ規則

拡張セキュリティ規則を使用することで、管理が簡便に

- 複数の IP アドレス
- 複数のポート
- サービス タグ
- アプリケーション セキュリティ グループ
  
### サービスタグ

- サービス タグは IP アドレスのグループを表し、セキュリティ規則の構成を簡素化する
- サービス タグは Microsoft によって管理されます (独自のものを作成することはできません)。 

#### タグの例

- **VirtualNetwork** - このタグは、Azure 内の (ハイブリッド接続を使っている場合は、さらにオンプレミスのネットワーク内の) あらゆる仮想ネットワーク アドレスを表します。
- **AzureLoadBalancer** - このタグは、Azure のインフラストラクチャのロード バランサーを表します。 このタグは、Azure の正常性プローブの送信元となるホストの仮想 IP アドレス (168.63.129.16) に変換されます。
- **Internet** - このタグは、仮想ネットワーク アドレスの外にあり、パブリックに到達できるあらゆるものを表します (パブリック IP アドレスを持つリソースなど)。 このようなリソースの 1 つは、Azure App Service の Web Apps 機能です。
- **AzureTrafficManager** - このタグは Azure Traffic Manager の IP アドレスを表します。
- **Storage** - このタグは、Azure Storage の IP アドレス空間を表します。 トラフィックを許可するか、拒否するかを指定できます。 特定のリージョンにのみアクセスを許可するかどうかを指定することもできますが、個別のストレージ アカウントを選択することはできません。
- **SQL** - このタグは、Azure SQL Database、Azure Database for MySQL、Azure Database for PostgreSQL、Azure SQL Data Warehouse サービスのアドレスを表します。 トラフィックを許可するか、拒否するかを指定できます。また、特定のリージョンに制限することができます。
- **AppService** - このタグは、Azure App Service 用のアドレス プレフィックスを表します。

## アプリケーション セキュリティ グループ

- 特定のアプリケーションで使用されるリソースのネットワーク セキュリティを構成できます。 IP アドレスやサブネットの割り当てに関係なく、仮想マシンは論理的にグループ化
- アプリケーション セキュリティ グループを使うと、ネットワーク インターフェイスをグループ化、次に、そのアプリケーション セキュリティ グループを、ネットワーク セキュリティ グループ内で送信元または送信先の規則として使えます

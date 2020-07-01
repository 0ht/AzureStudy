# NWの監視とトラブルシューティング

ネットワークの監視、診断、分析、トラブルシュートについて。
主にNetwork Watcherを使用する。

## Network Watcherとは

Azureネットワークの正常性を診断するサービス。以下2つのカテゴリに分類される。

- 監視
- 診断

## Network Watcherの**監視**ツール

### トポロジツール

ネットワークのリソース、相互接続、をグラフィカルに表示
トポロジを生成するには、仮想ネットワークと同じリージョンに Network Watcher インスタンスが必要

### 接続モニター

Azure リソース間の接続が機能していることを確認する。2 つの VM が通信できることをこのツールを使用して確認。
リソース間の待機時間も測定。接続モニターでは、VM の監視に加えて、IP アドレスまたは完全修飾ドメイン名 (FQDN) を調べることも可能。

### Network Performance Monitor

待機時間とパケットの破棄を追跡。
Network Performance Monitor を使用して、エンドポイント間の接続を監視できます。

- 分岐とデータセンター間
- 仮想ネットワーク間
- オンプレミスとクラウド間の接続
- Azure ExpressRoute 回線

## Network Watcherの**診断**ツール


### IP フロー検証

特定の仮想マシンに対してパケットが許可または拒否されているかどうかを示します

### 次ホップ

VM から任意の宛先へのパケットの移動方法を決定することができます。 送信元 VM、送信元ネットワーク アダプター、送信元 IP アドレス、および送信先 IP アドレスを指定します。 そうすると、ツールによってパケットの送信先が決定されます。 このツールを使用すると、不正確なルーティング テーブルに起因する問題を診断

### セキュリティ グループ ビュー

NSGの有効性を確認する

### パケット キャプチャ

VM 間で送受信されたすべてのパケットを記録するには、パケット キャプチャ ツールを使用します。 その後、キャプチャされたものを確認して、ネットワーク トラフィックに関する統計情報を収集したり、プライベート仮想ネットワークの予期しないネットワーク トラフィックなどの異常を診断したり
既定の使用制限は、リージョンあたり 100 パケット キャプチャ セッションで、全体の上限は 10,000 
パケット キャプチャは、VM にインストールされている Network Watcher エージェント VM 拡張機能に依存

### 接続のトラブルシューティング

送信元と送信先の VM 間の TCP 接続を確認するために使用します。 送信先 VM は、FQDN、URI、または IP アドレスを使用して指定

### VPN のトラブルシューティング

仮想ネットワーク ゲートウェイ接続に関する問題を診断



## ポイント
# Azureネットワークインフラストラクチャー

## Azure 仮想ネットワークについて知る

- サブネットを構成してネットワークを分離・セグメント化
- パブリックIP、パブリックLBを構成することでインターネット通信
- リソース間の通信
  - 仮想ネットワーク
  - サービスエンドポイント
  - VNet ピアリング
- オンプレミスとの通信
  - **ポイント対サイト：直接**
  - **サイト間：ゲートウェイ経由**
  - **Azure ExpressRoute：専用線**
- ルーティングをオーバーライド可能
  - ルートテーブル
  - Border Gateway Protocol：オンプレBGPルートを仮想ネットワークに反映、Azure VPN Gateway or ExpressRoute 

## 以下詳細
- [VPNゲートウェイ](./VPN.md)
  - [ポイント：サイトでVPNを構成する](./tryP2SVPNGateway.md)
- [仮想ネットワークピアリング](./VNetPeering.md)
  - []
- [Azure Load Balancer](./LoadBalancer.md)
  - [パブリックLoad Balancer](./tryPublicLoadBalancer.md)
- [Azure Traffic Manager](./TrafficManager.md)
  - [優先ルーティングでTraffic Managerを構成する](./tryTrafficManagerPriorityRouting.md)
- [ExpressRoute](./ExpressRoute.md)

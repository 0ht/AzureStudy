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
  - [仮想ネットワークピアリングを構成してみる](./tryVNetPeering.md)
- [Azure Load Balancer](./LoadBalancer.md)
  - [パブリックLoad Balancer](./tryPublicLoadBalancer.md)
- [Azure Traffic Manager](./TrafficManager.md)
  - [優先ルーティングでTraffic Managerを構成する](./tryTrafficManagerPriorityRouting.md)
- [ExpressRoute](./ExpressRoute.md)

## ポイント

- サイト間の通信の問題判別には、Network Performance Monitorを利用する
- Global VNet Peeringを構成した場合、Basic Load Balancer経由ではVMに接続できない。Standardを利用する必要がある。
- Test-AzNetworkWatcherIPFlowでコネクションの確認を行う。Get-AzEffectiveNetworkSecurityGroupでなぜルールが許可・ブロックしているか確認できる？
- サブネット間の通信を厳密に管理したい場合にはASGを作成し、NSGでASGタグを指定する
- 既存のドメインをAzureに移行するには、ゾーンをAzure DNSに委譲する
- Network Performance MoitorをしようしてPでするときは、ICPとTCPが使用される。Windowsでは生のTCPソケットが利用できないのでICMPを使用。
- NSGではサービスタグを活用するとよい(Internet など)
- VPNのSKUを覚えておく
- Standard SKU Public IPはデフォルトではインバウンド接続が有効になっていないので、NSGを設定して通信を許可する必要がある。Staticのみ許可
- VNet to VNet 接続をAcitive-Activeで構成可能
- IPv6構成する場合、アドレスは自動で振られないので明示的に追加する必要がある。VMとVMの通信にIPV6使えない？ので、LB経由で構成する必要がある？
- VMにNSGを追加するPowerShellは、Set-AzNetworkInterface
- PowerShellでのNSG設定方法、New-AzNetworkSecurityRuleConfig 、New-AzNetworkSecurityGroup
- Peeringが構成されたVnetにサブネットの追加・削除は不可

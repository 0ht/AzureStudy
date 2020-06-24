# ポイント対サイトのVPNゲートウェイを作成する

## 1.RouteBased VPN ゲートウェイを作成します。

## 2.認証の目的でルート証明書の公開キーをアップロードします。

## 3.ルート証明書からクライアント認定資格証を生成し、その後、認証の目的で仮想ネットワークに接続する各クライアント コンピューターにクライアント認定資格証をインストールします。

## 4.クライアントが仮想ネットワークに接続するために必要な情報を含む、VPN クライアント構成ファイルを作成します。




$VNetName  = "VNetData"
$FESubName = "FrontEnd"
$BESubName = "Backend"
$GWSubName = "GatewaySubnet"
$VNetPrefix1 = "192.168.0.0/16"
$VNetPrefix2 = "10.254.0.0/16"
$FESubPrefix = "192.168.1.0/24"
$BESubPrefix = "10.254.1.0/24"
$GWSubPrefix = "192.168.200.0/26"
$VPNClientAddressPool = "172.16.201.0/24"
$ResourceGroup = "VpnGatewayDemo"
$Location = "japaneast"
$GWName = "VNetDataGW"
$GWIPName = "VNetDataGWPIP"
$GWIPconfName = "gwipconf"

PPP アダプター VNetData:

   接続固有の DNS サフィックス . . . . .:
   説明. . . . . . . . . . . . . . . . .: VNetData
   物理アドレス. . . . . . . . . . . . .:
   DHCP 有効 . . . . . . . . . . . . . .: いいえ
   自動構成有効. . . . . . . . . . . . .: はい
   IPv4 アドレス . . . . . . . . . . . .: 172.16.201.2(優先)
   サブネット マスク . . . . . . . . . .: 255.255.255.255
   デフォルト ゲートウェイ . . . . . . .:
   DNS サーバー. . . . . . . . . . . . .: 10.2.1.3
   NetBIOS over TCP/IP . . . . . . . . .: 有効
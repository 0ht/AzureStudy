# Active Directoryを知る

## Active Directoryの機能

まとまった読み物として、
https://ids.itmedia.jp/dl/atmarkit_ebook7_activedirectory.pdf?bpc=c8f1aa5f69db888113e5001a8c838658baa14247857b99e81302018623a07af3

Azure AD DSとAD DSの比較（Azure ADもちょこっと）
https://asohiroblog.net/azure-ad-ds-vs-ad-ds-and-azure-ad/

wiki
https://ja.wikipedia.org/wiki/Active_Directory

意外とwikiが分かりやすい。

## Active Directoryとは？

- Windows Server ベースのシステムに認証の機能を提供する
- ドメイン：一回の認証でアクセスできる範囲
- ドメイン間で信頼関係を構成することが出来る（SSOの範囲を信頼関係を構成した複数のドメインまで広げることが出来る）
- AD Federation Serviceでクラウドと連携
- ユーザー/パスワードに頼らない、指紋・スマートカードによる認証もサポート
- ユーザーに関する様々な属性を設定可能
  - 属性ベースの権限付与が可能

主要な5つの機能

- ドメインサービス：ドメインの機能
- 証明書サービス：社内認証局としての機能
- フェデレーションサービス：組織間のSSO機能
- Rights managementサービス：特定のファイルに対する暗号化とアクセス制御機能を提供する
- LDAPサービス：ディレクトリデータベース機能

### ドメイン

ドメインは必要最小限数で運用する。Azure ADテナントと同じ考え方。複数運用とするのは以下のようなケース。

- 企業合併により既存の2つのドメインをそのまま利用して移行負荷を軽減
- 親会社・子会社などのIT基盤は共通だが、運用が異なるようなケース

ドメインの信頼関係には方向がある。

### フォレストとドメインツリー

いずれも双方向の信頼関係を示す。作成時のみ指定可能であるので慎重に検討。

フォレスト：ドメインの管理境界、フォレスト内のドメインは双方向の信頼関係
ドメインツリー：同じドメイン空間を共有する、ドメインの関連を示すもの

### ドメインコントローラー

- ドメインサービスを提供するコンポーネント。
- サーバーに Active Directoryドメインサービスの役割をインストールすると、そのサーバーは自動的にドメイ
ンコントローラーとなる
- 以下を提供する
    1. ディレクトリデータベースとしての役割（LDAPレジストリ）
    2. 認証と承認の役割（Kerberos）
    3. グループポリシーによるユーザー／コンピューターを制御する役割

#### ドメインコントローラーの複製

- ドメインコントローラー内製の「複製」機能を持っている
- 全てのコントローラーに書き込みが可能となっている

### サイト

- 高速通信が可能な範囲をまとめてサイトと呼ぶ
  - 東京サイトと大阪サイト のようなイメージ
- サイト間のドメインコントローラーで複製を行う場合は、複製がバッチ的に行われる
- サイト間の複製の設定はサイトリンクを構成し、ブリッジヘッドサーバーを経由して（優先指定が可能）

ここで、DR的にクラウド上にドメインコントローラーを配置しようという動きが出てくる。その場合の考慮点として以下のような点が挙げられているが、Azure AD DSを使用した場合にこれがどうなるのか別途確認が必要そう。

- サイトを分けることでトラフィックの現象を図る
- クラウドを読み取り専用にする

### DNSサーバー

ドメインコントローラーは、DNSのSRVレコードを参照する事で特定する。

通常、DNSは内部向けDNSと外部向けDNSを分けて運用することになるが、クライアントからは内部向けDNSサーバーを参照し、外部向けについては内部向けDNSサーバーからフォワーダを設定して外部向けサーバーにリクエストを転送する構成を取る。

ドメインコントローラー自体がネームサーバーとして動作するという構成を取ったりするのか？

- その様に![構成](https://docs.microsoft.com/ja-jp/windows-server/identity/ad-ds/plan/active-directory-integrated-dns-zones)できる模様。
  - その場合、ゾーン情報はドメインサービスに統合されて複製の対象になるため、ゾーン転送などをネームサーバー側で構成する必要がなくなる
  - 以下のディレクトリパーティションが作成される
    - ForestDnsZones と呼ばれるフォレスト全体のアプリケーションディレクトリパーティション
    - DomainDnsZones という名前のフォレスト内の各ドメイン用アプリケーションディレクトリパーティション
- ドメインに参加しているコンピュータは、コンピュータ名に参加しているドメインが連結されて登録される
https://docs.microsoft.com/ja-jp/windows-server/identity/ad-ds/plan/creating-a-dns-infrastructure-design

以下参考資料

- [DNS インフラストラクチャの設計を作成する](https://docs.microsoft.com/ja-jp/windows-server/identity/ad-ds/plan/creating-a-dns-infrastructure-design) 
- [DNS と AD DS](https://docs.microsoft.com/ja-jp/windows-server/identity/ad-ds/plan/dns-and-ad-ds)
- [Active Directory管理者のためのDNS入門](https://www.atmarkit.co.jp/ait/articles/0805/22/news139.html)
- [DNS Support for Active Directory Technical Reference](https://docs.microsoft.com/ja-jp/previous-versions/windows/it-pro/windows-server-2003/cc781627(v=ws.10))

## Azure ADとの連携

Azure には、マネージドサービスとしてのAD DS、即ちAzure AD DS、AADDSが提供されている。
Azure では、AADによるモダン認証を利用するのが定石だが、そういったモダン認証が利用できないレガシアプリケーションをAzureにLiftしてひとまず動かしたい、といった場合に有効なサービス。

### Azure AD DS

[Azure Active Directory Domain Services とは](https://docs.microsoft.com/ja-jp/azure/active-directory-domain-services/overview)

- AAD DS作成時にドメイン名前空間を定義。2つのDCが選択リージョンにデプロイされる。
- ADテナントから一方向の同期、オンプレなどからの同期はADテナントに対してAzure AD Connectを通して同期。
- 指定したVNetの専用サブネット内にデプロイされる
- VMが参照するDNSの設定として、VMが所属するドメインのネームサーバーからデプロイされたドメインコントローラーに条件付きフォワーダーを設定するか、所属するVNetに対してカスタムのネームサーバー（としてドメインコントローラー）を設定する

参考：

- [ハイブリッド組織向けの Azure AD DS](https://docs.microsoft.com/ja-jp/azure/active-directory-domain-services/scenarios#azure-ad-ds-for-hybrid-organizations)
- [チュートリアル:Azure Active Directory Domain Services のマネージド ドメイン用の仮想ネットワークを構成する](https://docs.microsoft.com/ja-jp/azure/active-directory-domain-services/tutorial-configure-networking#configure-dns-servers-in-the-peered-virtual-network)

## ADをハイブリッドで利用する

[ハイブリッド組織向けの Azure AD DS](https://docs.microsoft.com/ja-jp/azure/active-directory-domain-services/scenarios#azure-ad-ds-for-hybrid-organizations)

![picture 1](images/e847595ed682d52f0ee50693d01677805ae6ebd379900977b93dc886fc7e3962.png)  


## ここどうする？

### DNSの設定
### オンプレのDSにAzureのVMを追加

##
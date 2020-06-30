# Azure Security Cnterで、セキュリティチェック & インシデントの監視


- 諸々の推奨事項を提示してくれる。
- 詳細なデータの取得のためには、Monitoring Agentの導入が必要

ダッシュボードはこんな感じ
![picture 11](images/57dec5a61b05858e4bfbf1c025e71ac446ec6b39f843777eb08f0ed175983f4b.png)  

更に、各種セキュリティコンプライアンスの順守度と修正提案もしてくれる  
![picture 12](images/6113d0733de778a9638392555df0d8ccb2028af2231ebe58d807962eed3c9749.png)  

## その他便利機能

- **Just-In-Time (JIT) 仮想マシン アクセス**を使用して仮想マシンを保護
  - JIT は、仮想マシンへの永続的なアクセスをブロックする機能です。 仮想マシンには、構成した監査済みアクセスに基づいてのみアクセス
- **適応型アプリケーション制御**を使用して、仮想マシンで実行できるアプリケーションを制御
  - バックグラウンドで、機械学習を使用して、仮想マシンで実行されているプロセスを確認します。 仮想マシンを保持するリソース グループごとに例外規則が作成され、推奨事項が示されます。
- プレイブックを構成することで、アラート検出時に自動化された手順を実行可能。プレイブックはロジックアプリで構成する。

![picture 13](images/5203616fa13fb3891b635e5df33bdaa001d7c2ac17b688137eb3ac857e09a681.png)  

![picture 14](images/8abff62caaf7ae7fbcc118d0c584d3ad501276fffccf85f43e3ddc5773609063.png)  

![picture 15](images/060978f8190147c856e7539f849a42c47caaef71134b06ebbfffe14821fb0658.png)  

![picture 16](images/de4fd83d458cefa5e3d657699c3bb5353adea87e7d86a3ec07eab2c32e974d2c.png)  

![picture 17](images/4535110a5dac25cd2c7bcaf1fa84c2133c131e2b97e8c654e5a8200f567584cd.png)  


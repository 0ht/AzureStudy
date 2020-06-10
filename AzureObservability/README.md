## 診断データを収集および分析して Azure 仮想マシンの正常性を監視する
Azure VM の基本メトリック
	• CPU 使用率
	• ネットワーク トラフィック
	• OS ディスク使用量
	• 起動の成功

その他のメトリック
	• "Azure Diagnostics 拡張機能" と "Log Analytics エージェント" が必要
	• ストレージアカウント
	• 以下が取得できるようになる
		○ 強化されたブート診断を使用してブートの問題を調査する。
		○ 今後の分析のためにログやメトリックをアーカイブする。
		○ VM のパフォーマンスに応じて仮想マシンのスケール セットを自動スケーリングする。
		○ Application Insights を使用してアプリレベルのメトリックを取得する。
		○ OS の更新を自動化する。
		○ 一定期間における VM の構成変更を追跡する。
	

概要でデフォルトのメトリクスが確認可能
Diagを補完しようと思ったらストレージアカウントが必要

より詳細なメトリクス、リアルタイムのメトリクスが必要な場合は、Azure Diagnosticsを追加導入する。
拡張機能の有効には、VM作成時に監視セクションでOSのゲスト診断を有効にする
作成済みの場合は、診断設定から、ゲストレベルのモニタリングを有効にする。
拡張機能は Azure CLI または PowerShell を使用して追加することもできます。 
	Azure CLI では、az vm boot-diagnostics enable 
	PowerShell の場合は、Set-AzVMBootDiagnostic
	

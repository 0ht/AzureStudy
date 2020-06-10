# Azure Virtual Machines の概要

	• ネットワークから開始する
		○ 仮想ネットワーク (VNet) は、Azure において、Azure Virtual Machines と他の Azure サービスの間をプライベート接続で結ぶために使用
		○ 仮想ネットワーク外部のサービスから、仮想ネットワーク内部のサービスへの接続はできません
		○ ネットワークは、オンプレミス サーバーを含む外部サービスへのアクセスを許可するように構成できます
		○ VM を導入する前に必ずトポロジを考慮することをお勧め
		○ 仮想ネットワークに 1 つまたは複数のサブネットを作成できます
			§ 各サブネットの先頭から 4 個のアドレスと最後のアドレスは、Azure による使用のために予約されています
		○ 既定では、サブネット間にセキュリティ境界がないため、各サブネットのサービスは互いに通信できます。 ただし、ネットワーク セキュリティ グループ (NSG) を設定して、サブネット間と VM 間のトラフィック フローを制御
	
	• 各 VM デプロイの計画
		○ サーバーの通信相手
		○ 開いているポート
		○ 使用される OS
		○ 使用中のディスク領域の量
		○ 使用するデータの種類 オンプレミスに存在しないことによる (法的またはその他の) 制約の有無
		○ サーバーの CPU、メモリ、ディスク I/O の負荷の種類 考慮する必要があるバースト トラフィックの有無
		
	• VM に名前を付ける
		○ VM 名は、オペレーティング システムの一部として構成されるコンピューター名として使用されます。 Windows VM には最大 15 文字、Linux VM には最大 64 文字の名前を指定
		要素	例	メモ
		環境	dev、prod、QA	リソースの環境を識別
		場所	uw (米国西部)、ue (米国東部)	リソースのデプロイ先のリージョンを識別
		インスタンス	01、02	複数の名前付きインスタンスが存在するリソースの場合 (Web サーバーなど)
		製品またはサービス	service	リソースがサポートする製品、アプリケーション、サービスを識別
		ロール	sql、web、messaging	関連付けられているリソースのロールを識別
		
		貼り付け元  <https://docs.microsoft.com/ja-jp/learn/modules/intro-to-azure-virtual-machines/2-compile-a-checklist-for-creating-a-vm> 
		
		○ Azure リソースとは Azure で管理可能な項目です。 ご利用のデータセンターにある物理コンピューターと同じように、VM には、ジョブを実行するために必要な複数の要素があります。
			§ VM 自体
			§ ディスク用のストレージ アカウント
			§ 仮想ネットワーク (他の VM およびサービスとの共有)
			§ ネットワークで通信するためのネットワーク インターフェイス
			§ ネットワーク トラフィックをセキュリティで保護するためのネットワーク セキュリティ グループ
			§ パブリック インターネット アドレス (オプション)
		
	• VM の場所を決定する
		○ 場所の選択に関しては、それ以外に次の 2 点を考慮します。
			§  第 1 に、場所によって、使用可能なオプションが制限される場合があります。 各リージョンは、使用可能なさまざまなハードウェアを備えており、一部の構成はすべてのリージョンで使用できるわけではありません。
			§  第 2 に、場所によって価格差があります。 ワークロードが特定の場所にバインドされない場合は、必要な構成を複数のリージョンでチェックして、最低価格の場所を見つけると、非常にコスト効率が高くなる可能性があり
	• VM のサイズを決定する
		○ ワークロードの種類を考慮することです。 ワークロードに基づいて、利用可能な VM サイズのサブセットから選択
		○ 基本的なテストと運用向けの B シリーズから、大規模なコンピューティング タスク向けの H シリーズ
		Type	サイズ	説明
		汎用	B、Dsv3、Dv3、Dasv4、Dav4、DSv2、Dv2、Av2、DC、DCv2	バランスのとれた CPU 対メモリ比。 テストと開発、小～中規模のデータベース、および低～中程度のトラフィックの Web サーバーに最適です。
		コンピューティングの最適化	Fsv2	高い CPU 対メモリ比。 トラフィックが中程度の Web サーバー、ネットワーク アプライアンス、バッチ処理、アプリケーション サーバーに適しています。
		メモリの最適化	Esv3、Ev3、Easv4、Eav4、Mv2、M、DSv2、Dv2	高いメモリ対 CPU 比。 リレーショナル データベース サーバー、中～大規模のキャッシュ、およびメモリ内分析に適しています。
		ストレージの最適化	Lsv2	ビッグ データ、SQL、NoSQL データベース、データ ウェアハウス、および大規模なトランザクション データベースに最適な、高いディスク スループットと IO。
		GPU	NC、NCv2、NCv3、ND、NDv2 (プレビュー)、NV、NVv3、NVv4	負荷の高いグラフィックスのレンダリングやビデオ編集、ディープ ラーニングを使用したモデル トレーニングと推論 (ND) に特化した仮想マシン。 1 つまたは複数の GPU で利用できます。
		ハイ パフォーマンス コンピューティング	HB、HBv2、HC、H	高スループットのネットワーク インターフェイス (RDMA) のオプションを備えた、最も高速かつ強力な CPU 仮想マシン。
		
		貼り付け元  <https://docs.microsoft.com/ja-jp/azure/virtual-machines/linux/sizes?toc=/azure/virtual-machines/linux/toc.json> 
		○ 既存の VM サイズがニーズに合わない場合、サイズを変更
		
	• 価格モデルについて理解する
		○ コンピューティングとストレージという 2 種類の別々のコストが課金
		○ コンピューティング コスト
			§ コンピューティングの費用は、時間単位で課金されますが、請求は分単位で行われます。 たとえば、VM が 55 分間デプロイされていた場合、55 分間の使用に対してのみ課金
				□ Windowsはライセンスも料金に含まれるのでちょっと高い
				□ Azure ハイブリッド特典により、Windows の既存のライセンスを再利用してコストを削減できる場合があり
		○ ストレージ コスト
			§ VM で使用するストレージは別途課金
			§ VM が停止または割り当て解除されていて、実行中の VM に対する課金が発生しない場合でも、ディスクに使用しているストレージは課金
		オプション	説明
		従量課金制	従量課金制オプションでは、コンピューティング容量に対して秒単位で支払います。長期契約や事前の支払いは不要です。 いつでも起動または停止できるだけでなく、需要に応じてコンピューティング容量を増やしたり減らしたりできます。 このオプションは、実行するアプリケーションのワークロードが短期間である場合、または予想不可能で中断が許容されない場合に適しています。 これは、たとえば VM で簡単なテストを行う場合やアプリを開発する場合に適したオプションです。
		Reserved Virtual Machine Instances	Reserved Virtual Machine Instances (RI) オプションは、特定のリージョンにおける 1 年間から 3 年間分の仮想マシンの予約購入です。 前払いでの契約により、重量課金制の料金に比べて最大 72% の割引が受けられます。 RI は柔軟性があり、簡単に交換することや、早期解約料金を支払って返却することができます。 VM を継続して実行する必要がある場合、または予算を予測できるようにする必要があり、しかも、少なくとも 1 年間は VM を使用する契約ができる場合は、このオプションが適しています。
		貼り付け元  <https://docs.microsoft.com/ja-jp/learn/modules/intro-to-azure-virtual-machines/2-compile-a-checklist-for-creating-a-vm> 
		
	• VM 用のストレージ
		○ すべての Azure 仮想マシンには、少なくとも 2 個の仮想ハード ディスク (VHD) があり
			§ 1 つ目のディスクにはオペレーティング システムが格納され
			§ 2 つ目のディスクは一時ストレージとして使用
			§ 最大数は、選択した VM サイズによって異なります (通常は CPU ごとに 2 個)
		○ 各 VHD のデータは、Azure Storage にページ BLOB として保持
		○ ストレージ アカウントは、特定のサブスクリプション向けに、Azure Storage 内のオブジェクトへのアクセスを提供
		○ Standard または Premium ストレージ
			§ Azure Premium Storage は、ソリッド ステート ドライブ (SSD) を利用して、I/O を集中的に行うワークロードを実行する VM の高パフォーマンスと少ない待ち時間を実現します。
			§ Azure Premium Storage は運用環境のワークロードに使用します。特に、パフォーマンスの変動の影響を受けやすいワークロードや、I/O を集中的に行うワークロードに向いています。 
			§ 開発またはテスト用には、Standard ストレージで十分
		○  アンマネージド ディスクまたはマネージド ディスク
			オプション	説明
			アンマネージド ディスク	アンマネージド ディスクを使用する場合、VM ディスクに対応する VHD を保持するために使用するストレージ アカウントを自分で管理する必要があります。 使用する領域の容量に対するストレージ アカウントの料金を支払います。 1 つのストレージ アカウントには、I/O 操作数 20,000/秒という固定レート制限があります。これは、1 つのストレージ アカウントが最大使用率で 40 個の標準仮想ハード ディスクをサポートできることを意味します。 ディスクを増加してスケールアウトする必要がある場合は、ストレージ アカウントを増やす必要があり、複雑になる可能性があります。
			マネージド ディスク	マネージド ディスクは、より新しい、お勧めのディスク ストレージ モデルです。 ストレージ アカウントを管理する負担を Azure に移すことで、この複雑さを適切に解決します。 4 TB までのディスク サイズを指定します。Azure により、ディスクとストレージの "両方" が作成、管理されます。 ストレージ アカウント制限について気にする必要がないため、マネージド ディスクは簡単にスケールアウトできます。
			
			貼り付け元  <https://docs.microsoft.com/ja-jp/learn/modules/intro-to-azure-virtual-machines/2-compile-a-checklist-for-creating-a-vm> 
			
			
	• オペレーティング システムを選択する
		○ Azure Marketplace で、OS と特定のシナリオ用にインストールされた一般的なソフトウェア ツールを含む、より高度なインストール イメージを検索
		○ 、適切な OS イメージが見つからない場合は、必要なものを含む自分のディスク イメージを作成して Azure ストレージにアップロードし、それを使用して Azure VM を作成できます。 Azure では 64 ビット オペレーティング システムのみがサポート


Azure 仮想マシンを作成および管理するためのオプションについて

Azure Resource Manager
	•  Azure Resource Manager によって、これらの関連リソースをより効率的に操作できるようになります。 これでは、リソースが名前付きのリソース グループにまとめられるため、ユーザーはすべてのリソースを、まとめてデプロイ、更新、または削除できるようになり
	• Resource Manager では、特定の構成を作成およびデプロイするために使用できる "テンプレート" も作成でき
	

	• スクリプトではなく宣言型のテンプレートを使用してインフラストラクチャを管理します。
	• ソリューションのリソースを個別に処理するのではなく、すべてのリソースをグループとしてデプロイ、管理、監視します。
	• ソリューションを開発のライフサイクル全体で再デプロイします。リソースは、必ず一貫した状態でデプロイされます。
	• 正しい順序でデプロイされるように、リソース間の依存関係を定義します。
	• ロールベースのアクセス制御 (RBAC) が管理プラットフォームにネイティブ統合されるため、すべてのサービスにアクセス制御を適用できます。
	• タグをリソースに適用し、サブスクリプションのすべてのリソースを論理的に整理します。
	• 同じタグを共有するリソース グループのコストを表示することで、組織の課金をわかりやすくします。

Resource Manager テンプレートとは

が可能。

Azure PowerShell
	• PowerShell とは、シェル ウィンドウやコマンド解析などのサービスを提供する、クロスプラットフォームのシェルです。 Azure PowerShell は、(コマンドレットと呼ばれる) Azure 固有のコマンドを追加する、オプションのアドオン パッケージです

Azure CLI
	• クロスプラットフォーム コマンド ライン ツールです。 このツールは、macOS、Linux、および Windows で使用できます。
	• またはブラウザーで Cloud Shell を使用して使用できます。 Azure PowerShell のように、Azure CLI は、管理ワークフローを効率化する強力な方法です。 Azure CLI では、Azure PowerShell とは異なり、機能に PowerShell は不要
	• Azure CLI は、Ruby や Python などのその他のスクリプト言語で使用でき

プログラム (API)

Azure クライアント SDK
Azure クライアント SDK では、Azure REST API をカプセル化

Azure VM 拡張機能
Azure VM 拡張機能は、Azure VM の最初のデプロイ後にタスクを構成および自動化したい場合に使用できる、小規模なアプリケーション

Azure Automation サービス
Azure Automation では、頻繁に発生する、時間のかかる、エラーが発生しやすいタスクを簡単に自動化して、サービスを統合でき
プロセスの自動化
	監視タスクの設定
構成管理
	MS Endpoint Configuration Managerを使用してVMでの利用可能なソフトウェアの管理を行う
Update Management
	VMの更新、修正プログラムの管理
	
	
Azure VMの可用性の管理
	• サービスが中断されないようにするため、また、単一障害点を回避するために、VM ごとに 2 つのインスタンスをデプロイすることをお勧めします。 この機能は_可用性セット_と呼ばれています。
	• 可用性セットとは、全部が単一障害点の制限下に含まれないように、また、データ センターでホスト オペレーティング システムがアップグレードされるとき、全部が同時にアップグレードされないように、関連 VM のグループを確実にデプロイするための論理機能です。 可用性セットに配置された VM では、同じ機能セットを実行する必要があり、同じソフトウェアをインストールする必要があり
		○ Microsoft は、ある可用性セットにデプロイされたマルチインスタンス VM に対して、99.95% の外部接続性サービス レベル アグリーメント (SLA) を提供します。 そのため、適用する SLA に対して、ある可用性セット内に少なくとも 2 つの VM インスタンスをデプロイする必要があります。
	• 可用性セットは Azure portal のディザスター リカバリー セクションから作成
		○ 
	• 可用性セットに VM を配置するとき、Azure では、必ず障害ドメインと更新ドメインに分散されます。
	•  Azure Site Recovery では、プライマリ サイトからセカンダリの場所にワークロードが複製
		○ Azure Site Recovery の目標は仮想マシンまたは物理マシンの複製にあります。停電時、ワークロードの可用性を維持
			§ Site Recovery を利用すれば、Azure を復元先として利用できます。そのため、第二の物理データセンターを保守するためのコストと煩雑さから解放されます。
			§ Site Recovery では、運用環境に影響を与えることなく、復旧演習でフェールオーバーを極めてシンプルにテストできます。 そのため、計画フェールオーバーや計画外フェールオーバーを簡単にテストできます。 結局のところ、フェールオーバーを試したことがなければ、良いディザスター リカバリーを計画できません。
		○  計画にはカスタム PowerShell スクリプト、Azure Automation Runbook、手動介入手順を含めることができ

仮想マシンをバックアップする
	• Azure Backup は、オンプレミスとクラウドを問わず、物理コンピューターと仮想マシンを保護する_サービスとしてのバックアップ_ サービス
		○ Windows OS コンピューター上のファイルとフォルダー (物理または仮想、ローカルまたはクラウド)
		○ アプリケーション対応スナップショット (ボリューム シャドウ コピー サービス)
		○ Microsoft SQL Server、Microsoft SharePoint、Microsoft Exchange など、Microsoft の人気サーバー ワークロード
		○ Azure Virtual Machines のネイティブ サポート、Windows と Linux の両方
		○ Linux と Windows 10 のクライアント コンピューター
	• 
	
		○ ストレージ管理の自動化。 Azure Backup は、従量制課金モデルを使用して、バックアップ ストレージを自動的に割り当てて管理します。 料金は使用した分だけになります。
		○ 無制限のスケーリング。 Azure Backup では、Azure のパワーと拡張性が利用され、可用性が高くなります。
		○ 複数のストレージ オプション。 Azure Backup はローカル冗長ストレージを提供します。データのコピーはすべて同じリージョン内に存在し、geo 冗長ストレージでは、複製されたデータがセカンダリ リージョンに置かれます。
		○ 無制限のデータ転送。 転送する受信データまたは送信データ量に制限がありません。 転送データに対して料金は発生しません。
		○ データの暗号化。 データの暗号化により、Azure において、セキュリティで保護された転送と保存が実現します。
		○ アプリケーションの整合性のバックアップ。 アプリケーション整合性バックアップは、復旧ポイントがバックアップ コピーを復元するために必要なすべてのデータを持っていることを意味します。 Azure Backup では、アプリケーション間で一貫性のあるバックアップが行われます。
		○ 長期保存。 Azure では、バックアップ データを維持する時間に制限がありません。
	
	• Azure Backup では、ダウンロードし、バックアップするコンピューターにデプロイするコンポーネントがいくつか活用されます。 デプロイするコンポーネントは、保護する内容によって異なります。
		○ Azure Backup エージェント
		○ System Center Data Protection Manager
		○ Azure Backup Server
		○ Azure Backup VM 拡張機能
	• Azure Backup では、バックアップ データの格納に Recovery Services コンテナーが使用
	
仮想マシン作成

データディスクは、追加後にフォーマットする必要あり。
ディスクリソースを作成後、ディスクをVMに追加し、追加した後にフォーマットを行う必要がある。

初期状態
AzureUser@vm01-jp-east:~$ lsblk
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda       8:0    0   30G  0 disk
├─sda1    8:1    0 29.9G  0 part /
├─sda14   8:14   0    4M  0 part
└─sda15   8:15   0  106M  0 part /boot/efi
sdb       8:16   0    4G  0 disk
└─sdb1    8:17   0    4G  0 part /mnt

ディスク追加後
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda       8:0    0   30G  0 disk
├─sda1    8:1    0 29.9G  0 part /
├─sda14   8:14   0    4M  0 part
└─sda15   8:15   0  106M  0 part /boot/efi
sdb       8:16   0    4G  0 disk
└─sdb1    8:17   0    4G  0 part /mnt
sdc       8:32   0    4G  0 disk

初期化後
AzureUser@vm01-jp-east:~$ (echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc

Welcome to fdisk (util-linux 2.31.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0x3f81bfc7.

Command (m for help): Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): Partition number (1-4, default 1): First sector (2048-8388607, default 2048): Last sector, +sectors or +size{K,M,G,T,P} (2048-8388607, default 8388607):
Created a new partition 1 of type 'Linux' and of size 4 GiB.

Command (m for help): The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

AzureUser@vm01-jp-east:~$ lsblk
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda       8:0    0   30G  0 disk
├─sda1    8:1    0 29.9G  0 part /
├─sda14   8:14   0    4M  0 part
└─sda15   8:15   0  106M  0 part /boot/efi
sdb       8:16   0    4G  0 disk
└─sdb1    8:17   0    4G  0 part /mnt
sdc       8:32   0    4G  0 disk
└─sdc1    8:33   0    4G  0 part

フォーマット後
AzureUser@vm01-jp-east:~$ sudo mkfs -t ext4 /dev/sdc1
mke2fs 1.44.1 (24-Mar-2018)
Discarding device blocks: done
Creating filesystem with 1048320 4k blocks and 262144 inodes
Filesystem UUID: 47646645-7fae-4a3d-baa1-39324894ef46
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done
AzureUser@vm01-jp-east:~$ lsblk
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda       8:0    0   30G  0 disk
├─sda1    8:1    0 29.9G  0 part /
├─sda14   8:14   0    4M  0 part
└─sda15   8:15   0  106M  0 part /boot/efi
sdb       8:16   0    4G  0 disk
└─sdb1    8:17   0    4G  0 part /mnt
sdc       8:32   0    4G  0 disk
└─sdc1    8:33   0    4G  0 part

マウント
AzureUser@vm01-jp-east:~$ df
Filesystem     1K-blocks    Used Available Use% Mounted on
udev              179552       0    179552   0% /dev
tmpfs              39568     668     38900   2% /run
/dev/sda1       30309264 1707592  28585288   6% /
tmpfs             197832       0    197832   0% /dev/shm
tmpfs               5120       0      5120   0% /run/lock
tmpfs             197832       0    197832   0% /sys/fs/cgroup
/dev/sda15        106858    3668    103190   4% /boot/efi
/dev/sdb1        4061824   16380   3819400   1% /mnt
tmpfs              39564       0     39564   0% /run/user/1000
AzureUser@vm01-jp-east:~$ sudo mkdir /data && sudo mount /dev/sdc1 /data
AzureUser@vm01-jp-east:~$ df
Filesystem     1K-blocks    Used Available Use% Mounted on
udev              179552       0    179552   0% /dev
tmpfs              39568     668     38900   2% /run
/dev/sda1       30309264 1707600  28585280   6% /
tmpfs             197832       0    197832   0% /dev/shm
tmpfs               5120       0      5120   0% /run/lock
tmpfs             197832       0    197832   0% /sys/fs/cgroup
/dev/sda15        106858    3668    103190   4% /boot/efi
/dev/sdb1        4061824   16380   3819400   1% /mnt
tmpfs              39564       0     39564   0% /run/user/1000
/dev/sdc1        4061888   16376   3819464   1% /data

WindowsVMの作成
作成方法はLinuxとおおむね同じ。
RDPで接続
接続後は、サーバーマネージャーが起動する。ここでIISなどを構成することも可能
データディスクはやはり初期化する必要がある。


Azure CLIとは
vm操作のコマンド
サブコマンド	説明
create	新しい仮想マシンを作成します
deallocate	仮想マシンの割り当てを解除します
delete	仮想マシンを削除します
list	サブスクリプションに作成されている仮想マシンの一覧を表示します
open-port	受信トラフィック用に特定のネットワーク ポートを開きます
restart	仮想マシンを再起動します
show	仮想マシンについての詳細を取得します
start	停止している仮想マシンを起動します
stop	実行している仮想マシンを停止します
update	仮想マシンのプロパティを更新します

貼り付け元  <https://docs.microsoft.com/ja-jp/learn/modules/manage-virtual-machines-with-azure-cli/2-create-a-vm> 


必須パラメータは、以下の通り
パラメーター	説明
--resource-group	仮想マシンを所有するリソース グループです。[サンドボックス リソース グループ] を使用します。
--name	仮想マシンの名前であり、リソース グループ内で一意である必要があります。
--image	VM の作成に使用するオペレーティング システム イメージです。
--location	VM を配置するリージョンです。 通常は、VM のコンシューマーの近くになります。 この演習では、次の一覧から近くの場所を選択します。

貼り付け元  <https://docs.microsoft.com/ja-jp/learn/modules/manage-virtual-machines-with-azure-cli/2-create-a-vm> 



VMの作成
az vm create \
  --resource-group test-nw-rg \
  --location japaneast \
  --name SampleVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys \
  --verbose

tomoyuki@Azure:~$ az vm create \
>   --resource-group test-nw-rg \
>   --location japaneast \
>   --name SampleVM \
>   --image UbuntuLTS \
>   --admin-username azureuser \
>   --generate-ssh-keys \
>   --verbose
Use existing SSH public key file: /home/tomoyuki/.ssh/id_rsa.pub
{- Finished ..
  "fqdns": "",
  "id": "/subscriptions/cacb158e-adcf-4815-b7d4-1dd87212e9a0/resourceGroups/test-nw-rg/providers/Microsoft.Compute/virtualMachines/SampleVM",
  "location": "japaneast",
  "macAddress": "00-0D-3A-CE-12-0F",
  "powerState": "VM running",
  "privateIpAddress": "10.1.1.6",
  "publicIpAddress": "20.188.0.73",
  "resourceGroup": "test-nw-rg",
  "zones": ""
}
command ran in 34.513 seconds.

ここでは、generate-ssh-keys フラグも使用しています。 このパラメーターは Linux ディストリビューションに対して使用され、ssh ツールを使用して仮想マシンにリモート アクセスできるように、セキュリティ キーのペアが作成されます。 コンピューター上と VM 内の .ssh フォルダーに、2 つのファイルが配置されます。 ターゲット フォルダーに id_rsa という名前の SSH キーが既にある場合はそれが使用され、新しいキーは生成されません。

ssh azureuser@20.188.0.73


az vm list-vm-resize-options \
    --resource-group  test-nw-rg \
    --name SampleVM \
    --output table

az vm resize \
    --resource-grouptest-nw-rg \
    --name SampleVM \
    --size Standard_D2s_v3

便利コマンド
az vm list-ip-address
--output で出力フォーマットを選択可能

稼働状況を確認する
az vm get-instance-view \
    --name SampleVM \
    --resource-group test-nw-rg \
    --query "instanceView.statuses[?starts_with(code, 'PowerState/')].displayStatus" -o tsv

ポートを開く
az vm open-port \
    --port 80 \
    --resource-group test-nw-rg \
    --name SampleVM


仮想マシンを最新の状態に保つ
Update Management ソリューションには、次のいくつかの利点があります。
	• 仮想マシン内にエージェントや追加の構成がありません。
	• VM にログオンせずに更新プログラムを実行できます。 更新プログラムをインストールするためのパスワードを作成する必要もありません。
	• Update Management ソリューションは、不足している更新プログラムを示し、失敗したデプロイに関する情報を読みやすい形式で提供します。
	• 対象OS
		○ Windows Server (2008 以降)
		○ CentOS 6 (x86/x64) と CentOS 7
		○ Red Hat Enterprise 6 (x86/x64) および 7 (x64)
		○ SUSE Linux Enterprise Server 11 (x86/x64) および 12 (x64)
		○ Ubuntu 14.04 LTS、16.04 LTS、18.04 (x86/x64)
		
	• Update Management で使用されるコンポーネント
		○ Windows または Linux 用の Microsoft Monitoring Agent (MMA)
		○ PowerShell Desired State Configuration (DSC) (Linux の場合)
		○ Automation Hybrid Runbook Worker
		○ Microsoft Update または Windows Server Update Services (WSUS) (Windows コンピューターの場合)
		○ 
	• 
	• ハイブリッドworkerグループ
		○ ソリューションに含まれている Runbook をサポートするために、Log Analytics ワークスペースに直接接続された Windows コンピューターが自動的に Hybrid Runbook Worker として構成され
	
	
	• コンプライアンススキャン
		○ Update Management は、更新プログラムのコンプライアンスを確認するためにスキャンを実行します。 コンプライアンス スキャンは、既定で、
			§ Windows コンピューターでは 3 時間ごと、
			§ Linux コンピューターでは 12 時間ごとに実行されます。 
		○ このスキャン スケジュールに加えて、
			§ MMA の再起動後 15 分以内、
			§ 更新プログラムのインストール前、
			§ および更新プログラムのインストール後に、
			§ コンプライアンス スキャンが開始されます。
		○  コンピューターが更新プログラムのコンプライアンスを確認するためにスキャンを実行した後、エージェントによって情報が Azure Log Analytics に一括転送

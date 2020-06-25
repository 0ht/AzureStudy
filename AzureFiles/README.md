# Azure Files

## 概要

Azure上でホストされるSMBプロトコルでアクセスされるファイル共有のサービス。
Azure Files利用には、以下のような利点がある。

- ログファイルの保管（REST APIで配置して、何かしらのツールで解析）
- 管理をAzureに任せられる
- PowerShellもしくはCLIで管理を自動化
- グローバルに冗長化可能、バックアップも取得可能
- 通信はHTTPSで暗号化、保存時も暗号化
- クロスプラットフォーム
  
アクセス方法は2つ

- OSマウントされたドライブに直接アクセス
- Windowsサーバーを使用してAzure File Syncを導入してファイル同期

ファイルはストレージアカウントで管理されるため、ストレージアカウントのタイプでパフォーマンスのオプションが選択される

- Standard パフォーマンス: 2 桁ミリ秒の待機時間、10,000 IOPS、300 MBps の帯域幅
- Premium パフォーマンス: 1 桁ミリ秒の待機時間、100,000 IOPS、5 GBps の帯域幅


## データ移行には何が使用可能か？

ユーティリティ	説明  
- AzCopy	最高のパフォーマンスを提供するコマンドライン ツール。少数の小さいファイルに特に適しています。
- Robocopy	Windows と Windows Server に付属しているコマンドライン ツール。 AzCopy は Azure 対応に書かれており、パフォーマンスが向上します。
- Azure Storage Explorer	Windows、Linux、macOS で実行できるグラフィカル ファイル管理ユーティリティ。
- Azure portal	ポータルを使用して、ファイルとフォルダーをインポートします。
- Azure File Sync	初期のデータ転送に使用でき、その後、データ転送が済んだらアンインストールできます。
- Azure Data Box	最大 35 TB のデータがあり、1 週間以内にインポートする必要がある場合。

## 構成

大まかなステップとして以下の2ステップを実行。

1．ストレージアカウントの作成  
2．ファイル共有の作成

### 1．ストレージアカウントの作成

```PowerShell
az storage account create \
--name <unique account name> \
--resource-group <resource group name> \
--sku Standard_GRS
```

skuには、以下を指定可能

- Premium_LRS
- Premium_ZRS
- Standard_GRS
- Standard_GZRS
- Standard_LRS
- Standard_RAGRS
- Standard_RAGZRS
- Standard_ZRS

### 2.ファイル共有の設定

```azcli
az storage share create \
--account-name <unique account name> \
--account-key <account key> \
--name <share name>
```

--account-name と、--account-key または --sas-token のどちらかの引数を使用  
--connection-string でストレージ アカウントの接続文字列を使用  

### 認証について

ファイル共有へアクセスするコンピュータがAzureでホストされているドメイン参加済みのWindowsVMの場合は、Azure AD認証が必要.  
現時点では、SMB経由のKerberos認証はサポートされていない。

## ファイル共有のセキュリティ

### IPベースのファイアウォール規則

公開するIPレンジを設定してアクセスの範囲を絞る

```PowerShell
Add-AzStorageAccountNetworkRule `
    -ResourceGroupName "myresourcegroup" `
    -AccountName "mystorageaccount" `
    -IPAddressOrRange "NNN.NNN.NNN.NNN"
```

```azurecli
az storage account network-rule add \
    --resource-group "myresourcegroup" \
    --account-name "mystorageaccount" \
    --ip-address "NNN.NNN.NNN.NNN"
```

### HTTTPで通信を暗号化する

```Powershell
Set-AzStorageAccount `
    -Name "StorageAccountName" `
    -ResourceGroupName "ResourceGroupName" `
    -EnableHttpsTrafficOnly $True
```

```cli
az storage account update \
    --resource-group ResourceGroupName \
    --name StorageAccountName \
    --https-only true
```

### Azure AD DS認証を有効にする

Azure AD DS の利点は、ロールベースのアクセス制御を使用してアクセスを管理できることです。 Files では、既存の NTFS DACL が継承されます。
DS認証もNTFS DACLもなんもわからん

ステップ

1. Azure ADテナントの作成
2. Azure AD Domain Serviceを有効にする
3. VMをドメイン参加
4. Azureファイル共有の選択・作成

ここまでは前提

5. Azure FilesでのAzure AD DS認証を有効にする
6. 共有レベルのアクセス許可を割り当て
7. ディレクトリの構成、ファイルレベルのNTFSアクセスの許可
8. AzureAD資格情報を使用して共有をマウント

### 共有スナップショットを使用して誤削除を防ぐ

スナップショットは、ファイル共有のルート レベルにあり、それに含まれるすべてのフォルダーとファイルに適用される



## ポイント

- File Syncでは、General type useのみサポート、failoverはサポートされていない
- File Syncでは、windowsクラスターfailoverのdeduplication機能は使用できない
  
  
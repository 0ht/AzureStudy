# 仮想マシンを最新の状態に保つ

## Update Management ソリューション

- 仮想マシン内にエージェントや追加の構成がありません。
- VM にログオンせずに更新プログラムを実行できます。 更新プログラムをインストールするためのパスワードを作成する必要もありません。
- Update Management ソリューションは、不足している更新プログラムを示し、失敗したデプロイに関する情報を読みやすい形式で提供します。
  
- 対象OS
  - Windows Server (2008 以降)
  - CentOS 6 (x86/x64) と CentOS 7
  - Red Hat Enterprise 6 (x86/x64) および 7 (x64)
  - SUSE Linux Enterprise Server 11 (x86/x64) および 12 (x64)
  - Ubuntu 14.04 LTS、16.04 LTS、18.04 (x86/x64)
  
- Update Management で使用されるコンポーネント
  - Windows または Linux 用の Microsoft Monitoring Agent (MMA)
  - PowerShell Desired State Configuration (DSC) (Linux の場合)
  - Automation Hybrid Runbook Worker
  - Microsoft Update または Windows Server Update Services (WSUS) (Windows コンピューターの場合)

- ハイブリッドworkerグループ
  - ソリューションに含まれている Runbook をサポートするために、Log Analytics ワークスペースに直接接続された Windows コンピューターが自動的に Hybrid Runbook Worker として構成される

- コンプライアンススキャン
  - Update Management は、更新プログラムのコンプライアンスを確認するためにスキャンを実行します。 コンプライアンス スキャンは、既定で、
    - Windows コンピューターでは 3 時間ごと、
    - Linux コンピューターでは 12 時間ごとに実行されます。
  - このスキャン スケジュールに加えて、
    - MMA の再起動後 15 分以内
    - 更新プログラムのインストール前
    - 更新プログラムのインストール後 にコンプライアンス スキャンが開始される
  - コンピューターが更新プログラムのコンプライアンスを確認するためにスキャンを実行した後、エージェントによって情報が Azure Log Analytics に一括転送

## VMの起動診断

有効化のコマンド

```sh
Set-AzVMDiagnosticsExtension
```

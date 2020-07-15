# Azure AD Join

https://docs.microsoft.com/ja-jp/azure/active-directory/devices/azureadjoin-plan

## 概要

- デバイスをAzure ADに直接参加させる機能
- マネージド環境とフェデレーション環境の両方で動作
  - マネージド環境
    - パスワードハッシュ同期もしくはパススルー認証のいずれかとシームレスシングルサインオンを使用してデプロイ
  - フェデレーション環境
    - 以下両方のプロトコルをサポートしているIDプロバイダーが必要
    - Ws-Fed: このプロトコルは、デバイスを Azure AD に参加させるために必要です。
    - WS-Trust: このプロトコルは、Azure AD 参加済みデバイスにサインインするために必要です。
- スマートカードと証明書ベースの認証を使用してAzure ADに参加させることはできない。
  - AD FS構成してある場合は、スマートカードを使用してAzure AD参加済みデバイスにサインインすることが可能
- ユーザーをオンプレミスのADに作成する場合は、Azure AD Connectを使用してAzure ADと同期する必要がある

## デバイス管理

- サポートされているデバイスは、Windows10 のみ
- 管理プラットフォームとして、Intune、MDM CSPなどが必要
- 
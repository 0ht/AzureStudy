# Azure AD Identity Governance

## 概要

- Identity Governance は、オンプレミスであれクラウドであれ、従業員とビジネス パートナーとベンダー、そして、サービスとアプリケーションの垣根を越えて組織が次のタスクを実行できるようにします。
  - ID ライフサイクルの管理
  - アクセスのライフサイクルの管理
  - 管理に使用される特権アクセスのセキュリティ保護
- 具体的には、組織が次の 4 つの重要な問題に対処できるようにすることを目的とする
  - どのユーザーがどのリソースへのアクセス権を持つ必要があるか。
  - それらのユーザーはそのアクセス権によって何を実行するか。
  - アクセスを管理するための効果的な組織的管理は存在しているか。
  - 監査者は管理が機能していることを確認できるか。

## エンタイトルメント管理

## アクセスレビュー

ユーザーのアクセス権 | レビュー担当者になれるユーザー | レビューが作成される場所 | レビュー担当者のエクスペリエンス
-----------|-----------------|--------------|-----------------
セキュリティ グループ メンバー/Office グループ メンバー | 指定されたレビュー担当者/グループ所有者/自己レビュー | Azure AD アクセス レビュー/Azure AD グループ | アクセス パネル
接続されたアプリに割り当て | 指定されたレビュー担当者/自己レビュー | Azure AD アクセス レビュー/azure AD エンタープライズ アプリ (プレビュー) | アクセス パネル
Azure AD ロール | 指定されたレビュー担当者/自己レビュー | Azure AD PIM(Privileged Identity Management) | Azure portal
Azure リソース ロール | 指定されたレビュー担当者/自己レビュー | Azure AD PIM(Privileged Identity Management) | Azure portal

## Priviledged Identity Management

https://docs.microsoft.com/ja-jp/azure/active-directory/privileged-identity-management/pim-how-to-start-security-review?toc=/azure/active-directory/governance/toc.json

 Azure AD Privileged Identity Management (PIM) を使用して、特権 Azure AD ロールのアクセス レビューを作成できます。 自動的に実行される定期的なアクセス レビューを構成することもできます。
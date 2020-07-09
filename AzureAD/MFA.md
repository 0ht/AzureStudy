# 多要素認証によるセキュリティで Azure Active Directory ユーザーを保護

## 概要

- Azure Multi-Factor Authentication (MFA) では、完全な認証のために 2 つ以上の要素を必須とすることで、ID のセキュリティを強化
- 3つの要素
  - **ユーザーが知っているもの**
    - パスワード、あるいはセキュリティの質問に対する答えなどがこれに相当します。
  - **ユーザーが持っているもの**
    - 通知を受け取るモバイル アプリまたはトークンを生成するデバイスがこれに相当します。
  - **ユーザー自身**
    - 多くのモバイル デバイスで使用されている指紋や顔のスキャンなど、生体認証がこれに相当します。

### MFAを利用するには

- Azure Active Directory Premium または Microsoft 365 Business - いずれのサービスも Azure Multi-Factor Authentication に対応しており、条件付きアクセス ポリシーを利用して多要素認証を要求できます。
- Azure AD Free、またはスタンドアロンの Office 365 ライセンス - 事前に作成した条件付きアクセス ベースライン保護ポリシーを使用し、ユーザーと管理者に多要素認証を要求します。
- Azure Active Directory 全体管理者 - Azure Multi-Factor Authentication の一部の機能を集めたものであり、全体管理者アカウントを保護するための手段として利用されます。

- 多要素認証を有効にするオプション
  - 徳敵の条件下で発動する条件付きアクセスポリシー（Conditional Access Policy）こっちが推奨
  - 個々のユーザーにAzure MFAを有効にする（Grand Access control）

### MFAにおけるユーザーの状態

Status | 説明 | 非ブラウザー アプリに影響があるか | ブラウザー アプリに影響があるか | 影響を受ける先進認証
-------|----|-------------------|------------------|-----------
無効Disabled | Azure Multi-Factor Authentication に登録されていない、新しいユーザーの既定の状態です。 | いいえ | いいえ | いいえ
有効Enabled | ユーザーは多要素認証に登録されていますが、まだ登録されていません。 次回のサインイン時に登録することを求められます。 | いいえ。 これらは登録プロセスが完了するまで機能し続けます。 | はい。 セッションの有効期限が切れると、Azure Multi-Factor Authentication の登録が必要になります。 | はい。 アクセス トークンの有効期限が切れると、Azure Multi-Factor Authentication の登録が必要になります。
強制Enforced | ユーザーは登録されており、Azure Multi-Factor Authentication の登録プロセスが完了しています。 | はい。 アプリはアプリ パスワードを必要とします。 | はい。 ログイン時に Azure Multi-Factor Authentication が必要です。 | はい。 ログイン時に Azure Multi-Factor Authentication が必要です。


## 多要素認証の仕組み

![picture 57](images/79ae0d32eb8c48558039a47b9758d46e9df49810470d28b7cc6f0d3cbcb6980f.png)  

1. ローカルの Azure MFA サービスにより、オンプレミスの Active Directory に認証要求を渡すことで、最初のサインイン要求が検証されます。
2. 正しい資格情報が入力され、その正当性が確認された場合、サービスから Azure Multi-Factor Authentication Server に要求が送信されます。
3. Azure Multi-Factor Authentication Server からユーザーに追加の検証課題が送信されます。 次のような検証方法を簡単に構成できます。
   1. 電話。 ユーザーが登録した電話機に Azure Multi-Factor Authentication Server から電話連絡が行われます。
   2. テキスト メッセージ。 ユーザーの携帯電話に Azure Multi-Factor Authentication Server から 6 桁のコードが届きます。
   3. モバイル アプリの通知。 Azure Multi-Factor Authentication Server からユーザーのスマートフォンに検証要求が届き、モバイル アプリで [検証] を選択し、検証を完了するように求められます。
   4. モバイル アプリの検証コード。 Azure Multi-Factor Authentication Server からユーザーのモバイル アプリに 6 桁のコードが届きます。 ユーザーはサインイン ページでこのコードを入力します。
   5. Open Authentication (OATH) 準拠トークンのイニシアチブ。 以下を検証方法として利用できます。

サービスを Azure で実行している場合:

4. サービスから、初回検証のために Azure AD にサインイン要求がまず送信され、次に、Azure Multi-Factor Authentication Server に送信されます。
5. 前述のように、Azure Multi-Factor Authentication Server からユーザーに追加の検証課題が送信されます。

## MFAを利用するには、

多要素認証を使用して、デバイスを認証する

- MFA Portalにアクセス
- 最低一つのverification methodを選択
- 信頼済みデバイスにMFAを記録させる
- アクセスポリシーを設定
  
![picture 58](images/8145b1356e4010ed3f54f031ece97c9f7750a357edb0de463ba614f9dda59a1c.png)  

特定のアプリでMFAを有効にするには



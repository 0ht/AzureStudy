# Mobile Appsについて

https://docs.microsoft.com/ja-jp/previous-versions/azure/app-service-mobile/app-service-mobile-value-prop

## 概要

- Mobile Apps 機能は、エンタープライズ開発者とシステム インテグレーター向けの、スケーラブルでグローバルに利用できるモバイル アプリケーション開発プラットフォーム
- ネイティブ アプリとクロスプラットフォーム アプリの構築
  - iOS、Android、Windows のネイティブ アプリを構築する場合でも、Xamarin や Cordova (PhoneGap) のクロスプラットフォーム アプリを構築する場合でも、ネイティブ SDK を使用して App Service を活用することができます。
- エンタープライズ システムへの接続
  - Mobile Apps の機能を使用して、企業へのサインオンを数分で追加し、企業のオンプレミス リソースまたはクラウド リソースに接続することができます。
- データ同期を使用したオフライン対応アプリの構築
  - オフラインでも動作し、企業のデータ ソースや SaaS (サービスとしてのソフトウェア) API と接続したときに Mobile Apps を使用してバックグラウンドでデータを同期するアプリを構築することにより、モバイル ユーザーの生産性が向上します。
- 瞬時に多数のユーザーへプッシュ通知
  - 顧客のニーズに合わせてカスタマイズされ、適切なタイミングで送信される、任意のデバイスへの即時のプッシュ通知によって顧客の関心を引きます

## 機能

- 認証と承認
  - Azure Active Directory など、エンタープライズ認証用の ID プロバイダーに加え、Facebook、Google、Twitter、Microsoft アカウントなど、ソーシャル プロバイダーがサポートされます。 Mobile Apps は、各プロバイダーに対して OAuth 2.0 サービスを提供します。 また、プロバイダー固有の機能を実現するために、ID プロバイダーの SDK を統合することもできます。
- データ アクセス
  - Mobile Apps では、Azure SQL Database またはオンプレミス SQL Server にリンクされた、モバイル対応の OData v3 データ ソースを提供します。 このサービスは Entity Framework に基づいているため、他の NoSQL および SQL データ プロバイダー (Azure Table Storage、MongoDB、Azure Cosmos DB など) や SaaS API プロバイダー (Office 365、Salesforce.com など) と簡単に統合できます。
- オフライン同期
  - クライアント SDK を使用することで、オフライン データ セットで動作する堅牢で応答性の高いモバイル アプリケーションを簡単に構築できます。 このデータ セットは、競合解決のサポートも含め、バックエンド データと自動的に同期できます。
- プッシュ通知
  - クライアント SDK は Azure Notification Hubs の登録機能とシームレスに統合されます。これにより、数百万人のユーザーにプッシュ通知を同時に送信できます。
- クライアント SDK
  - ネイティブ開発 (iOS、Android、Windows)、クロスプラットフォーム開発 (Xamarin.iOS と Xamarin.Android、Xamarin.Forms)、ハイブリッド アプリケーション開発 (Apache Cordova) に対応したクライアント SDK の完全なセットが用意されています。 各クライアント SDK は、オープンソースであり、MIT ライセンスで使用できます。


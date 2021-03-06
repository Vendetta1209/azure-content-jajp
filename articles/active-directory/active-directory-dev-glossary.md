<properties
   pageTitle="Azure Active Directory 開発者向け用語集 | Microsoft Azure"
   description="Azure Active Directory 開発で頻出する概念や機能に関する用語の定義を記載しています。"
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/31/2016"
   ms.author="bryanla"/>

# Azure Active Directory 開発者向け用語集
この記事では、Azure Active Directory (AD) 開発で重要となるいくつかの概念について定義しています。Azure AD のアプリケーション開発を習得する際の参考としてください。

## Twitter アプリケーションの 
[承認サーバー](#authorization-server)によって発行される[セキュリティ トークン](#security-token)の一種。[クライアント アプリケーション](#client-application)が、[保護されたリソース サーバー](#resource-server)にアクセスする目的で使用します。要求されたレベルのアクセスに関して、[リソース所有者](#resource-owner)がクライアントに付与しているアクセス権限を通常 [JSON Web トークン (JWT)][JWT] の形式で 1 つにまとめたものがこのトークンです。このトークンは、認証対象に関して当てはまる[要求](#claim)をすべて含んでおり、クライアント アプリケーションが特定のリソースにアクセスする際に一種の資格情報として使用することができます。そのため、リソース所有者がその資格情報をクライアントに開示する必要がありません。

表現の対象となる資格情報によっては、アクセス トークンを "User+App" や "App-Only" と呼ぶこともあります。たとえばクライアント アプリケーションが使用する承認付与には、次のようなタイプがあります。

- ["承認コード" 型の承認付与](#authorization-grant): エンド ユーザーはまず、リソース所有者として認証を行い、リソースにアクセスするための承認をクライアントに委任します。その後クライアントは、アクセス トークンを取得した時点で認証を行います。このトークンは、クライアント アプリケーションを承認したユーザーとアプリケーションの両方を表すことから、より具体的に "User+App" トークンと呼ばれることがあります。
- ["クライアント資格情報" 型の承認付与](#authorization-grant): クライアントが行うのは単一の認証のみです。クライアントがリソース所有者の認証/承認なしで機能することから、このトークンは、"App-Only" トークンと呼ばれることがあります。

詳細については、「[Azure AD のトークン リファレンス][AAD-Tokens-Claims]」を参照してください。

## アプリケーション マニフェスト  
[Azure クラシック ポータル][AZURE-classic-portal]に備わっている機能の 1 つで、アプリケーションの ID 構成が JSON 形式で生成されて表現されます。そのマニフェストが関連付けられている [Application][AAD-Graph-App-Entity] エンティティと [ServicePrincipal][AAD-Graph-Sp-Entity] エンティティを更新するための機構として使用されます。詳細については、「[Azure Active Directory のアプリケーション マニフェストについて][AAD-App-Manifest]」を参照してください。

## アプリケーション オブジェクト  
[Azure クラシック ポータル][AZURE-classic-portal]でアプリケーションを登録/更新すると、そのテナントを対象に、アプリケーション オブジェクトおよび対応する[サービス プリンシパル オブジェクト](#service-principal-object)の両方が作成/更新されます。アプリケーション オブジェクトは、アプリケーションの ID 構成をグローバルに (そのアプリケーションがアクセスできるすべてのテナントに対して) "*定義*" します。このオブジェクトをひながたとして、対応するサービス プリンシパル オブジェクトが "*生成*" され、実行時にローカル (特定のテナント) で使用されます。

詳細については、「[アプリケーション オブジェクトおよびサービス プリンシパル オブジェクト][AAD-App-SP-Objects]」を参照してください。

## アプリケーションの登録  
アプリケーションの "ID とアクセス管理" の機能を Azure AD で行うためには、そのアプリケーションを Azure AD [テナント](#tenant)に登録する必要があります。アプリケーションを Azure AD に登録するとき、アプリケーションに使用する ID 構成を指定します。これによって Azure AD との連携が可能となり、次のような機能が使用できるようになります。

- Azure AD Identity Management と [OpenID Connect][OpenIDConnect] プロトコル実装によるシングル サインオンの強固な管理
- [クライアント アプリケーション](#client-application)による[保護されたリソース](#resource-server)へのブローカー アクセス (Azure AD の OAuth 2.0 [承認サーバー](#authorization-server)実装を介したアクセス)
- リソース所有者の承認に基づいて保護されたリソースへのクライアント アクセスを管理する[同意フレームワーク](#consent)

詳細については、「[Azure Active Directory とアプリケーションの統合][AAD-Integrating-Apps]」を参照してください。

## authentication
特定の当事者に対し、本物の資格情報の提示を要求する行為。ID 管理とアクセス制御に必要なセキュリティ プリンシパルの拠り所となります。たとえば [OAuth2 承認付与](#authorization-grant)時には、使用する付与形態に応じて、[リソース所有者](#resource-owner)または[クライアント アプリケーション](#client-application)の役割を果たす当事者が、本物であることを証明する側になります。

## authorization
認証済みのセキュリティ プリンシパルに対し、何かを実行する権限を付与する行為。Azure AD プログラミング モデルでは、主に次の 2 つの使用ケースが存在します。

- [OAuth2 承認付与](#authorization-grant)フロー時: [リソース所有者](#resource-owner)が[クライアント アプリケーション](#client-application)に承認を付与すると、その所有者のリソースにクライアントがアクセスできます。
- クライアントによるリソース アクセス時: [リソース サーバー](#resource-server)側で実装されます。[アクセス トークン](#access-token)内に存在する [claim](#claim) 値に基づいてアクセス制御の判断を行います。

## 承認コード
"承認コード" 型 (4 種類ある OAuth2 [承認付与](#authorization-grant) の 1 つ) のフローの過程で[承認エンドポイント](#authorization-endpoint)から[クライアント アプリケーション](#client-application)に提供される有効期間の短い "トークン"。[リソース所有者](#resource-owner)の認証に対する応答としてこのコードがクライアント アプリケーションに返されることで、所有者に代わってリソースにアクセスするための承認が既にリソース所有者からクライアント アプリケーションに委任されていることが示されます。このフローの中で、このコードは後で[アクセス トークン](#access-token)と引き換えられます。

## 承認エンドポイント
[承認サーバー](#authorization-server)によって実装されるエンドポイントの 1 つ。OAuth2 承認付与フローの過程で[承認付与](#authorization-grant)を提供するための、[リソース所有者](#resource-owner)との対話に使用されます。実際に付与される内容は、使用されている承認付与フローによって異なる場合があります ([承認コード](#authorization-code)、[セキュリティ トークン](#security-token)など)。

詳細については、OAuth2 仕様の[承認付与タイプ][OAuth2-AuthZ-Grant-Types]と[承認エンドポイント][OAuth2-AuthZ-Endpoint]に関するセクションおよび [OpenIDConnect 仕様][OpenIDConnect-AuthZ-Endpoint]を参照してください。

## 承認付与
[リソース所有者](#resource-owner)の保護されたリソースにアクセスしてよいという[承認](#authorization)を表す資格情報。[クライアント アプリケーション](#client-application)に対して付与されます。クライアント アプリケーションは、その種類や要件に応じて、[OAuth2 Authorization Framework によって規定された 4 つの付与タイプ][OAuth2-AuthZ-Grant-Types] \("承認コード付与"、"クライアント資格情報付与"、"暗黙的付与"、"リソース所有者パスワード資格情報付与") のいずれかを使ってアクセス許可を得ることができます。クライアントに返される資格情報は、使用された承認付与フローに応じて、[アクセス トークン](#access-token)か[承認コード](#authorization-code) (その後アクセス トークンに交換される) のどちらかになります。

## 承認サーバー
[OAuth2 Authorization Framework][OAuth2-Role-Def] の定義によれば、[リソース所有者](#resource-owner)を認証し、その承認を得た後にアクセス トークンを[クライアント](#client-application)に発行するサーバーをいいます。[クライアント アプリケーション](#client-application)は実行時に、その[承認](#authorization-endpoint)エンドポイントおよび[トークン](#token-endpoint) エンドポイントを介し、OAuth2 によって定義された[承認付与](#authorization-grant)に従って承認サーバーと対話します。

Azure AD アプリケーション統合の場合、Azure AD アプリケーションと Microsoft サービス API ([Microsoft Graph API][Microsoft-Graph] など) に使用する承認サーバーのロールを Azure AD が実装します。

## 要求
[セキュリティ トークン](#security-token)には要求が格納されます。この要求を通じて、一方のエンティティ ([クライアント アプリケーション](#client-application)、[リソース所有者](#resource-owner)など) に関するアサーションが、もう一方のエンティティ ([リソース サーバー](#resource-server)など) に渡されます。要求は、トークンのサブジェクト ([承認サーバー](#authorization-server)によって認証されたセキュリティ プリンシパルなど) に関する事実を伝達する名前と値のペアです。特定のトークンとして提示される要求は、いくつかの不確定要素 (トークンの種類、サブジェクトの認証に使用された資格情報の種類、アプリケーション構成など) に依存します。

詳細については、「[Azure AD のトークン リファレンス][AAD-Tokens-Claims]」を参照してください。

## クライアント アプリケーション  
[OAuth2 Authorization Framework][OAuth2-Role-Def] の定義によれば、[リソース所有者](#resource-owner)に代わって、保護されたリソースを要求するアプリケーションをいいます。"クライアント" という言葉の意味には、特定のハードウェア実装上の特性 (アプリケーションがサーバーで実行されるのか、デスクトップで実行されるのか、またはそれ以外のデバイスで実行されるのか、など) は含まれません。

クライアント アプリケーションは、リソース所有者に[承認](#authorization)を要求することによって、[OAuth2 承認付与](#authorization-grant)フローに参加し、リソース所有者に代わって API やデータにアクセスすることができます。OAuth2 Authorization Framework では、資格情報の機密維持に対するクライアントの能力に基づき、"confidential" と "public" という [2 種類のクライアント][OAuth2-Client-Types]を定義しています。アプリケーションは、Web サーバー上で実行される [Web クライアント (confidential)](#web-client) や、デバイス上にインストールされる[ネイティブ クライアント (public)](#native-client) アプリケーション、デバイスのブラウザーで実行される[ユーザー エージェント ベース クライアント (public)](#user-agent-based-client) を実装できます。

## 同意
[リソース所有者](#resource-owner)から[クライアント アプリケーション](#client-application)に承認 (リソース所有者に代わって保護されたリソースにアクセスするための具体的な[権限](#permissions)) を付与するプロセス。クライアントから要求された権限によっては、組織データまたは個人データへのアクセスを許可することへの同意を管理者 (組織のデータの場合) またはユーザー (個人のデータの場合) が求められます。さらに、[マルチテナント](#multi-tenant-application)のシナリオでは、同意するユーザーのテナントにアプリケーションの[サービス プリンシパル](#service-principal-object)が記録されます。

## ID トークン
[承認サーバー](#authorization-server)の[承認エンドポイント](#authorization-endpoint)から提供される [OpenID Connect][OpenIDConnect-ID-Token] [セキュリティ トークン](#security-token)。このトークンには、エンド ユーザーの[リソース所有者](#resource-owner)の認証に関連した[要求](#claim)が格納されます。ID トークンもアクセス トークンと同様、デジタル署名された [JSON Web トークン (JWT)][JWT] として表現されます。ただし、アクセス トークンとは異なり、ID トークンの要求は、リソース アクセス (特にアクセス制御) に関連した目的には使用されません。

詳細については、「[Azure AD のトークン リファレンス][AAD-Tokens-Claims]」を参照してください。

## マルチテナント アプリケーション
Azure AD に登録されるタイプの[クライアント アプリケーション](#client-application)で、最初に登録されたテナントに限らず、任意の Azure AD [テナント](#tenant)にプロビジョニングされたユーザー アカウントからのサインインと[同意](#consent)を許可するように設計されます。一方シングル テナントとして登録されるアプリケーションの場合は、アプリケーションの登録先と同じテナントにプロビジョニングされたユーザー アカウントからのみサインインが許可されます。[ネイティブ クライアント](#native-client) アプリケーションは、既定ではマルチテナントとなります。これに対し、[Web クライアント](#web-client) アプリケーションは、シングル テナントとマルチテナントを選択できるようになっています。

詳細については、「[マルチテナント アプリケーション パターンを使用してすべての Azure Active Directory (AD) ユーザーがサインインできるようにする方法][AAD-Multi-Tenant-Overview]」を参照してください。

## ネイティブ クライアント
デバイス上にネイティブでインストールされるタイプの[クライアント アプリケーション](#client-application)。すべてのコードがデバイス上で実行され、機密性を保った状態でプライベートに資格情報を保存することができないため、"public" クライアントと見なされます。詳細については、[OAuth2 のクライアント タイプとプロファイル][OAuth2-Client-Types]に関するセクションを参照してください。

## アクセス許可
[クライアント アプリケーション](#client-application)は、アクセス許可要求を宣言することで[リソース サーバー](#resource-server)へのアクセス権を取得します。次の 2 種類があります。

- "委任" されたアクセス許可。サインインした[リソース所有者](#resource-owner)から委任された承認の下で、[スコープに基づく](#scopes)アクセス権を要求します。実行時には、クライアントの[アクセス トークン](#access-token)に ["scp" 要求](#claim)として出現します。
- "アプリケーション" のアクセス許可。クライアント アプリケーションの資格情報/ID の下で[ロールベース](#roles)のアクセス権を要求します。実行時には、クライアントのアクセス トークンに ["roles" 要求](#claim)として出現します。

これらの要求は[同意](#consent)プロセス時にも出現し、管理者またはリソース所有者には、そのテナント内のリソースに対するクライアント アクセスを許可/拒否する機会が与えられます。

アクセス許可要求の構成は、[Azure クラシック ポータル][AZURE-classic-portal]の [アプリケーション] タブまたは [構成] タブにある [他のアプリケーションに対するアクセス許可] で、必要な "委任されたアクセス許可" と "アプリケーションのアクセス許可" (後者には全体管理者ロールのメンバーシップが必要) を選択することによって行います。[public クライアント](#client-application)は、資格情報を維持できないため、要求できるのは委任されたアクセス許可のみです。一方 [confidential クライアント](#client-application)は、委任されたアクセス許可とアプリケーションのアクセス許可のどちらでも要求することができます。クライアントの[アプリケーション オブジェクト](#application-object)は、宣言された権限をその [requiredResourceAccess プロパティ][AAD-Graph-App-Entity]に格納します。

## リソース所有者
[OAuth2 Authorization Framework][OAuth2-Role-Def] の定義によれば、保護されたリソースへのアクセス権を付与することのできるエンティティをいいます。リソース所有者が人である場合は、"エンド ユーザー" と呼ばれます。たとえば[クライアント アプリケーション](#client-application)は、[Microsoft Graph API][Microsoft-Graph] を介してユーザーのメールボックスにアクセスする必要があるとき、そのメールボックスのリソース所有者に権限を要求する必要があります。

## リソース サーバー
[OAuth2 Authorization Framework][OAuth2-Role-Def] の定義によれば、保護されたリソースのホストとして、[アクセス トークン](#access-token)を提示する[クライアント アプリケーション](#client-application)からのリソース要求 (保護されたリソースに対する要求) を受理し、応答する機能を備えたサーバーをいいます。保護されたリソース サーバーまたはリソース アプリケーションと呼ばれることもあります。

リソース サーバーは API を公開しており、そこで保護されているリソースに対しては、OAuth 2.0 Authorization Framework を使用して、[スコープ](#scopes)と[ロール](#roles)を介したアクセスが強制的に適用されます。たとえば、Azure AD テナント データへのアクセスを提供する Azure AD Graph API や、メール、カレンダー、ドキュメントといったデータへのアクセスを提供する Office 365 API があります。この 2 つの API は [Microsoft Graph API][Microsoft-Graph] から利用することもできます。

リソース アプリケーションの ID 構成は、クライアント アプリケーションと同様、Azure AD テナントへの[登録](#application-registration)を通じて確立され、アプリケーション オブジェクトとサービス プリンシパル オブジェクトの両方が得られます。Azure AD Graph API など、Microsoft が提供している一部の API には、あらかじめ登録されているサービス プリンシパルが存在し、プロビジョニング時にすべてのテナントで利用できるようになっています。

## roles
ロールは、[スコープ](#scopes)と同様、[リソース サーバー](#resource-server)の保護されたリソースへのアクセスを管理するための手段です。ロールには、"ユーザー" ロールと "アプリケーション" ロールの 2 種類があります。ユーザー ロールでは、リソースへのアクセスを必要とするユーザー/グループに関してロールベースのアクセス制御を実装します。これに対してアプリケーション ロールで実装するのは、アクセスを要求する[クライアント アプリケーション](#client-application)の場合と同じです。

ロールは、リソースによって定義される文字列 ("経費承認者"、"読み取り専用" など) です。[Azure クラシック ポータル][AZURE-classic-portal]からリソースの[アプリケーション マニフェスト](#application-manifest)を介して管理され、リソースの [appRoles プロパティ][AAD-Graph-Sp-Entity]に格納されます。クライアント アプリケーションが、"アプリケーション" ロールにアクセスするための[権限](#permissions)を要求すると、クライアントの[アクセス トークン](#access-token)の ["roles" 要求](#claim)で、実行時にその要求がリソースに提示されます。"ユーザー" ロールには、[Azure クラシック ポータル][AZURE-classic-portal]から Azure AD 管理者がユーザーを割り当てることができます。

Azure AD の Graph API によって公開されているアプリケーション ロールの詳しい説明については、[Graph API のアクセス許可スコープ][AAD-Graph-Perm-Scopes]に関するページを参照してください。具体的な実装例については、[Azure AD を使用したクラウド アプリケーションでのロール ベースのアクセス制御][Duyshant-Role-Blog]に関するページを参照してください。

## スコープ
スコープは、[ロール](#roles)と同様、[リソース サーバー](#resource-server)の保護されたリソースへのアクセスを管理するための手段です。リソースへの委任されたアクセス権を所有者から与えられている[クライアント アプリケーション](#client-application)に対し、[スコープ ベース][OAuth2-Access-Token-Scopes]のアクセス制御を実装する目的で使用されます。

スコープは、リソースによって定義される文字列 ("Mail.Read"、"Directory.ReadWrite.All" など) です。[Azure クラシック ポータル][AZURE-classic-portal]からリソースの[アプリケーション マニフェスト](#application-manifest)を介して管理され、リソースの [oauth2Permissions プロパティ][AAD-Graph-Sp-Entity]に格納されます。クライアント アプリケーションが、[アクセス許可](#permissions)を要求すると、クライアントの[アクセス トークン](#access-token)の ["scp" 要求](#claim)で、実行時にその要求がリソースに提示されます。

推奨される名前付け規則は、"resource.operation.constraint" 形式です。Azure AD の Graph API によって公開されているスコープの詳しい説明については、[Graph API のアクセス許可スコープ][AAD-Graph-Perm-Scopes]に関するページを参照してください。Office 365 サービスによって公開されているスコープについては、「[Office 365 API permissions reference (Office 365 API アクセス許可リファレンス)][O365-Perm-Ref]」を参照してください。

## セキュリティ トークン
要求を含んだ署名付きのドキュメント (OAuth2 トークン、SAML 2.0 アサーションなど)。OAuth 2.0 [承認付与](#authorization-grant)のケースでは、[アクセス トークン](#access-token) (OAuth2) と [ID Token](OpenID Connect) が一種のセキュリティ トークンになります。どちらも [JSON Web トークン (JWT)][JWT] として実装されます。

## サービス プリンシパル オブジェクト
[Azure クラシック ポータル][AZURE-classic-portal]でアプリケーションを登録/更新すると、そのテナントを対象に、アプリケーション オブジェクトおよび対応する[サービス プリンシパル オブジェクト](#application-object)の両方が作成/更新されます。アプリケーション オブジェクトは、アプリケーションの ID 構成をグローバルに (関連するアプリケーションがアクセスできるすべてのテナントに対して) "*定義*" します。このオブジェクトをひながたとして、対応するサービス プリンシパル オブジェクトが "*生成*" され、実行時にローカル (特定のテナント) で使用されます。

詳細については、「[アプリケーション オブジェクトおよびサービス プリンシパル オブジェクト][AAD-App-SP-Objects]」を参照してください。

## サインイン
[クライアント アプリケーション](#client-application)がエンド ユーザーの認証を開始し、関連する状態を認証後に収集するプロセス。[セキュリティ トークン](#security-token)を取得すると共に、アプリケーションのセッションをその状態に限定することを目的としています。状態には、ユーザー プロファイル情報などのアーティファクトや、トークンの要求から生成される情報が含まれている場合があります。

アプリケーションのサインイン機能は通常、シングル サインオン (SSO) を実装するために使用されます。一方 "サインアップ" は、エンド ユーザーが (初回サインイン時に) アプリケーションへのアクセス権を取得するための入口となる機能で、サインインの前にこの機能が実行されることもあります。サインアップ機能は、ユーザーごとの特別な状態を収集して永続化するために使用され、[ユーザーの同意](#consent)を必要とする場合があります。

## サインアウト
エンド ユーザーの認証を取り消し、[サインイン](#sign-in)の過程で[クライアント アプリケーション](#client-application)のセッションに関連付けられたユーザーの状態を解除するプロセス。

## テナント
Azure AD ディレクトリのインスタンスを "Azure AD テナント" といいます。テナントには、連携するアプリケーションのレジストリ サービス、ユーザー アカウントや登録済みアプリケーションの認証、各種プロトコル (OAuth2、SAML など) をサポートするうえで必要な REST エンドポイントなど、さまざまな機能が備わっています。エンドポイントには、[承認エンドポイント](#authorization-endpoint)や[トークン エンドポイント](#token-endpoint)のほか、[マルチテナント アプリケーション](#multi-tenant-application)で使用される "共通" エンドポイントがあります。

テナントを利用するための各種方法について詳しくは、「[Azure Active Directory テナントを取得する方法][AAD-How-To-Tenant]」をご覧ください。

## トークン エンドポイント
[承認サーバー](#authorization-server)によって実装されるエンドポイントの 1 つ。OAuth2 [承認付与](#authorization-grant)をサポートするために使用されます。付与された承認によっては、[クライアント](#client-application)に[アクセス トークン](#access-token) (と場合によっては "更新" トークン) を提供したり、その承認を [OpenID Connect][OpenIDConnect] プロトコルと組み合わせて [ID トークン](#ID-token)を提供したりすることができます。

## ユーザー エージェント ベースのクライアント
Web サーバーからコードをダウンロードしてユーザー エージェント (Web ブラウザーなど) 内で実行する[クライアント アプリケーション](#client-application)の一種。その例として Single Page Application (SPA) が挙げられます。すべてのコードがデバイス上で実行され、機密性を保った状態でプライベートに資格情報を保存することができないため、"public" クライアントと見なされます。詳細については、[OAuth2 のクライアント タイプとプロファイル][OAuth2-Client-Types]に関するセクションを参照してください。

## ユーザー プリンシパル
サービス プリンシパル オブジェクトはアプリケーション インスタンスを表現するためのセキュリティ プリンシパルです。一方、ユーザー プリンシパル オブジェクトも、セキュリティ プリンシパルのひとつですが、表現の対象となるのはユーザーです。ユーザー プリンシパル オブジェクトのスキーマは、Azure AD Graph の[ユーザー エンティティ][AAD-Graph-User-Entity]によって定義されます。ユーザー エンティティは、ユーザーに関連したプロパティ (姓と名、ユーザー プリンシパル名、ディレクトリ ロールにおけるメンバーシップなど) から成っており、実行時に Azure AD がユーザー プリンシパルを確立するために必要なユーザー ID 構成を提供します。ユーザー プリンシパルは、[同意](#consent)の委任を記録したり、アクセス制御の意思決定を行ったりするときに、認証済みのユーザーを表す目的で使用されます。

## Web クライアント
Web サーバーですべてのコードを実行するタイプの[クライアント アプリケーション](#client-application)。Web サーバーには、機密性を保った状態でプライベートに資格情報を保存できるので、Web クライアントは "confidential" クライアントと見なされます。詳細については、[OAuth2 のクライアント タイプとプロファイル][OAuth2-Client-Types]に関するセクションを参照してください。

## 次のステップ
「[Azure Active Directory 開発者ガイド][AAD-Dev-Guide]」をお読みください。Azure AD と[アプリケーションの統合][AAD-How-To-Integrate]や、[Azure AD 認証の基礎、サポートされる認証シナリオ][AAD-Auth-Scenarios]など、Azure AD 開発に関連したあらゆるトピックへの入口となっています。

Microsoft のコンテンツ改善のため、下部の Disqus コメント セクションよりご意見をお寄せください。

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/ja-JP/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken

<!---HONumber=AcomDC_0803_2016-->
<properties
   pageTitle="ロジック アプリのデプロイ テンプレートを作成する | Microsoft Azure"
   description="ロジック アプリのデプロイ テンプレートを作成し、リリース管理で使用する方法について説明します。"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/25/2016"
   ms.author="jehollan"/>

# ロジック アプリのデプロイ テンプレートの作成

ロジック アプリが作成された後は、このロジック アプリを Azure Resource Manager のテンプレートとして作成することができます。これにより、任意の環境またはリソース グループにロジック アプリを簡単にデプロイできます。Resource Manager テンプレートの概要は、「[Authoring Azure Resource Manager template (Azure Resource Manager テンプレートを作成する)](../resource-group-authoring-templates.md)」と「[Deploy resources with Azure Resource Manager template (Azure Resource Manager テンプレートでリソースをデプロイする)](../resource-group-template-deploy.md)」で確認できます。

## ロジック アプリ デプロイ テンプレート

ロジック アプリには、次の 3 つの基本的なコンポーネントがあります。

* **ロジック アプリ リソース**。このリソースには、価格設定計画、場所、ワークフロー定義などの情報が含まれています。
* **ワークフロー定義**。これは、コード ビューで表示されます。ワークフロー定義には、フロー手順の定義およびエンジンの実行方法が含まれています。これはロジック アプリ リソースの `definition` プロパティです。
* **接続**。接続文字列やアクセス トークンなど、コネクタ接続関連のメタデータを安全に保存するための個別のリソースです。ロジック アプリでは、ロジック アプリ リソースの `parameters` セクションで参照されます。

以上のコンポーネントを既存のロジック アプリで参照する場合、[Azure リソース エクスプローラー](http://resources.azure.com)などのツールを利用できます。

リソース グループ デプロイで使用するロジック アプリのテンプレートを作成するには、リソースを定義し、必要に応じてパラメーター化する必要があります。たとえば、開発環境、テスト環境、運用環境にデプロイする場合、環境ごとに異なる接続文字列を SQL データベースに使用すると効果的です。または、異なるサブスクリプションやリソース グループ内でデプロイすることもできます。

## ロジック アプリのデプロイ テンプレートの作成

ロジック アプリのデプロイ テンプレートの作成に役立つツールが提供されています。テンプレートは手書きで作成できます。つまり、上述のリソースを使用して、必要に応じてパラメータを作成できます。また、[Logic App Template Creator](https://github.com/jeffhollan/LogicAppTemplateCreator) という PowerShell モジュールを利用することもできます。このオープン ソース モジュールは、最初にロジック アプリとそれが利用している接続を評価し、デプロイに必要なパラメーターと共にテンプレート リソースを生成します。たとえば、ロジック アプリが Azure Service Bus キューからメッセージを受信し、Azure SQL データベースにデータを追加した場合、このツールはすべてのオーケストレーション ロジックを保存し、デプロイ時に設定できるように、SQL と Service Bus 接続文字列をパラメーター化します。

>[AZURE.NOTE] 接続はロジック アプリと同じリソース グループ内に置く必要があります。

### ロジック アプリ テンプレートの PowerShell モジュールのインストール

最も簡単なモジュールのインストール方法は、[PowerShell ギャラリー](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1)とコマンド `Install-Module -Name LogicAppTemplate` を使用することです。

PowerShell モジュールを手動でインストールすることもできます。

1. [ロジック アプリのテンプレート作成機能](https://github.com/jeffhollan/LogicAppTemplateCreator/releases)の最新リリースをダウンロードします。
1. PowerShell モジュール フォルダー (通常は `%UserProfile%\Documents\WindowsPowerShell\Modules`) にフォルダーを解凍します。

モジュールがあらゆるテナントやサブスクリプション アクセス トークンで機能するように、[ARMClient](https://github.com/projectkudu/ARMClient) コマンド ライン ツールの使用をお勧めします。この[ブログの投稿](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html)では、ARMClient についてさらに詳しく説明しています。

### PowerShell を使用したロジック アプリ テンプレートの生成

PowerShell をインストールした後は、次のコマンドを使用してテンプレートを生成できます。

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId` は Azure サブスクリプション ID です。この行はまず、ARMClient を使用してアクセス トークンを取得し、このアクセス トークンを PowerShell スクリプトにパイプして、JSON ファイルでテンプレートを作成します。

## ロジック アプリ テンプレートにパラメーターを追加する

ロジック アプリ テンプレートを作成したら、引き続き必要なパラメータを追加または変更できます。たとえば、定義により、1 回のデプロイでデプロイを予定している Azure 機能または入れ子になったワークフローにリソース ID が追加される場合、テンプレートにリソースを追加し、必要に応じて ID をパラメーター化すると便利です。これは、カスタム API や Swagger エンドポイントの参照をリソース グループごとにデプロイする予定の場合にも該当します。

## ロジック アプリ テンプレートをデプロイする

PowerShell、REST API、Visual Studio Release Management、Azure ポータル テンプレート デプロイなど、さまざまなツールを使用して、テンプレートをデプロイできます。詳細については、「[Deploy resources with Azure Resource Manager template (Azure Resource Manager テンプレートでリソースをデプロイする)](../resource-group-template-deploy.md)」を参照してください。パラメーターの値を保存する[パラメーター ファイル](../resource-group-template-deploy.md#parameter-file)の作成も推奨されます。

### OAuth 接続を作成する

デプロイ後、ロジック アプリは有効なパラメーターを使用してエンド ツー エンドで動作します。ただし、有効なアクセス トークンを生成するには、OAuth 接続を承認する必要があります。デザイナーでロジック アプリを開き、接続を承認することにより、これを行うことができます。または、自動化する場合は、各 OAuth 接続に対して同意するスクリプトを使用することができます。例として、[LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) プロジェクトの GitHub のスクリプトがあります。

## Visual Studio Release Management

環境をデプロイし、管理する一般的なシナリオは、Visual Studio Release Management のようなツールを ロジック アプリ デプロイ テンプレートと共に使用することです。Visual Studio Team Services に含まれる [Deploy Azure Resource Group (Azure リソース グループのデプロイ)](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) タスクは、あらゆるビルドまたはリリース パイプラインに追加できます。認証には[サービス プリンシパル](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)をデプロイする必要があります。その後、リリース定義を生成できます。

1. Release Management で、新しい定義を作成するには、**空**を選択して空の定義を開始します。

    ![新しい空の定義を作成します。][1]

1. これに必要なリソースを選択します。多くの場合、手作業で生成したか、ビルド プロセスで生成されたロジック アプリ テンプレートを選択します。
1. **Azure Resource Group Deployment (Azure リソース グループ デプロイ)** タスクを追加します。
1. [サービス プリンシパル](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)で構成し、テンプレート ファイルとテンプレート パラメーター ファイルを参照します。
1. 必要に応じて、その他の環境、自動化されたテスト、承認者のために、リリース プロセスの手順の構築を続行します。

<!-- Image References -->
[1]: ./media/app-service-logic-create-deploy-template/emptyReleaseDefinition.PNG

<!---HONumber=AcomDC_0803_2016-->
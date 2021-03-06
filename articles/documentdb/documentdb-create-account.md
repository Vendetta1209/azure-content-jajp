<properties
	pageTitle="DocumentDB アカウントを作成する方法 | Microsoft Azure"
	description="Azure DocumentDB で NoSQL データベースを作成します。手順に従って DocumentDB アカウントを作成し、きわめて高速なグローバル スケールの NoSQL データベースを作成する手順を紹介しています。" 
	keywords="データベースの作成"
	services="documentdb"
	documentationCenter=""
	authors="mimig1"
	manager="jhubbard"
	editor="monicar"/>

<tags
	ms.service="documentdb"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="08/25/2016"
	ms.author="mimig"/>

# Azure ポータルを使用して DocumentDB アカウントを作成する方法

> [AZURE.SELECTOR]
- [Azure ポータル](documentdb-create-account.md)
- [Azure CLI と ARM](documentdb-automation-resource-manager-cli.md)

Microsoft Azure DocumentDB でデータベースを作成するには、次の条件を満たす必要があります。

- Azure アカウントを入手する。まだ持っていない場合は、[無料 Azure アカウント](https://azure.microsoft.com/free)を取得できます。
- DocumentDB アカウントを作成する。

DocumentDB データベース アカウントは、Azure ポータル、Azure Resource Manager テンプレート、Azure CLI (コマンド ライン インターフェイス) のいずれかを使用して作成できます。この記事では、Azure Portal を使用して DocumentDB アカウントを作成する方法について説明します。Azure リソース マネージャーまたは Azure CLI を使用してアカウントを作成する方法については、[DocumentDB データベース アカウントを自動で作成する方法](documentdb-automation-resource-manager-cli.md)に関するページを参照してください。

DocumentDB を初めて使用する場合は、 Scott Hansleman による [こちら](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) の 4 分間のビデオを視聴し、オンライン ポータルで最もよく使用されるタスクの実行方法を確認してください。

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## 次のステップ

DocumentDB アカウントを作成できたら、次の手順として DocumentDB データベースを作成します。データベースを作成するには、次のいずれかを使用します。

- Azure ポータル。[Azure ポータルを使用した DocumentDB データベースの作成](documentdb-create-database.md)に関するページの説明を参照してください。
- GitHub の [azure-documentdb-net](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples) リポジトリの [DatabaseManagement](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples/DatabaseManagement) プロジェクトにある C# .NET のサンプル。
- 包括的なチュートリアル: [.NET](documentdb-get-started.md)、[.NET MVC](documentdb-dotnet-application.md)、[Java](documentdb-java-application.md)、[Node.js](documentdb-nodejs-application.md)、または [Python](documentdb-python-application.md)。
- [DocumentDB SDK](documentdb-sdk-dotnet.md)。DocumentDB には、.NET、Java、Python、Node.js、および JavaScript API の SDK があります。


データベースを作成した後に、データベースに [1 つまたは複数のコレクションを追加](documentdb-create-collection.md)し、それらのコレクションに[ドキュメントを追加する](documentdb-view-json-document-explorer.md)必要があります。

コレクションにドキュメントを用意した後で、ポータルの[クエリ エクスプローラー](documentdb-query-collections-query-explorer.md)、[REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx)、またはいずれかの [SDK](documentdb-sdk-dotnet.md) を使用することで、[DocumentDB SQL](documentdb-sql-query.md) を使用してドキュメントに対して[クエリを実行](documentdb-sql-query.md#executing-queries)することができます。

DocumentDB の詳細については、以下の資料を参照してください。

-	[DocumentDB のラーニング パス](https://azure.microsoft.com/documentation/learning-paths/documentdb/)
-	[DocumentDB のリソース モデルと概念](documentdb-resources.md)

<!---HONumber=AcomDC_0831_2016-->
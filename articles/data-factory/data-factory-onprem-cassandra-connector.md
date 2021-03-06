<properties 
	pageTitle="Data Factory を使用して Cassandra からデータを移動する | Microsoft Azure" 
	description="Azure Data Factory を使用してオンプレミスの Cassandra データベースからデータを移動する方法について説明します。" 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/07/2016" 
	ms.author="spelluru"/>

# Azure Data Factory を使用してオンプレミスの Cassandra データベースからデータを移動する
この記事では、Azure Data Factory のコピー アクティビティを使用して、オンプレミスの Cassandra データベースから、[サポートされているソースとシンク](data-factory-data-movement-activities.md#supported-data-stores) に関するセクションのシンクの欄に一覧表示されているデータ ストアにデータをコピーする方法について説明します。この記事は、「[データ移動アクティビティ](data-factory-data-movement-activities.md)」という記事に基づき、コピー アクティビティによるデータ移動の一般概要とサポートされるデータ ストアの組み合わせについて紹介しています。

現在のところ、Data Factory は Cassandra データベースから [サポートされているシンク データ ストア](data-factory-data-movement-activities.md#supported-data-stores)へのデータの移動のみに対応しており、他のデータ ストアから Cassandra データベースにデータを移動することはできません。

## 前提条件
Azure Data Factory サービスをオンプレミスの Cassandra データベースに接続できるようにするには、次をインストールする必要があります。

- データベースをホストするコンピューターと同じコンピューター、またはデータベースとのリソースの競合を避けるために別のコンピューター上にインストールされた、バージョン 2.0 以降の Data Management Gateway。Data Management Gateway は、安全かつ管理された方法でオンプレミスのデータをクラウド サービスに接続するソフトウェアです。Data Management Gateway の詳細については、[オンプレミスとクラウド間でのデータ移動](data-factory-move-data-between-onprem-and-cloud.md)に関する記事を参照してください。
  
	ゲートウェイをインストールすると、Cassandra データベースへの接続に使用される Microsoft Cassandra ODBC ドライバーが自動的にインストールされます。

> [AZURE.NOTE] 接続/ゲートウェイに関する問題のトラブルシューティングのヒントについては、[ゲートウェイの問題のトラブルシューティング](data-factory-data-management-gateway.md#troubleshoot-gateway-issues)に関するセクションをご覧ください。

## データのコピー ウィザード
Cassandra データベースから、サポートされているシンク データ ストアにデータをコピーするパイプラインを作成する最も簡単な方法は、データのコピー ウィザードを使用することです。データのコピー ウィザードを使用してパイプラインを作成する簡単な手順については、「[チュートリアル: コピー ウィザードを使用してパイプラインを作成する](data-factory-copy-data-wizard-tutorial.md)」を参照してください。

次の例は、[Azure ポータル](data-factory-copy-activity-tutorial-using-azure-portal.md)、[Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)、または [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) を使用してパイプラインを作成する際に使用できるサンプルの JSON 定義です。これらの例は、Cassandra データベースから Azure BLOB ストレージにデータをコピーする方法を示しています。ただし、Azure Data Factory のコピー アクティビティを使用して、[こちら](data-factory-data-movement-activities.md#supported-data-stores)に記載されているシンクのいずれかにデータをコピーすることができます。


## サンプル: Cassandra から BLOB へのデータのコピー
このサンプルでは、1 時間おきに Cassandra データベースから Azure BLOB にデータがコピーされます。これらのサンプルで使用される JSON プロパティの説明はサンプルに続くセクションにあります。Azure Data Factory のコピー アクティビティを使用して、[データ移動アクティビティ](data-factory-data-movement-activities.md#supported-data-stores)に関する記事に記載されているシンクのいずれかにデータを直接コピーすることができます。

- [OnPremisesCassandra](#onpremisescassandra-linked-service-properties) 型のリンクされたサービス。
- [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) 型のリンクされたサービス。
- [CassandraTable](#cassandratable-properties) 型の入力[データセット](data-factory-create-datasets.md)。
- [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) 型の出力[データセット](data-factory-create-datasets.md)。
- [CassandraSource](#cassandrasource-type-properties) と [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) を使用するコピー アクティビティを含む[パイプライン](data-factory-create-pipelines.md)。

**Cassandra のリンクされたサービス**

この例では、**Cassandra** のリンクされたサービスを使用します。このリンクされたサービスでサポートされているプロパティについては、[Cassandra のリンクされたサービス](#onpremisescassandra-linked-service-properties)に関するセクションをご覧ください。

	{
    	"name": "CassandraLinkedService",
    	"properties":
    	{
			"type": "OnPremisesCassandra",
			"typeProperties":
			{
				"authenticationType": "Basic",
				"host": "mycassandraserver",
				"port": 9042,
				"username": "user",
				"password": "password",
				"gatewayName": "mygateway"
        	}
    	}
	}


**Azure Storage のリンクされたサービス**

	{
		"name": "AzureStorageLinkedService",
		"properties": {
		"type": "AzureStorage",
			"typeProperties": {
				"connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
			}
		}
	}

**Cassandra の入力データセット**

	{
		"name": "CassandraInput",
		"properties": {
			"linkedServiceName": "CassandraLinkedService",
			"type": "CassandraTable",
			"typeProperties": {
				"tableName": "mytable",
				"keySpace": "mykeyspace" 
			},
			"availability": {
				"frequency": "Hour",
				"interval": 1
			},
			"external": true,
			"policy": {
				"externalData": {
					"retryInterval": "00:01:00",
					"retryTimeout": "00:10:00",
					"maximumRetry": 3
				}
			}
		}
	}

**external** を **true** に設定すると、データセットが Data Factory の外部にあり、Data Factory のアクティビティによって生成されたものではないことが Data Factory サービスに通知されます。

**Azure BLOB の出力データセット**

データは新しい BLOB に 1 時間おきに書き込まれます (頻度: 時間、間隔: 1)。

	{
		"name": "AzureBlobOutput",
		"properties":
		{
			"type": "AzureBlob",
			"linkedServiceName": "AzureStorageLinkedService",
			"typeProperties":
			{
				"folderPath": "adfgetstarted/fromcassandra"
			},
			"availability":
			{
				"frequency": "Hour",
				"interval": 1
			}
		}
	}


**コピー アクティビティのあるパイプライン**

上記の入力データセットと出力データセットを使用するように構成され、1 時間おきに実行するようにスケジュールされているコピー アクティビティがパイプラインに含まれています。パイプライン JSON 定義で、**source** の型が **CassandraSource** に設定され、**sink** の型が **BlobSink** に設定されています。

RelationalSource でサポートされるプロパティの一覧については、「[RelationalSource type プロパティ](#cassandrasource-type-properties)」をご覧ください。
	
	{  
		"name":"SamplePipeline",
		"properties":{  
			"start":"2016-06-01T18:00:00",
			"end":"2016-06-01T19:00:00",
			"description":"pipeline with copy activity",
			"activities":[  
			{
				"name": "CassandraToAzureBlob",
				"description": "Copy from Cassandra to an Azure blob",
				"type": "Copy",
				"inputs": [
				{
					"name": "CassandraInput"
				}
				],
				"outputs": [
				{
					"name": "AzureBlobOutput"
				}
				],
				"typeProperties": {
					"source": {
						"type": "CassandraSource",
						"query": "select id, firstname, lastname from mykeyspace.mytable"
		
					},
					"sink": {
						"type": "BlobSink"
					}
				},
				"scheduler": {
					"frequency": "Hour",
					"interval": 1
				},
				"policy": {
					"concurrency": 1,
					"executionPriorityOrder": "OldestFirst",
					"retry": 0,
					"timeout": "01:00:00"
				}
			}
			]	
		}
	}
## OnPremisesCassandra のリンクされたサービスのプロパティ

次の表は、Cassandra のリンクされたサービスに固有の JSON 要素の説明をまとめたものです。

| プロパティ | 説明 | 必須 |
| -------- | ----------- | -------- | 
| type | type プロパティを **OnPremisesCassandra** に設定する必要があります。 | はい |
| host | Cassandra サーバーの 1 つまたは複数の IP アドレスかホスト名。<br/><br/>IP アドレスまたはホスト名のコンマ区切りのリストを指定して、すべてのサーバーに同時に接続します。 | はい | 
| ポート | Cassandra サーバーがクライアント接続のリッスンに使用する TCP ポート。 | いいえ、既定値: 9042 |
| authenticationType | Basic、または匿名 | はい |
| username |ユーザー アカウントのユーザー名を指定します。 | はい (authenticationType が Basic に設定されている場合)。 |
| パスワード | ユーザー アカウントのパスワードを指定します。 | はい (authenticationType が Basic に設定されている場合)。 |
| gatewayName | オンプレミスの Cassandra データベースへの接続に使用されるゲートウェイの名前。 | はい |
| encryptedCredential | ゲートウェイによって暗号化された資格情報。 | いいえ | 

## CassandraTable プロパティ

データセットの定義に利用できるセクションとプロパティの完全な一覧については、「[データセットの作成](data-factory-create-datasets.md)」という記事を参照してください。データセット JSON の構造、可用性、ポリシーなどのセクションはすべてのデータセット型 (Azure SQL、Azure BLOB、Azure テーブルなど) で同じです。

**typeProperties** セクションはデータセット型ごとに異なり、データ ストアのデータの場所などに関する情報を提供します。**CassandraTable** 型のデータセットの typeProperties セクションには次のプロパティがあります。

| プロパティ | 説明 | 必須 |
| -------- | ----------- | -------- |
| keyspace | Cassandra データベースのkeyspace またはスキーマの名前。 | はい (**CassandraSource** の**クエリ**が定義されていない場合)。 |
| tableName | Cassandra データベースのテーブル名。 | はい (**CassandraSource** の**クエリ**が定義されていない場合)。 |


## CassandraSource type プロパティ
アクティビティの定義に利用できるセクションとプロパティの完全な一覧については、「[パイプラインの作成](data-factory-create-pipelines.md)」を参照してください。名前、説明、入力テーブル、出力テーブル、さまざまなポリシーなどのプロパティがあらゆる種類のアクティビティで利用できます。

一方で、アクティビティの typeProperties セクションで利用できるプロパティはアクティビティの種類により異なり、コピー アクティビティの場合、source と sink の種類によって異なります。

コピー アクティビティで、source が **CassandraSource** 型の場合は、typeProperties セクションで次のプロパティを使用できます。

| プロパティ | 説明 | 使用できる値 | 必須 |
| -------- | ----------- | -------------- | -------- |
| query | カスタム クエリを使用してデータを読み取ります。 | SQL-92 クエリまたはCQL クエリ。「[CQL reference (CQL リファレンス)](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html)」をご覧ください。<br/><br/>SQL クエリを使用する場合は、クエリを実行するテーブルを表す **keyspace name.table name** を指定します。 | いいえ (データセットの tableName と keyspace が定義されていない場合)。 |
| consistencyLevel | 一貫性レベルは、データがクライアント アプリケーションに返される前に、読み取り要求に応答する必要があるレプリカの数を指定します。Cassandra は読み取り要求を満たすために、データの指定された数のレプリカを確認します。 | ONE、TWO、THREE、QUORUM、ALL、 LOCAL\_QUORUM、EACH\_QUORUM、 LOCAL\_ONE。詳細については、「[Configuring data consistency (データ整合性の構成)](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html)」をご覧ください。 | いいえ。既定値は ONE です。 |  


### Cassandra の型マッピング
Cassandra の型 | .Net ベースの型
--------------- | ---------------
ASCII |	String 
BIGINT | Int64
BLOB | Byte
BOOLEAN | Boolean
DECIMAL | Decimal
DOUBLE | Double
FLOAT | Single
INET | String
INT | Int32
TEXT | String
TIMESTAMP | DateTime
TIMEUUID | Guid
UUID | Guid
VARCHAR | String
VARINT | Decimal

> [AZURE.NOTE]  
コレクションの種類 (マップ、セット、リストなど) については、[仮想テーブルを使用した Cassandra コレクションの種類の取り扱い](#work-with-collections-using-virtual-table)に関するセクションをご覧ください。
> 
> ユーザー定義型はサポートされていません。
> 
> バイナリ列と文字列の列の長さは 4000 バイト以内である必要があります。

## 仮想テーブルを使用したコレクションの取り扱い
Azure Data Factory では、ビルトインの ODBC ドライバーを使用して Cassandra データベースへの接続や、Cassandra データベースからのデータのコピーを行います。マップ、セット、およびリストを含むコレクションの種類については、ODBC ドライバーが、対応する仮想テーブルへのデータの再正規化を行います。具体的には、テーブルにコレクション列が含まれている場合に、ドライバーが次の仮想テーブルを生成します。

-	実テーブルと同じデータ (コレクション列を除く) を含む **ベース テーブル**。ベース テーブルには、それが表す実テーブルと同じ名前が使用されます。
-	入れ子になったデータを展開する、各コレクション列の**仮想テーブル**。コレクションを表す仮想テーブルには、実テーブルの名前、区切り文字の "_vt_"、および列の名前を使用して名前が付けられます。

仮想テーブルは、非正規化データへのドライバーのアクセスを有効にして、実テーブルのデータを参照します。 詳細については、次の「例」を参照してください。仮想テーブルのクエリや結合により、Cassandra コレクションの内容にアクセスできます。

[コピー ウィザード](data-factory-data-movement-activities.md#data-factory-copy-wizard)を利用して、仮想テーブルを含む Cassandra データベースのテーブルの一覧を直感的に表示し、内部データをプレビューすることができます。また、コピー ウィザードでクエリを構築して検証し、結果を確認することもできます。

### 例
たとえば、次の "ExampleTable" は、整数の主キー列 ("pk\_int" という名前)、テキスト列 (値という名前)、リスト列、 マップ列、 およびセット列 ("StringSet" という名前) で構成された、Cassandra データベース テーブルです。

pk\_int | 値 | 一覧表示 | マップ |	StringSet
------ | ----- | ---- | --- | --------
1 | "サンプル値 1" | ["1", "2", "3"] | {"S1": "a", "S2": "b"} | {"A", "B", "C"}
3 | "サンプル値 3" | ["100", "101", "102", "105"] | {"S1": "t"} | {"A", "E"}

ドライバーによって、この単一のテーブルを表す複数の仮想テーブルが生成されます。 仮想テーブルの外部キー列は、実テーブルの主キー列を参照して、仮想テーブルの行が対応する実テーブルの行を示します。

最初の仮想テーブルは、次に示す "ExampleTable" という名前のベース テーブルです。ベース テーブルには、元のデータベース テーブルと同じデータ (コレクションを除く) が含まれています。このテーブルで省略されたコレクションは、他の仮想テーブルで展開されます。

pk\_int | 値
------ | -----
1 | "サンプル値 1"
3 | "サンプル値 3"

次のテーブルは、リスト、マップ、および StringSet の列からのデータを再正規化した仮想テーブルを表しています。"\_index" や "\_key" で終わる名前の列は、元のリストまたはマップ内のデータの位置を示しています。 "\_value" で終わる名前の列には、コレクションから展開されたデータが含まれています。

#### テーブル "ExampleTable\_vt\_List":
pk\_int | List\_index | List\_value
------ | ---------- | ----------
1 | 0 | 1
1 | 1 | 2
1 | 2 | 3
3 | 0 | 100
3 | 1 | 101
3 | 2 | 102
3 | 3 | 103

#### テーブル "ExampleTable\_vt\_Map":
pk\_int | Map\_key | Map\_value
------ | ------- | ---------
1 | S1 | A
1 | S2 | b
3 | S1 | t

#### テーブル "ExampleTable\_vt\_StringSet":
pk\_int | StringSet\_value
------ | ---------------
1 | A
1 | B
1 | C
3 | A
3 | E





[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]
[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## パフォーマンスとチューニング  
Azure Data Factory でのデータ移動 (コピー アクティビティ) のパフォーマンスに影響する主な要因と、パフォーマンスを最適化するための各種方法については、「[コピー アクティビティのパフォーマンスとチューニングに関するガイド](data-factory-copy-activity-performance.md)」をご覧ください。

<!---HONumber=AcomDC_0817_2016-->
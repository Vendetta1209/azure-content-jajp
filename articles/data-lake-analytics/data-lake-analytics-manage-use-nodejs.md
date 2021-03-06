<properties
   pageTitle="Azure SDK for Node.js を使用して Azure Data Lake Analytics を管理する | Azure"
   description="Azure SDK for Node.js を使用して Data Lake Analytics アカウント、データ ソース、ジョブ、およびユーザーを管理する方法について説明します"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# Azure SDK for Node.js を使用して Azure Data Lake Analytics を管理する


[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Azure SDK for Node.js を使用して、Azure Data Lake Analytics アカウント、ジョブ、およびカタログを管理できます。他のツールを使用する管理のトピックを表示するには、上のタブ セレクターをクリックします。

現時点では、以下がサポートされています。

  *  **Node.js のバージョン: 0.10.0 以降**
  *  **アカウント用の REST API のバージョン: 2015-10-01-preview**
  *  **カタログ用の REST API のバージョン: 2015-10-01-preview**
  *  **ジョブ用の REST API のバージョン: 2016-03-20-preview**

## 特徴

- アカウント管理: 作成、取得、一覧表示、更新、および削除。
- ジョブ管理: 送信、取得、一覧表示、取り消し。
- カタログ管理: 取得、一覧表示、作成 (シークレット)、更新 (シークレット)、削除 (シークレット)。

## インストール方法

```bash
npm install azure-arm-datalake-analytics
```

## Azure Active Directory を使用して認証する

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## Data Lake Analytics クライアントを作成する

```javascript
var adlaManagement = require("azure-arm-datalake-analytics");
var acccountClient = new adlaManagement.DataLakeAnalyticsAccountClient(credentials, 'your-subscription-id');
var jobClient = new adlaManagement.DataLakeAnalyticsJobClient(credentials, 'azuredatalakeanalytics.net');
var catalogClient = new adlaManagement.DataLakeAnalyticsCatalogClient(credentials, 'azuredatalakeanalytics.net');
```

## Data Lake Analytics アカウントを作成する

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlaacct';
var location = 'eastus2';

// A Data Lake Store account must already have been created to create
// a Data Lake Analytics account. See the Data Lake Store readme for
// information on doing so. For now, we assume one exists already.
var datalakeStoreAccountName = 'existingadlsaccount';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location,
  properties: {
    defaultDataLakeStoreAccount: datalakeStoreAccountName,
    dataLakeStoreAccounts: [
      {
        name: datalakeStoreAccountName
      }
    ]
  }
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## ジョブの一覧を取得する

```javascript
var util = require('util');
var accountName = 'testadlaacct';
jobClient.job.list(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## Data Lake Analytics カタログ内のデータベースの一覧を取得する
```javascript
var util = require('util');
var accountName = 'testadlaacct';
catalogClient.catalog.listDatabases(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## 関連項目

- [Microsoft Azure SDK for Node.js](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure SDK for Node.js - Data Lake Store の管理](https://github.com/Azure/azure-sdk-for-node/tree/autorest/lib/services/dataLake.Store)

<!---HONumber=AcomDC_0615_2016-->
<properties
   pageTitle="Service Fabric アプリケーションのデプロイ | Microsoft Azure"
   description="Service Fabric のアプリケーションをデプロイおよび削除する方法"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/10/2016"
   ms.author="ryanwi"/>

# PowerShell を使用してアプリケーションのデプロイと削除を実行する

[アプリケーションの種類をパッケージ化][10]した後は、Azure Service Fabric クラスターにデプロイできる状態になっています。デプロイには、次の 3 つの手順が含まれます。

1. アプリケーション パッケージをアップロードする
2. アプリケーションの種類を登録する
3. アプリケーション インスタンスを作成する

>[AZURE.NOTE] Visual Studio を使用してローカルの開発クラスターでアプリケーションのデプロイとデバッグを行う場合、以下のすべての手順は、アプリケーション プロジェクトのスクリプト フォルダーにある PowerShell スクリプトによって自動的に処理されます。この記事では、これらのスクリプトが実行する内容の背景を説明し、Visual Studio の外部で同じ操作を実行できるようにします。

## アプリケーション パッケージをアップロードする

アプリケーション パッケージをアップロードすると、そのパッケージは内部 Service Fabric コンポーネントがアクセスできる場所に保存されます。PowerShell を使用してアップロードを実行できます。この記事の PowerShell コマンドを実行する前に、必ず最初に [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) で Service Fabric クラスターに接続してください。

必要なアプリケーション マニフェストとサービス マニフェストのほか、コード パッケージ、構成パッケージ、データ パッケージが含まれている *MyApplicationType* という名前のフォルダーがあるとします。[Copy-ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125905.aspx) コマンドは、クラスター イメージ ストアにパッケージをアップロードします。Service Fabric SDK PowerShell モジュールの一部である **Get-ImageStoreConnectionStringFromClusterManifest** コマンドレットは、イメージ ストアの接続文字列の取得に使用します。SDK モジュールをインポートするには、次のコマンドを実行します。

```
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

アプリケーション パッケージを *C:\\users\\ryanwi\\Documents\\Visual Studio 2015\\Projects\\MyApplication\\myapplication\\pkg\\debug* から *c:\\temp\\MyApplicationType* にコピーできます ("debug" ディレクトリの名前を "MyApplicationType" に変更します)。次の例では、パッケージをアップロードします。

~~~
PS C:\temp> dir

    Directory: c:\temp


Mode                LastWriteTime         Length Name                                                                                   
----                -------------         ------ ----                                                                                   
d-----        8/12/2016  10:23 AM                MyApplicationType                                                                          

PS C:\temp> tree /f .\Stateless1Pkg

C:\TEMP\MyApplicationType
│   ApplicationManifest.xml
|
└───Stateless1Pkg
    |   ServiceManifest.xml
    │
    └───Code
    │   │  Microsoft.ServiceFabric.Data.dll
    │   │  Microsoft.ServiceFabric.Data.Interfaces.dll
    │   │  Microsoft.ServiceFabric.Internal.dll
    │   │  Microsoft.ServiceFabric.Internal.Strings.dll
    │   │  Microsoft.ServiceFabric.Services.dll
    │   │  ServiceFabricServiceModel.dll
    │   │  MyService.exe
    │   │  MyService.exe.config
    │   │  MyService.pdb
    │   │  System.Fabric.dll
    │   │  System.Fabric.Strings.dll
    │   │
    │   └───ja-JP
    |         Microsoft.ServiceFabric.Internal.Strings.resources.dll
    |         System.Fabric.Strings.resources.dll
    |
    ├───Config
    │     Settings.xml
    │
    └───Data
          init.dat

PS C:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath MyApplicationType -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
Copy application package succeeded

PS D:\temp>
~~~

## アプリケーション パッケージを登録する

アプリケーション パッケージを登録すると、アプリケーション マニフェストで宣言されたアプリケーションの種類とバージョンを利用できるようになります。システムは、前の手順でアップロードされたパッケージの読み取り、パッケージの検証 ([Test-ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125950.aspx) をローカルで実行することに相当)、パッケージのコンテンツの処理、および内部システムの場所への処理済みのパッケージのコピーを実行します。

~~~
PS D:\temp> Register-ServiceFabricApplicationType MyApplicationType
Register application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp>
~~~

[Register-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125958.aspx) コマンドは、アプリケーション パッケージがシステムによって正常にコピーされた場合にのみ戻ります。この所要時間は、アプリケーション パッケージの内容によって異なります。必要に応じて **-TimeoutSec** パラメーターを使用してタイムアウトを長く設定できます (既定のタイムアウトは 60 秒です)。

[Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125871.aspx) コマンドは、正常に登録されたアプリケーションの種類の全バージョンを一覧表示します。

## アプリケーションを作成する

[New-ServiceFabricApplication](https://msdn.microsoft.com/library/mt125913.aspx) コマンドを使用して、正常に登録されたアプリケーションの種類のバージョンでアプリケーションをインスタンス化できます。各アプリケーションの名前は、*fabric:* スキームで開始され、各アプリケーション インスタンスに対して一意でなければなりません。ターゲット アプリケーションの種類のアプリケーション マニフェストに定義されている既定のサービスも同時に作成されます。

~~~
PS D:\temp> New-ServiceFabricApplication fabric:/MyApp MyApplicationType AppManifestVersion1

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationStatus      : Ready
HealthState            : OK
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/MyService
ServiceKind            : Stateless
ServiceTypeName        : MyServiceType
IsServiceGroup         : False
ServiceManifestVersion : SvcManifestVersion1
ServiceStatus          : Active
HealthState            : Ok

PS D:\temp>
~~~

[Get-ServiceFabricApplication](https://msdn.microsoft.com/library/mt163515.aspx) コマンドは、正常に作成されたすべてのアプリケーション インスタンスとその全体的な状態を一覧表示します。

[Get-ServiceFabricService](https://msdn.microsoft.com/library/mt125889.aspx) コマンドは、特定のアプリケーション インスタンス内で正常に作成されたすべてのサービス インスタンスを一覧表示します。既定のサービス (指定されている場合) は、ここに表示されます。

登録されたアプリケーションの種類の任意のバージョンに対して、複数のアプリケーション インスタンスを作成できます。各アプリケーション インスタンスは分離して実行され、独自の作業ディレクトリとプロセスを使用します。

## アプリケーションの削除

アプリケーション インスタンスが不要になった場合、[Remove-ServiceFabricApplication](https://msdn.microsoft.com/library/mt125914.aspx) コマンドを使用して完全に削除できます。このコマンドを使用すると、アプリケーションに属するすべてのサービスも自動的に削除されます。その結果、すべてのサービスの状態が完全に削除されます。この操作は元に戻せません。また、アプリケーションの状態を復元できません。

~~~
PS D:\temp> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS D:\temp> Get-ServiceFabricApplication
PS D:\temp>
~~~

アプリケーションの種類の特定のバージョンが不要になった場合は、[Unregister-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125885.aspx) コマンドを使用して登録を解除する必要があります。未使用の種類の登録を解除すると、イメージ ストアでその種類のアプリケーション パッケージのコンテンツによって使用されている記憶域が解放されます。あるアプリケーションの種類に対してインスタンス化されたアプリケーションがなく、それを参照している保留中のアプリケーションのアップグレードもない場合に、そのアプリケーションの種類の登録を解除できます。

~~~
PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp> Unregister-ServiceFabricApplicationType MyApplicationType AppManifestVersion1
Unregister application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

PS D:\temp>
~~~

## トラブルシューティング

### Copy-ServiceFabricApplicationPackage がImageStoreConnectionString を求める

Service Fabric SDK 環境には、適切な既定値を事前に設定しておく必要があります。ただし、すべてのコマンドの ImageStoreConnectionString が、Service Fabric クラスターによって使用されている値に一致していなければならない場合もあります。この値は [Get-ServiceFabricClusterManifest](https://msdn.microsoft.com/library/mt126024.aspx) コマンドで取得したクラスター マニフェストで確認できます。

~~~
PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType

cmdlet Copy-ServiceFabricApplicationPackage at command pipeline position 1
Supply values for the following parameters:
ImageStoreConnectionString:

PS D:\temp> Get-ServiceFabricClusterManifest
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]

PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType -ImageStoreConnectionString file:D:\ServiceFabric\Data\ImageStore
Copy application package succeeded

PS D:\temp>
~~~

## 次のステップ

[Service Fabric アプリケーションのアップグレード](service-fabric-application-upgrade.md)

[Service Fabric の正常性の概要](service-fabric-health-introduction.md)

[Service Fabric サービスの診断とトラブルシューティング](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric のアプリケーションのモデル化](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md

<!---HONumber=AcomDC_0817_2016-->
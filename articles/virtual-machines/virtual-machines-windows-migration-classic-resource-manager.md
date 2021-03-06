<properties
	pageTitle="プラットフォームでサポートされているクラシックから Azure Resource Manager への IaaS リソースの移行 | Microsoft Azure"
	description="この記事では、プラットフォームでサポートされているクラシックから Azure Resource Manager へのリソースの移行について説明します"
	services="virtual-machines-windows"
	documentationCenter=""
	authors="mahthi"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/22/2016"
	ms.author="mahthi"/>

# プラットフォームでサポートされているクラシックから Azure Resource Manager への IaaS リソースの移行

この記事では、クラシックから Resoure Manager デプロイメント モデル への、サービスとしてのインフラストラクチャ (IaaS) リソースの移行を有効にする方法について説明します。[Azure Resource Manager の機能と利点](../resource-group-overview.md)の詳細を参照してください。仮想ネットワーク サイト間ゲートウェイを使用して、サブスクリプション内で共存する 2 つのデプロイメント モデルのリソースを接続する方法について詳しく説明します。

## 移行の目的

Resource Manager では、テンプレートを使用して複雑なアプリケーションをデプロイできます。また、VM の拡張機能を使用して仮想マシンを構成し、アクセス管理とタグ付けを統合します。Azure Resource Manager には、仮想マシンの可用性セットへのスケーラブルな並列デプロイも含まれます。さらに、新しいモデルでは、計算、ネットワーク、ストレージの個別のライフサイクル管理が提供されます。最後に、仮想ネットワークでの仮想マシンの実行によって、セキュリティが既定で有効になることが重要視されています。

機能面について言うと、クラシック デプロイメント モデルの計算、ネットワーク、およびストレージ機能のほとんどが、Azure Resource Manager でもサポートされています。Azure Resource Manager の新機能を活用するには、クラシック デプロイメント モデルから既存のデプロイを移行します。

## 移行後のオートメーションおよびツールの変更

クラシック デプロイメント モデルから Resource Manager デプロイメント モデルへのリソース移行の一環として、既存のオートメーションまたはツールを、リソース移行後も確実に機能し続けるように更新する必要があります。

## クラシックから Resource Manager への IaaS リソース移行の意味

詳しく説明する前に、IaaS リソースでのデータ プレーンと管理プレーンの操作の違いについて簡単に説明したいと思います。

- "管理プレーン" は、リソースを変更するための管理プレーンまたは API を指します。たとえば、VM の作成、VM の再起動、新しいサブネットでの仮想ネットワークの更新などの操作は、実行中のリソースを管理しますが、インスタンスへの接続には直接影響しません。
- "データ プレーン" (アプリケーション) はアプリケーション自体のランタイムを指します。Azure API を経由しないインスタンスとの対話が含まれます。Web サイトへのアクセスまたは実行中の SQL Server インスタンスや MongoDB サーバーからのデータのプルは、データ プレーンまたはアプリケーション対話と見なされます。ストレージ アカウントから BLOB をコピーすることや、パブリック IP アドレスにアクセスして RDP または SSH で仮想マシンに接続することもデータ プレーンです。これらの操作の場合、計算、ネットワーク、およびストレージでアプリケーションは実行されたままになります。

>[AZURE.NOTE] 一部の移行シナリオでは、Azure Platform が仮想マシンを停止および割り当て解除してから再起動します。これにより、短時間のデータ プレーン ダウンタイムが発生します。

## 移行のサポート対象範囲

主に計算、ネットワーク、およびストレージの 3 つの移行範囲を対象としています。

### 仮想マシンの移行 (仮想ネットワーク外)

Resource Manager デプロイメント モデルでは、既定でアプリケーションのセキュリティが適用されます。すべての VM が Resource Manager モデルの仮想ネットワーク内にある必要があります。Azure Platform は移行の一環として、仮想マシンを再起動 (`Stop`、`Deallocate`、および `Start`) します。仮想ネットワークについては、2 つのオプションがあります。

- まず、新しい仮想ネットワークを作成し、仮想マシンをその新しい仮想ネットワークに移行するようにプラットフォームに要求できます。
- また、Resource Manager の既存の仮想ネットワークに仮想マシンを移行することもできます。

>[AZURE.NOTE] この移行範囲では、移行中のある期間、管理プレーンおよびデータ プレーンのいずれの操作も許可されない場合があります。

### 仮想マシンの移行 (仮想ネットワーク内)

ほとんどの VM 構成では、クラシック デプロイメント モデルと Resource Manager デプロイメント モデルの間でメタデータのみが移行されます。基になる VM は、同じネットワーク内の同じストレージを使用する同じハードウェアで実行されます。移行中の一定期間は、管理プレーン操作が許可されない場合があります。ただし、データ プレーンは引き続き機能します。つまり、移行中は VM (クラシック) 上で実行されているアプリケーションでダウンタイムが発生することはありません。

次の構成は現在サポートされていません。今後、これらのサポートが追加されたときに、この構成の一部の VM でダウンタイムが発生する場合があります (VM の動作の停止、割り当て解除、再起動が行われます)。

-	単一のクラウド サービスに複数の可用性セットがある。
-	単一のクラウド サービスに 1 つ以上の可用性セットと、可用性セットに存在しない VM がある。

>[AZURE.NOTE] 単一のクラウド サービスに 1 つ以上の可用性セットと、可用性セットに属さない VM がある上記の特定の構成の場合は、データ プレーン ダウンタイムが発生します。

### ストレージ アカウントの移行

シームレスに移行できるように、クラシック ストレージ アカウントで Resource Manager VM をデプロイできます。この機能により、ストレージ アカウントとは関係なく、コンピューティングおよびネットワーク リソースを移行できます。また、移行する必要があります。仮想マシンおよび仮想ネットワークを移行したら、ストレージ アカウントを移行して、移行プロセスを完了する必要があります。

>[AZURE.NOTE] Resource Manager デプロイメント モデルには、従来のイメージおよびディスクという概念がありません。クラシック イメージやディスクは、ストレージ アカウントを移行すると Resource Manager スタックには表示されなくなりますが、バッキング VHD はストレージ アカウントに残ります。

## サポートされていない機能と構成

現時点では、一部の機能および構成セットはサポートされていません。以下のセクションでは、こうした機能や構成セットに関する推奨事項について説明します。

### サポートされていない機能

次の機能は現在サポートされていません。必要に応じて、これらの設定を削除し、VM を移行してから、Resource Manager デプロイメント モデルでその設定を再度有効にすることができます。

リソース プロバイダー | 機能
---------- | ------------
計算 | 関連付けのない仮想マシン ディスク。
計算 | 仮想マシン イメージ。
ネットワーク | エンドポイント ACL。
ネットワーク | 仮想ネットワーク ゲートウェイ (サイト間、Azure ExpressRoute、アプリケーション ゲートウェイ、ポイント対サイト)。
ネットワーク | VNet ピアリングを使用した仮想ネットワーク (VNet から ARM に移行した後、ピアリング)。詳しくは、「[VNet ピアリング](../virtual-network/virtual-network-peering-overview.md)」を参照してください。
ネットワーク | Traffic Manager プロファイル。

### サポートされていない構成

次の構成は現在サポートされていません。

サービス | 構成 | 推奨
---------- | ------------ | ------------
Resource Manager | クラシック リソース用のロールベースの Access Control (RBAC) | リソースの URI は移行後に変更されるため、移行後に必要になる RBAC ポリシーの更新を計画しておくことをお勧めします。
計算 | VM に関連付けられている複数のサブネット | サブネットのみを参照するように、サブネット構成を更新します。
計算 | 仮想ネットワークに属しているが、明示的なサブネットが割り当てられていない仮想マシン | 必要に応じて VM を削除することができます。
計算 | アラートの自動スケール ポリシーが適用されている仮想マシン | 移行を実行すると、これらの設定は削除されます。したがって、移行を行う前に環境を評価することを強くお勧めします。移行の完了後に、アラート設定を再構成することもできます。
計算 | XML VM 拡張機能 (Visual Studio デバッガー、Web デプロイ、およびリモート デバッグ) | これはサポートされていません。仮想マシンからこれらの拡張機能を削除して、移行を続行することをお勧めします。
計算 | Premium storage を使用したブート診断 | VM のブート診断機能を無効にしてから移行を続行してください。移行が完了した後に、Resource Manager スタックでブート診断を再び有効にできます。さらに、スクリーン ショットとシリアル ログに使用されている BLOB を削除する必要があるため、これらの BLOB に対して課金されることはなくなります。
計算 | Web/worker ロールを含む Cloud Services | 現在これはサポートされていません。
ネットワーク | 仮想マシンと Web/worker ロールを含む仮想ネットワーク | 現在これはサポートされていません。
Azure App Service | App Service 環境を含む仮想ネットワーク | 現在これはサポートされていません。
Azure HDInsight | HDInsight サービスを含む仮想ネットワーク | 現在これはサポートされていません。
Microsoft Dynamics Lifecycle Services | Dynamics Lifecycle Services によって管理される仮想マシンを含む仮想ネットワーク | 現在これはサポートされていません。

## 移行エクスペリエンス

移行エクスペリエンスを開始する前に、以下をお勧めします。

- 移行するリソースで、サポートされていない機能や構成が使用されていないことを確認します。通常プラットフォームでは、これらの問題が検出され、エラーが生成されます。
- 仮想ネットワークにない VM は、準備操作の一環として停止され、割り当てが解除されます。パブリック IP アドレスが失われないようにする場合は、準備操作をトリガーする前に IP アドレスの予約を検討してください。ただし、仮想ネットワーク内にある VM は停止されず、割り当ては解除されません。
- 移行中に発生する可能性のある予期しないエラーに備え、営業時間外での移行を計画します。
- 準備手順の完了後に検証しやすいように、PowerShell、コマンド ライン インターフェイス (CLI) コマンド、または REST API を使用して、VM の現在の構成をダウンロードします。
- 移行を開始する前に、Resource Manager デプロイメント モデルを処理する自動/運用スクリプトを更新します。必要に応じて、リソースの準備ができた状態で GET 操作を実行できます。
- クラシック IaaS リソースで構成されている RBAC ポリシーを評価し、移行が完了したら計画します。

移行のワークフローを次に示します

![移行のワークフローを示すスクリーンショット](./media/virtual-machines-windows-migration-classic-resource-manager/migration-workflow.png)

>[AZURE.NOTE] 次のセクションのすべての操作がべき等です。サポートされていない機能や構成エラー以外の問題が発生した場合は、準備、中止、またはコミット操作を再試行することをお勧めします。Azure Platform では、操作が再試行されます。

### 検証

検証操作は、移行プロセスの最初の手順です。この手順の目標は、移行対象のリソースのデータをバックグラウンドで分析し、リソースを移行できるかどうかを示すコード (成功/失敗) を返すことです。

移行を検証する仮想ネットワークまたはホストされるサービス (仮想ネットワークでない場合) を選択します。

* リソースを移行できない場合は、移行がサポートされていない理由がすべて Azure Platform でリストされます。

### 準備

準備操作は、移行プロセスの 2 番目の手順です。この手順の目的は、クラシックから Resource Manager リソースへの IaaS リソースの変換をシミュレートし、これを並行して示し、視覚化することです。

移行を準備する仮想ネットワークまたはホストされるサービス (仮想ネットワークでない場合) を選択します。

* リソースを移行できない場合は、移行プロセスが停止され、準備操作が失敗した理由がリストされます。
* リソースを移行できる場合は、最初に、移行対象のリソースに対する管理プレーン操作がロックされます。たとえば、移行中、VM にデータ ディスクを追加することはできません。

次に、リソースを移行するために、クラシックから Resource Manager へのメタデータの移行が Azure Platform で開始されます。

準備操作が完了すると、クラシックと Resource Manager の両方でリソースを視覚化するためのオプションが表示されます。クラシック デプロイメント モデルのすべてのクラウド サービスに対して、`cloud-service-name>-migrated` というパターンのリソース グループ名が Azure Platform で作成されます。

>[AZURE.NOTE] 従来の仮想ネットワークにない仮想マシンは、この移行フェーズで "停止済みかつ割り当て解除済み" になります。

### チェック (手動またはスクリプト)

このチェック手順では、必要に応じて、移行が正しく行われていることを検証するために、前の手順でダウンロードした構成を使用できます。また、ポータルにサインインし、プロパティとリソースをスポット チェックして、メタデータが正しく移行されていることを検証することもできます。

仮想ネットワークを移行する場合、仮想マシンの構成のほとんどが再開されません。これらの VM のアプリケーションの場合、アプリケーションがまだ稼働していることを検証できます。

監視/オートメーションおよび操作スクリプトをテストすることで、VM が期待どおりに機能しているか、および更新したスクリプトが正しく機能するかを確認できます。リソースが準備されている状態の場合は、GET 操作のみがサポートされます。

移行をコミットしなければならない期間は設定されません。この状態で必要なだけ時間をかけることができます。ただし、中止またはコミットするまで、これらのリソースに対して管理プレーンがロックされることに注意してください。

問題がある場合は、いつでも移行を中止し、クラシック デプロイメント モデルに戻ることができます。戻ると、Azure Platform でリソースに対する管理プレーン操作が開始されるため、クラシック デプロイメント モデルでその VM に対する通常の操作を再開することができます。

### 中止

中止は省略可能な手順です。この手順により、クラシック デプロイメント モデルへの変更を元に戻し、移行を中止できます。

>[AZURE.NOTE] コミット操作をトリガーしたら、この操作は実行できません。

### コミット

検証が完了したら、移行をコミットできます。リソースは Resource Manager デプロイメント モデルでのみ使用できるようになります。クラシックには表示されません。つまり、移行されたリソースは新しいポータルでのみ管理できます。

>[AZURE.NOTE] これはべき等操作です。失敗した場合は、操作を再試行することをお勧めします。失敗が続く場合は、サポート チケットを作成するか、[VM フォーラム](https://social.msdn.microsoft.com/Forums/azure/ja-JP/home?forum=WAVirtualMachinesforWindows)でフォーラム投稿を作成し、ClassicIaaSMigration タグを付けてください。

## よく寄せられる質問

**この移行計画は Azure 仮想マシン上で実行されている既存のサービスやアプリケーションに影響しますか?**

いいえ。VM (クラシック) は、一般公開で完全にサポートされるサービスです。引き続きこうしたリソースを利用して、Microsoft Azure のフットプリントを拡大できます。

**近日中に移行する予定がない場合、VM はどうなりますか?**

既存のクラシック API やリソース モデルが廃止されることはありません。Resource Manager デプロイメント モデルで使用できる高度な機能により、移行が簡単になります。Resource Manager の IaaS に含まれる[機能強化をいくつか](virtual-machines-windows-compare-deployment-models.md)確認することを強くお勧めします。

**既存のツールにとって、この移行計画はどのような意味がありますか?**

Resource Manager デプロイメント モデルへのツールの更新は、移行計画で考慮する必要がある最も重要な変更の 1 つです。

**管理プレーンのダウンタイムはどれくらいですか?**

移行対象のリソースの数によって異なります。小規模なデプロイ (VM が数十台) の場合、移行に要する時間は 1 時間未満です。大規模なデプロイ (VM が数百台) の場合は、移行に数時間かかることがあります。

**Resource Manager で移行対象のリソースがコミットされた後で、ロールバックすることはできますか?**

リソースが準備完了状態の場合は、移行を中止できます。リソースがコミット操作によって正常に移行されたら、ロールバックできません。

**コミット操作が失敗した場合、移行をロールバックすることはできますか?**

コミット操作が失敗した場合は、移行を中止できません。コミット操作を含むすべての移行操作がべき等です。したがって、しばらくしてから操作を再試行することをお勧めします。エラーが続く場合は、サポート チケットを作成するか、[VM フォーラム](https://social.msdn.microsoft.com/Forums/azure/ja-JP/home?forum=WAVirtualMachinesforWindows)でフォーラム投稿を作成し、ClassicIaaSMigration タグを付けてください。

**Resource Manager で IaaS を使用する必要がある場合、別の Express Route 回線を購入する必要はありますか?**

いいえ。最近、[クラシックおよび Resource Manager での ExpressRoute 回線の共存](../expressroute/expressroute-howto-coexist-resource-manager.md)が可能になりました。ExpressRoute 回線が既にある場合は、新しいものを購入する必要はありません。

**クラシック IaaS リソース用にロールベースの Access Control ポリシーを構成した場合はどうすればよいですか?**

移行中に、リソースはクラシックから Resource Manager に変換されます。したがって、移行後に必要になる RBAC ポリシーの更新を計画しておくことをお勧めします。

**現在、Azure Site Recovery または Azure Backup サービスを使用している場合はどうすればよいですか?**

Resource Manager での VM の Azure Site Recovery および Backup サポートは最近追加されました。また、Resource Manager への VM の移行をサポートできるように取り組んでいます。現時点で、こうした機能を使用する場合には、移行を実行しないことをお勧めします。

**サブスクリプションまたはリソースを検証し、移行が可能かどうかを確認できますか?**

はい。プラットフォームでサポートされる移行オプションでは、移行準備の最初の手順として、リソースを移行できるかどうかを検証します。検証操作が失敗した場合、移行を完了できないすべての理由についてメッセージを受信します。

**IaaS リソースの移行準備中にクォータ エラーが発生した場合はどうなりますか?**

移行を中止することをお勧めします。その後、VM を移行するリージョンのクォータを増やすサポート要求を記録します。クォータ要求が承認されたら、移行手順を再開できます。

**問題はどのようにレポートすればよいですか?**

移行の問題や質問については、ClassicIaaSMigration というキーワードを付けて、[VM フォーラム](https://social.msdn.microsoft.com/Forums/azure/ja-JP/home?forum=WAVirtualMachinesforWindows)に投稿してください。質問はすべてのこのフォーラムに投稿することをお勧めします。サポート契約がある場合は、サポート チケットを記録してもかまいません。

**移行中にプラットフォームで選択されたリソース名が気に入らない場合はどうすればよいですか?**

クラシック デプロイメント モデルで明示的に名前を指定したリソースはすべて、移行中は保持されます。場合によっては、新しいリソースが作成されます。たとえば、各 VM に対してネットワーク インターフェイスが作成されます。現時点では、移行中に作成されるこれらの新しいリソース名を制御することはできません。[Azure フィードバック フォーラム](http://feedback.azure.com)で、この機能の投票を記録してください。

**次のようなメッセージを受信しました。"*VM のエージェントの全般的な状態が、準備が整っていないことを示しています。そのため、VM を移行することはできません。VM エージェントから、エージェントの全般的な状態が準備完了として報告されるようにしてください*" または "*VM には、VM から状態が報告されない拡張機能が含まれています。そのため、この VM は移行できません。*"**

このメッセージを受信するのは、VM がインターネットに送信接続されていない場合です。VM エージェントでは、送信接続を使用して、5 分ごとにエージェントの状態を更新するために Azure ストレージ アカウントにアクセスします。

## 次のステップ
これで Resource Manager へのクラシック IaaS リソースの移行について理解できました。次は、リソースの移行を開始します。

- [プラットフォームでサポートされているクラシックから Azure Resource Manager への移行に関する技術的な詳細](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
- [PowerShell を使用してクラシックから Azure Resource Manager へ IaaS リソースを移行する](virtual-machines-windows-ps-migration-classic-resource-manager.md)
- [CLI を使用してクラシックから Azure Resource Manager へ IaaS リソースを移行する](virtual-machines-linux-cli-migration-classic-resource-manager.md)
- [コミュニティ PowerShell スクリプトを使用して Azure Resource Manager にクラシック仮想マシンを複製する](virtual-machines-windows-migration-scripts.md)

<!---HONumber=AcomDC_0824_2016-->
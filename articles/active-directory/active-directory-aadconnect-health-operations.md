<properties
	pageTitle="Azure AD Connect Health の操作。"
	description="この記事では、Azure AD Connect Health のデプロイ後に実行できる追加操作について説明しています。"
	services="active-directory"
	documentationCenter=""
	authors="karavar"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/14/2016"
	ms.author="vakarand"/>

# Azure AD Connect Health の操作

以下のトピックでは、Azure AD Connect Health を使って実行できる各種の操作について説明します。

## 電子メール通知の有効化
ID インフラストラクチャの状態に問題があることを示すアラートが生成されたときに電子メール通知を送信するように Azure AD Connect Health サービスを構成することができます。アラートが生成されたときのほか、解決済みとしてマークされたときにも通知が送信されます。電子メール通知を構成するには以下の手順に従います。

![Azure AD Connect Health の電子メール通知の検出](./media/active-directory-aadconnect-health/email_noti_discover.png)

>[AZURE.NOTE] 既定では、電子メール通知は無効です。


### Azure AD Connect Health の電子メール通知を有効にするには

1. 電子メール通知の送信元となるサービスの [アラート] ブレードを開きます。
2. 操作バーの [通知設定] ボタンをクリックします。
3. 電子メール通知スイッチをオンに設定します。
4. すべてのグローバル管理者が電子メール通知を受信するように構成するチェック ボックスをオンにします。
5. それ以外の電子メール アドレスで電子メール通知を受信する必要がある場合は、[追加の電子メール受信者] ボックスで指定できます。このリストから電子メール アドレスを削除するには、対象のエントリを右クリックして [削除] を選択します。
6. 変更を確定するには、[保存] をクリックします。[保存] を選択した後にのみ、すべての変更が有効になります。

## サーバーまたはサービス インスタンスを削除する

### Azure AD Connect Health サービスからのサーバーの削除
特定のサーバーを監視対象から除外しなければならない場合があります。Azure AD Connect Health サービスから特定のサーバーを削除するには以下の手順に従ってください。

サーバーを削除する際は次の点に注意してください。

- 削除したサーバーからはデータが一切収集されなくなります。そのサーバーは監視サービスから除外されます。削除されたサーバーについての新しいアラートや監視データ、使用状況分析データは表示できません。
- この操作では、Health エージェントがサーバーからアンインストールされることも削除されることもありません。Health エージェントをアンインストールせずにこの手順を実行した場合、Health エージェントに関連したエラー イベントがそのサーバー上に表示されます。
- この操作では、対象サーバーから既に収集されたデータが削除されません。そのデータは、Microsoft Azure データ保持ポリシーに従って削除されます。
- この操作の実行後、同じサーバーの監視をもう一度開始する必要がある場合、対象サーバーから Health エージェントをいったんアンインストールしてから再度インストールする必要があります。


#### Azure AD Connect Health サービスからサーバーを削除するには

Azure AD Connect Health for AD FS と Azure AD Connect (Sync):

1. [サーバーの一覧] ブレードから、削除するサーバー名を選択して [サーバー] ブレードを開きます。
2. [サーバー] ブレードで、操作バーから [削除] ボタンをクリックします。
3. 確認ボックスにサーバー名を入力して削除操作を確認します。
4. [削除] ボタンをクリックします。

Azure AD Connect Health for AD DS:

1. [ドメイン コントローラー] ダッシュボードを開きます。
2. 削除するドメイン コントローラーを選択します。
3. アクション バーの [Delete Selected (選択項目の削除)] をクリックします。
4. サーバーの削除アクションを確定します。
5. [削除] ボタンをクリックします。

### Azure AD Connect Health サービスからのサービス インスタンスの削除

特定のサービス インスタンスを削除しなければならない場合があります。Azure AD Connect Health サービスから特定のサービス インスタンスを削除するには以下の手順に従ってください。

サービス インスタンスを削除する際は次の点に注意してください。

- この操作では、現在のサービス インスタンスが監視サービスから削除されます。
- このサービス インスタンスの一部として監視されていた、いずれのサーバーからも、Health エージェントはアンインストールされず、削除されることもありません。Health エージェントをアンインストールせずにこの手順を実行した場合、Health エージェントに関連したエラー イベントがそのサーバー上に表示されます。
- このサービス インスタンスからのデータはすべて、Microsoft Azure データ保持ポリシーに従って削除されます。
- この操作の実行後、そのサービスの監視を開始する必要がある場合は、監視対象となる全サーバー上の Health エージェントをいったんアンインストールしてから再度インストールしてください。この操作の実行後、同じサーバーの監視をもう一度開始する必要がある場合、対象サーバーから Health エージェントをいったんアンインストールしてから再度インストールする必要があります。


#### Azure AD Connect Health サービスからサービス インスタンスを削除するには

1. [サービスの一覧] ブレードから、削除するサービスの ID (ファーム名) を選択して、[サービス] ブレードを開きます。
2. [サーバー] ブレードで、操作バーから [削除] ボタンをクリックします。
3. 確認ボックスにサービス名を入力して確認します (例: sts.contoso.com)。
4. [削除] ボタンをクリックします。<br><br>


[//]: # "RBAC セクションの開始"
## ロール ベースの Access Control を使用してアクセスを管理する
### 概要
Azure AD Connect Health の[ロール ベースの Access Control](role-based-access-control-configure.md) では、グローバル管理者以外のユーザー/グループに Azure AD Connect Health サービスへのアクセスを提供します。これは、目的のユーザー/グループにロールを割り当て、ディレクトリ内のグローバル管理者を制限するメカニズムを用意することで実現します。

#### ロール
Azure AD Connect Health では、次の組み込みのロールをサポートします。

| ロール | アクセス許可 |
| ----------- | ---------- |
| Owner | 所有者は、***アクセスの管理*** (例: ユーザー/グループへのロールの割り当て)、***ポータルからのすべての情報の表示*** (例: アラートの表示)、Azure AD Connect Health 内の***設定の変更*** (例: 電子メール通知) を実行できます。<br>このロールは、既定で Azure AD のグローバル管理者に割り当てられ、これを変更することはできません。 |
|Contributor| 共同作成者は、ポータルからの***すべての情報の表示*** (例: アラートの表示) と Azure AD Connect Health 内の***設定の変更*** (例: 電子メール通知) を実行できます。|
|Reader| 閲覧者は、ポータルから Azure AD Connect Health 内の***すべての情報の表示*** (例: アラートの表示) を実行できます。|

上記以外のすべてのロール ("ユーザー アクセス管理者" や "DevTest Labs ユーザー" など) は、ポータル エクスペリエンスで利用できる場合でも、Azure AD Connect Health 内のアクセスに影響することはありません。

#### アクセス スコープ

Azure AD Connect では、2 つのレベルのアクセス管理をサポートします。

- ***すべてのサービス インスタンス***: これはほとんどのユーザー用の推奨パスであり、Azure AD Connect Health によって監視されるすべてのロールの種類のすべてのサービス インスタンス (例:ADFS ファーム) アクセスを制御します。

- ***サービス インスタンス***: 場合によっては、ロールの種類またはサービス インスタンスに基づいてアクセスを分離する必要があります。この場合は、サービス インスタンス レベルでアクセスを管理できます。

アクセス許可は、エンド ユーザーがディレクトリ レベルまたはサービス インスタンス レベルのいずれかでアクセス権がある場合に与えられます。


### ユーザーまたはグループに Azure AD Connect Health へのアクセスを許可する方法
#### 手順 1: 適切なアクセス スコープを選択する
Azure AD Connect Health 内で*すべてのサービス インスタンス* レベルのユーザーにアクセスを許可するには、Azure AD Connect Health でメイン ブレードを開きます。<br>
#### 手順 2: ユーザー/グループを追加し、ロールを割り当てる
1. [構成] セクションから [ユーザー] 部分をクリックします。<br>
![Azure AD Connect Health RBAC のメイン ブレード](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. [追加] を選択します。
3. [所有者] などの [ロール] を選択します。<br> 
![Azure AD Connect Health RBAC のユーザーの追加](./media/active-directory-aadconnect-health/RBAC_add.png)
4. 対象となるユーザー/グループの名前または識別子を入力します。1 つまたは複数のユーザー/グループを同時に選択できます。[選択] をクリックします。![Azure AD Connect Health RBAC のユーザーの選択](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. [OK] を選択します。<br>

6. ロールの割り当てが完了すると、ユーザー/グループが一覧に表示されます。<br>
![Azure AD Connect Health RBAC のユーザー一覧](./media/active-directory-aadconnect-health/RBAC_user_list.png)

これらの手順によって、一覧に表示されているユーザーとグループは、割り当てられたロールに従ってアクセスを許可されます。
>[AZURE.NOTE]
- グローバル管理者は常にすべての操作にフル アクセスできますが、グローバル管理者のアカウントは上記のリストには表示されません。
- "ユーザーの招待" 機能は、Azure AD Connect Health ではサポートされません。

#### 手順 3: ブレードの場所をユーザーまたはグループと共有する
1. アクセス許可を割り当てた後、ユーザーは [http://aka.ms/aadconnecthealth](http://aka.ms/aadconnecthealth) に移動することで Azure AD Connect Health にアクセスできます。
2. ブレードで [ダッシュボードにピン留め] をクリックするだけで、ブレードまたは別のパートをダッシュボードにピン留めできます。<br> 
![Azure AD Connect Health RBAC のピン ブレード](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)


>[AZURE.NOTE] "閲覧者" ロールを割り当てられたユーザーは、Azure Marketplace から Azure AD Connect Health の拡張機能を取得する "作成" 操作を実行することはできません。ただし、このユーザーは、上記のリンクに移動してブレードにアクセスできます。引き続き使用するために、ブレードをダッシュボードにピン留めできます。

### ユーザー/グループを削除する
Azure AD Connect Health Role の [ロール ベースの Access Control] 部分に追加したユーザーまたはグループは、右クリックして [削除] を選択することで削除できます。<br> 
![Azure AD Connect Health RBAC のユーザーの削除](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # "RBAC セクションの終了"

## 関連リンク

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health エージェントのインストール](active-directory-aadconnect-health-agent-install.md)
* [AD FS での Azure AD Connect Health の使用](active-directory-aadconnect-health-adfs.md)
* [Azure AD Connect Health for Sync の使用](active-directory-aadconnect-health-sync.md)
* [AD DS での Azure AD Connect Health の使用](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health の FAQ](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Health のバージョンの履歴](active-directory-aadconnect-health-version-history.md)

<!---HONumber=AcomDC_0720_2016-->
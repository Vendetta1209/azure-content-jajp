<properties
   pageTitle="Azure Security Center でのセキュリティ ポリシーの設定 | Microsoft Azure"
   description="このドキュメントは、Azure セキュリティ センターでのセキュリティ ポリシーを構成する場合に役立ちます。"
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="yurid"/>

# Azure セキュリティ センターでのセキュリティ ポリシーの設定
このドキュメントでは、Security Center でセキュリティ ポリシーを構成するうえで必要な手順について詳しく説明します。

## セキュリティ ポリシーとは
セキュリティ ポリシーは、指定されたサブスクリプションまたはリソース グループ内のリソースに推奨されるコントロールのセットを定義します。Security Center では、セキュリティに関する会社のニーズ、および各サブスクリプションでのアプリケーションの種類やデータの機密度に合わせて、Azure サブスクリプションまたはリソース グループのポリシーを定義できます。

たとえば、開発やテストに使用されるリソースは、運用アプリケーションで使用されるリソースとは異なるセキュリティ要件を持つ場合があります。同様に、PII (個人情報) のような規制されたデータを持つアプリケーションには、より高いレベルのセキュリティが必要です。Azure Security Center で有効になっているセキュリティ ポリシーは、セキュリティに関する推奨事項と監視を推進し、潜在的な脆弱性を識別して脅威を軽減します。どのオプションがより適しているかを判断する方法の詳細については、「[Azure Security Center 計画および運用ガイド](security-center-planning-and-operations-guide.md)」を参照してください。

## サブスクリプションのセキュリティ ポリシーの設定

セキュリティ ポリシーは、サブスクリプションまたはリソース グループごとに構成できます。セキュリティ ポリシーを変更するには、そのサブスクリプションの所有者または共同作業者である必要があります。Azure ポータルにアクセスし、次の手順に従って Security Center でセキュリティ ポリシーを構成します。

1. Security Center のダッシュボードで **[ポリシー]** タイルをクリックします。

2. **[セキュリティ ポリシー - サブスクリプションまたはリソース グループごとにポリシーを定義する]** ブレードが右側に開いたら、セキュリティ ポリシーを有効にするサブスクリプションを選択します。サブスクリプション全体ではなく、リソース グループのセキュリティ ポリシーを有効にする場合は、リソース グループのセキュリティ ポリシーの設定について説明する次のセクションまで下へスクロールしてください。

    ![Defining policy](./media/security-center-policies/security-center-policies-fig1-ga.png)

3. そのサブスクリプションの **[セキュリティ ポリシー]** ブレードが開き、次に示すようなオプションのセットが表示されます。

    ![データ収集の有効化](./media/security-center-policies/security-center-policies-fig2-ga.png)

	このブレードで使用できるオプションは次のとおりです。
	- **防止ポリシー**: このオプションを使用すると、サブスクリプションまたはリソース グループごとにポリシーを構成できます。
	- **電子メールの通知**: 電子メール通知は、最初に警告が発生したとき、および重大度が高い警告に対してのみ送信されます。また、電子メールの設定は、サブスクリプション ポリシーに対してのみ構成できます。電子メールの通知を構成する方法の詳細については、「[Azure Security Center でセキュリティ連絡先の詳細情報を指定する](security-center-provide-security-contact-details.md)」を参照してください。
	- **価格レベル**: [価格レベル] の選択からアップグレードするには、このオプションを使用します。価格オプションの詳細については、[Security Center のページ](https://azure.microsoft.com/pricing/details/security-center/)を参照してください。

	
4.  **[仮想マシンからデータを収集]** オプションが **[オン]** になっていることを確認します。このオプションは、既存および新規のリソースに対する自動ログ収集を有効化します。

    >[AZURE.NOTE] サブスクリプションごとにデータ収集を有効にすることをお勧めします。これにより、既存および新規のすべての VM でセキュリティの監視を利用できるようになります。データ収集を有効にすると、監視エージェントがインストールされます。この場所からのデータ収集を今すぐ有効にしない場合は、後からヘルス ビューと推奨事項のビューから有効にすることができます。また、特定のサブスクリプションのみ、または選択した VM でデータ収集を有効にすることもできます。サポートされる VM に関する詳細は、「[Azure セキュリティ センターに関する FAQ](security-center-faq.md)」を参照してください。

5. ストレージ アカウントをまだ構成していない場合は、**セキュリティ ポリシー**を開いたときに、次に示すような警告が表示されることがあります。

    ![Storage の選択](./media/security-center-policies/security-center-policies-fig2.png)

6. この警告が表示された場合は、このオプションをクリックして、次に示すようにリージョンを選択します。

    ![Storage の選択](./media/security-center-policies/security-center-policies-fig3-ga.png)

7. 仮想マシンが実行されている各リージョンに対し、これらの仮想マシンから収集されたデータが格納されるストレージ アカウントを選択します。これにより、プライバシーとデータ主権目的で、同じ地域にデータを保持することが容易になります。使用するリージョンを決めたら、そのリージョンを選択し、ストレージ アカウントを選択します。

8. **[ストレージ アカウントの選択]** ブレードで **[OK]** をクリックします。

    > [AZURE.NOTE] 必要に応じて、さまざまなリージョンの仮想マシンからのデータを 1 つの中央のストレージ アカウントに集計することができます。詳細については、「[Azure Security Center のよく寄せられる質問 (FAQ)](security-center-faq.md)」を参照してください。

9. **[セキュリティ ポリシー]** ブレードで **[オン]** をクリックして、このサブスクリプションで使用するセキュリティの推奨事項を有効にします。**[防止ポリシー]** オプションをクリックします。次のように、**[セキュリティ ポリシー]** ブレードが開きます。

	![Selecting the security policies](./media/security-center-policies/security-center-policies-fig4-ga.png)

次の表を参照して、各オプションの機能を確認してください。

| ポリシー | 状態がオンの場合 |
|----- |-----|
| システムの更新 | 毎日、その仮想マシンに構成されているサービスに応じて、利用可能なセキュリティ更新プログラムと重要な更新プログラムの一覧を Windows Update または WSUS から取得し、不足している更新プログラムがあれば適用することを推奨します。ディストリビューションで提供されるパッケージ管理システムを使用して Linux システム内で最新の更新プログラムを確認し、どのパッケージに適用可能な更新プログラムがあるかを判断します。また、[Cloud Services](./cloud-services/cloud-services-how-to-configure.md) 仮想マシンのセキュリティ更新プログラムと重要な更新プログラムについても確認します。 |
| OS の脆弱性 | 毎日、仮想マシンが攻撃を受けやすくなる可能性がある OS の構成を分析して、これらの脆弱性に対処するための構成の変更を推奨します。監視対象のこの特定の構成に関する詳細は、[推奨される基準の一覧](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335)を参照してください。 |
| エンドポイント保護 | ウイルス、スパイウェア、およびその他の悪意のあるソフトウェアを特定して削除するため、すべての Windows 仮想マシンにエンドポイント保護をプロビジョニングすることをお勧めします。|
| ディスク暗号化 | 静止データの保護を強化するために、ディスク暗号化はすべての仮想マシンで有効にすることをお勧めします。 
| ネットワーク セキュリティ グループ | VM のパブリック エンドポイントへの受信および送信トラフィックを制御するように、[ネットワーク セキュリティ グループ](../virtual-network/virtual-networks-nsg.md) (NSG) を構成することを推奨します。サブネットに構成された NSG は、他に指定しない限り、すべての仮想マシンのネットワーク インターフェイスによって継承されます。このオプションは、NSG が構成されていることをチェックすることに加え、受信セキュリティ ルールを評価して、受信トラフィックを許可するルールがあるかどうかを特定します。 |
| Web アプリケーション ファイアウォール | 次の場合には、仮想マシン上に Web アプリケーション ファイアウォールをプロビジョニングすることをお勧めします。[インスタンスレベル パブリック IP](../virtual-network/virtual-networks-instance-level-public-ip.md) (ILPIP) が使用され、関連付けられている NSG 受信セキュリティ規則がポート 80 および 443 へのアクセスを許可するように構成されている場合。負荷分散された IP (VIP) が使用され、関連付けられている負荷分散と受信 NAT ルールがポート 80 および 443 へのアクセスを許可するように構成されている場合 (詳細については、「[Azure リソース マネージャーによる Load Balancer のサポート](../load-balancer/load-balancer-arm.md)」を参照)。 |
| Next Generation Firewall (次世代ファイアウォール) | これを利用すると、Azure に組み込まれているネットワーク セキュリティ グループの適用範囲外までネットワーク保護が拡張されます。次世代ファイアウォールの利用が推奨されるデプロイが Security Center によって検出されると、仮想アプライアンスをプロビジョニングできるようになります。 |
| SQL 監査 | コンプライアンス、高度な検出と調査のため、Azure SQL Server とデータベースへのアクセスの監査を有効にすることをお勧めします。 |
| SQL の透過的なデータ暗号化 | データがセキュリティ侵害を受けた場合でも読み取れないようするため、Azure SQL データベースと関連付けられたバックアップおよびトランザクション ログファイルに対し、REST での暗号化を有効にすることをお勧めします。 |
	
すべてのオプションの構成が完了したら、推奨事項が表示されている **[セキュリティ ポリシー]** ブレードで **[OK]** をクリックし、初期設定が表示されている **[セキュリティ ポリシー]** ブレードで **[保存]** をクリックします。

## リソース グループのセキュリティ ポリシーの設定

リソース グループごとにセキュリティ ポリシーを構成する場合、その手順はサブスクリプションのセキュリティ ポリシーを設定する手順と似ています。主な違いは、サブスクリプション名を展開し、一意のセキュリティ ポリシーを構成するリソース グループを選択する必要があることです。

![Resource group selection](./media/security-center-policies/security-center-policies-fig5-ga.png)

リソース グループを選択すると、**[Security policy (セキュリティ ポリシー)]** ブレードが開きます。既定では、**[継承]** オプションが有効になっています。つまり、このリソース グループのすべてのセキュリティ ポリシーは、サブスクリプション レベルから継承されます。リソース グループごとにカスタム セキュリティ ポリシーを設定する場合は、この構成を変更できます。その場合は、**[Unique (一意)]** を選択し、**[Prevention policy (防止ポリシー)]** オプションの下で変更を加える必要があります。

![Security policy per resource group](./media/security-center-policies/security-center-policies-fig6-ga.png)

> [AZURE.NOTE] サブスクリプション レベルのポリシーとリソース グループ レベルのポリシーが競合する場合は、リソース レベルのポリシーが優先されます。


## 関連項目

このドキュメントでは、Azure セキュリティ センターでのセキュリティ ポリシーの構成方法について説明しました。Azure セキュリティ センターの詳細については、次を参照してください。

- 「[Azure Security Center 計画および運用ガイド](security-center-planning-and-operations-guide.md)」 -- Azure Security Center を導入するための設計上の考慮事項を計画および理解する方法について説明します。
- 「[Azure Security Center でのセキュリティ ヘルスの監視](security-center-monitoring.md)」 -- Azure リソースの正常性を監視する方法について説明しています。
- 「[Azure Security Center でのセキュリティの警告の管理と対応](security-center-managing-and-responding-alerts.md)」 -- セキュリティの警告の管理と対応の方法について説明しています。
- 「[Azure Security Center を使用したパートナー ソリューションの監視](security-center-partner-solutions.md)」 -- パートナー ソリューションの正常性状態を監視する方法について説明しています。
- 「[Azure Security Center のよく寄せられる質問 (FAQ)](security-center-faq.md)」 -- このサービスの使用に関してよく寄せられる質問が記載されています。
- [Azure セキュリティ ブログ](http://blogs.msdn.com/b/azuresecurity/) -- Azure のセキュリティとコンプライアンスについてのブログ記事を確認できます。

<!---HONumber=AcomDC_0831_2016-->
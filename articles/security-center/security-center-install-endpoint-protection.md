<properties
   pageTitle="Azure Security Center で Endpoint Protection をインストールする | Microsoft Azure"
   description="このドキュメントでは、";**Endpoint Protection をインストールします**"; という Azure Security Center の推奨事項を実装する方法について説明します。"
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# Azure Security Center で Endpoint Protection をインストールする

マルウェア対策を有効にしていない場合、Azure Security Center では、マルウェア対策プログラムを Azure 仮想マシン (VM) にプロビジョニングするよう推奨されます。この推奨事項は、Windows VM にのみ適用されます。

> [AZURE.NOTE] このドキュメントでは、サンプルのデプロイを使用してサービスについて紹介します。ステップ バイ ステップ ガイドではありません。

## 推奨事項の実装

1. **[推奨事項]** ブレードで、**[Endpoint Protection をインストールします]** を選択します。![Select Install Endpoint Protection][1]

2. **[Endpoint Protection をインストールします]** ブレードが開き、マルウェア対策が有効になっていない VM の一覧が表示されます。マルウェア対策をインストールする VM を一覧から選択し、**[VM にインストール]** をクリックします。![Select VMs to install antimalware on][2]

3. **[Select Endpoint Protection (Endpoint Protection の選択)]** ブレードが開き、使用するマルウェア対策ソリューションを選択できます。この例では、**[Microsoft マルウェア対策]** を選択します。![Select Endpoint Protection][3]

4. マルウェア対策ソリューションに関する追加情報が表示されます。**[作成]** を選択します。![マルウェア対策ソリューションの作成][4]

5. **[拡張機能の追加]** ブレードで必要な構成設定を入力し、**[OK]** を選択します。構成設定の詳細については、「[マルウェア対策の既定の構成とカスタム構成](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration)」を参照してください。

これで、[Microsoft マルウェア対策](../azure-security-antimalware.md)が、選択した VM でアクティブになりました。

## 関連項目

この記事では、"Endpoint Protection をインストールします" という Security Center の推奨事項を実装する方法について説明しました。 Azure でのマルウェア対策プログラムの有効化について詳しくは、以下を参照してください。

- 「[Cloud Services および Virtual Machines 向け Microsoft マルウェア対策](../azure-security-antimalware.md)」-- Microsoft マルウェア対策をデプロイする方法を説明しています。

セキュリティ センターの詳細については、次を参照してください。

- 「[Azure Security Center でのセキュリティ ポリシーの設定](security-center-policies.md)」-- セキュリティ ポリシーの構成方法について説明しています。
- 「[Azure Security Center でのセキュリティに関する推奨事項の管理](security-center-recommendations.md)」-- 推奨事項に従って Azure リソースを保護する方法について説明しています。
- 「[Azure Security Center でのセキュリティ ヘルスの監視](security-center-monitoring.md)」-- Azure リソースの正常性を監視する方法について説明しています。
- 「[Azure Security Center でのセキュリティの警告の管理と対応](security-center-managing-and-responding-alerts.md)」-- セキュリティの警告の管理と対応の方法について説明しています。
- 「[Azure Security Center を使用したパートナー ソリューションの監視](security-center-partner-solutions.md)」-- パートナー ソリューションの正常性状態を監視する方法について説明しています。
- 「[Azure Security Center のよく寄せられる質問 (FAQ)](security-center-faq.md)」-- このサービスの使用に関してよく寄せられる質問が記載されています。
- [Azure セキュリティ ブログ](http://blogs.msdn.com/b/azuresecurity/) -- Azure のセキュリティとコンプライアンスについてのブログ記事を確認できます。

<!--Image references-->
[1]: ./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]: ./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]: ./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]: ./media/security-center-install-endpoint-protection/create-antimalware-solution.png

<!---HONumber=AcomDC_0817_2016-->
<properties
   pageTitle="PIM へのアクセス権を付与する方法 |Microsoft Azure"
   description="Azure Active Directory Privileged Identity Management 拡張機能でユーザーにロールを追加して、ユーザーが PIM を管理できるようにする方法について説明します。"
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/30/2016"
   ms.author="kgremban"/>

# Azure AD Privileged Identity Management を管理するアクセス権を付与する方法

組織の Azure AD Privileged Identity Management (PIM) を有効にするグローバル管理者は、自動的にロールの割り当てを取得し、PIM にアクセスします。ただし、他のグローバル管理者も含めて、既定でアクセスできるユーザーは他にはいません。PIM へのアクセス権を付与するには、最初のユーザーが他のユーザーに**特権ロール管理者**ロールを割り当てます。この割り当ては、PIM 内から実行する必要があり、PowerShell や他のポータルを使用して変更することはできません。

> [AZURE.NOTE] Azure AD PIM を管理するには、Azure MFA が必要です。Microsoft アカウントは Azure MFA に登録できないため、Microsoft アカウントでサインインしたユーザーは Azure AD PIM にアクセスできません。

1 人のユーザーがロックアウトされたり、アカウントが削除されたりしたときのために、特権ロール管理者ロールを割り当てられたユーザーが常に少なくとも 2 人いるようにしてください。

## PIM を管理するためのアクセス権を別のユーザーに付与する

1. [Azure ポータル](https://portal.azure.com/)にサインインし、ダッシュボードで **Azure AD Privileged Identity Management** アプリを選択します。
2. **[特権ロールの管理]**、**[特権ロール管理者]**、**[追加]**の順に選択します。

    ![特権ロール管理者の追加 - スクリーンショット][1]

4. [管理対象ユーザーの追加] ブレードで、手順 1 は既に完了しています。手順 2、**[ユーザーを選択する]** を選択し、追加するユーザーを検索します。

    ![ユーザーの選択 - スクリーンショット][2]

6. 検索結果からユーザーを選択し、**[実行]** をクリックします。
7. **[OK]** をクリックして選択を保存します。選択したユーザーが特権ロール管理者の一覧に表示されます。

    - 新しいロールを他のユーザーに割り当てるたびに、ユーザーは自動的に対象として設定され、ロールをアクティブ化します。ロールでユーザーを永続的にする場合は、一覧のユーザーをクリックします。ユーザー情報メニューで **[永続的にする]** を選択します。

8. [Azure AD Privileged Identity Management の使用](active-directory-privileged-identity-management-getting-started.md)へのリンクをユーザーに送信します。


## 別のユーザーに付与された、PIM を管理するためのアクセス権を削除する

特権ロール管理者ロールからユーザーを削除する際は、割り当てられているユーザーの人数が削除後も 2 人以上になるように、必ず事前に確認してください。

1. PIM ダッシュボードで、**[特権ロール管理者]** ロールをクリックします。そのロールの現在のユーザー一覧が表示されます。
2. ユーザーの一覧でユーザーをクリックします。
3. **[削除]** をクリックします。確認メッセージが表示されます。
4. **[はい]** をクリックすると、ロールからユーザーが削除されます。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 次のステップ
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png

<!---HONumber=AcomDC_0706_2016-->
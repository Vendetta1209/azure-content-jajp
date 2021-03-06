<properties
	pageTitle="VM を作成するための Azure DevTest Labs のカスタム イメージの管理 | Microsoft Azure"
	description="VHD ファイルまたは Azure DevTest Labs 内の既存の VM からカスタム イメージを作成する方法について説明します。"
	services="devtest-lab,virtual-machines"
	documentationCenter="na"
	authors="tomarcher"
	manager="douge"
	editor=""/>

<tags
	ms.service="devtest-lab"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/24/2016"
	ms.author="tarcher"/>

# VM を作成するための Azure DevTest Labs のカスタム イメージの管理

## Overview
[ラボを作成](devtest-lab-create-lab.md)したら、[そのラボに仮想マシン (VM) を追加](devtest-lab-add-vm-with-artifacts.md)することができます。VM を作成する場合は、*ベース*として*カスタム イメージ*または*Marketplace イメージ*を指定します。この記事では、後でカスタム イメージから VM を作成できるように、[VHD からカスタム イメージを作成する](#create-a-custom-image-from-a-vhd)方法について説明します。さらに、後で迅速に VM を作成できるように、[VM からカスタム イメージを作成する](#create-a-custom-image-from-a-vm)こともできます。

## VHD ファイルからカスタム イメージを作成する

このセクションでは、VHD ファイルからカスタム イメージを作成する方法を説明します。なお、このセクションのすべての手順を実行するには、有効な VHD ファイルへのアクセス権が必要です。


1. [Azure ポータル](http://go.microsoft.com/fwlink/p/?LinkID=525040)にサインインします。

1. **[参照]** を選択し、一覧の **[DevTest Labs]** を選択します。

1. ラボの一覧で目的のラボを選択します。

1. 選択したラボの **[設定]** ブレードが表示されます。

1. ラボの **[設定]** ブレードで、**[カスタム イメージ]** を選択します。

    ![カスタム イメージ オプション](./media/devtest-lab-create-template/lab-settings-custom-images.png)

1. **[カスタム イメージ]** ブレードで、**[+ カスタム イメージ]** を選択します。

    ![カスタム イメージの追加](./media/devtest-lab-create-template/add-custom-image.png)

1. カスタム イメージの名前を入力します。この名前は、新しい VM を作成するときにベース イメージの一覧に表示されます。

1. カスタム イメージの説明を入力します。この説明は、新しい VM を作成するときにベース イメージの一覧に表示されます。

1. **[VHD ファイル]** を選択します。

1. 一覧表示されていない VHD ファイルにアクセスする場合は、「[VHD ファイルのアップロード](#upload-a-vhd-file)」セクションの手順に従って VHD ファイルを追加し、完了したら、ここに戻ります。

1. 目的の VHD ファイルを選択します。

1. **[OK]** を選択して、**[VHD ファイル]** ブレードを閉じます。

1. **[OS 構成]** を選択します。

1. **[OS 構成]** タブで、**[Windows]** または **[Linux]** を選択します。

1. **[Windows]** を選択した場合は、チェック ボックスを使用して、コンピューターで *Sysprep* が実行されたかどうかを指定します。

1. **[OK]** を選択して、**[OS 構成]** ブレードを閉じます。

1. **[OK]** を選択して、カスタム イメージを作成します。

1. 「[次のステップ](#next-steps)」セクションを参照してください。

###VHD ファイルのアップロード

新しいカスタム イメージを追加するには、VHD ファイルにアクセスする必要があります。

1. **[VHD ファイル]** ブレードで、**[Upload a VHD file using PowerShell (PowerShell を使用して VHD ファイルをアップロード)]** を選択します。

    ![イメージのアップロード](./media/devtest-lab-create-template/upload-image-using-psh.png)

1. 次のブレードには、VHD ファイルを Azure サブスクリプションにアップロードする PowerShell スクリプトの変更と実行の手順が表示されます。**注:** VHD ファイルのサイズと接続速度によっては、このプロセスに時間がかかる可能性があります。

## VM からのカスタム イメージの作成
既に構成されている VM がある場合は、その VM からカスタム イメージを作成し、後でそのカスタム イメージを使用して、他と同じ VM を作成することができます。次の手順では、VM からカスタム イメージを作成する方法を説明します。

1. [Azure ポータル](http://go.microsoft.com/fwlink/p/?LinkID=525040)にサインインします。

1. **[参照]** を選択し、一覧の **[DevTest Labs]** を選択します。

1. ラボの一覧で目的のラボを選択します。

1. ラボの **[概要]** ブレードで、カスタム イメージの作成元にする VM を選択します。

1. VM のブレードで、**[Create custom image (VHD) (カスタム イメージ を作成する (VHD))]** を選択します。

	![カスタム イメージのメニュー項目の作成](./media/devtest-lab-create-template/create-custom-image.png)

1. **[Create image (イメージの作成)]** ブレードで、カスタム イメージの名前と説明を入力します。この情報は、VM を作成する際にベースの一覧に表示されます。

	![カスタム イメージの作成ブレード](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. sysprep を VM で実行したかどうかを選択します。sysprep を VM で実行していない場合は、カスタム イメージから VM を作成するときに sysprep を実行するかどうかを指定します。

1. 完了したら **[OK]** を選択して、カスタム イメージを作成します。

## 関連するブログ記事

- [Custom images or formulas? (カスタム イメージか数式か?)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Copying Custom Images between Azure DevTest Labs (Azure DevTest Labs 間でのカスタム イメージのコピー)](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##次のステップ

VM を作成するときに使用するカスタム イメージを追加したら、次は [VM をラボに追加](./devtest-lab-add-vm-with-artifacts.md)します。

<!---HONumber=AcomDC_0824_2016-->
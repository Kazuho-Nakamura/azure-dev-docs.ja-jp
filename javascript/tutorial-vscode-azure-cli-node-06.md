---
title: アプリ コードに変更を加えて Azure に再デプロイする
description: チュートリアル パート 6、変更を加えて再デプロイする
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: f656e7414d6b7b6cecc28d69f108bfe92eb82894
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686126"
---
# <a name="make-changes-and-redeploy"></a>変更を加えて再デプロイする

[前の手順: ログをストリーミングする](tutorial-vscode-azure-cli-node-05.md)

この手順では、アプリ コードに変更を加えてローカル Git リポジトリにコミットした後、Azure にプッシュしてサイトを再デプロイします。

1. `myExpressApp` フォルダーで *views/index.pug* ファイルを開き、5 行目のメッセージを `p Welcome to Azure!` に変更します。

    ![index.pug ファイルの編集](media/azure-cli/editpugfile.png)

1. ファイルを保存します。

1. ターミナルまたはコマンド プロンプトで、次のコマンドを実行して変更を Git にコミットします。

    ```bash
    git commit -a -m "Edited message"
    ```

1. 先ほど作成した Azure という名前の Git リモートに変更をプッシュします。

    ```bash
    git push azure master
    ```

1. App Service は既に Git リポジトリに接続されているため、コマンドからの出力は、変更が Azure に自動的に発行されていることを示しています。 

    ```output
    Enumerating objects: 7, done.
    Counting objects: 100% (7/7), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (4/4), done.
    Writing objects: 100% (4/4), 405 bytes | 202.00 KiB/s, done.
    Total 4 (delta 2), reused 0 (delta 0)
    remote: Updating branch 'master'.
    remote: Updating submodules.
    remote: Preparing deployment for commit id '9fd89a25b6'.
    remote: Generating deployment script.
    remote: Running deployment command...
    remote: Handling node.js deployment.
    remote: Creating app_offline.htm
    remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
    remote: Copying file: 'views\index.pug'
    remote: Deleting app_offline.htm
    remote: Using start-up script bin/www from package.json.
    remote: Generated web.config.
    remote: The package.json file does not specify node.js engine version constraints.
    remote: The node.js application will run with the default node.js version 6.9.5.
    remote: Selected npm version 3.10.10
    remote: ..
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git
       a98bad8..9fd89a2  master -> master
    ```

1. ブラウザーでアプリを更新して、次の変更を確認します。

    ![発行された変更がブラウザーに表示されている](media/azure-cli/remote-app-changes.png)

> [!div class="nextstepaction"]
> [変更を確認しました](tutorial-vscode-azure-cli-node-07.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=publishing-changes)
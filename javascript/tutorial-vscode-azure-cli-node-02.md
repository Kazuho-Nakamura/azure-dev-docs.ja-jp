---
title: Node.js アプリを作成し、Azure CLI を使用して Azure にデプロイする
description: チュートリアル パート 2、アプリ コードを作成する。
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: dba089c58cf6413263348b7bdeb975852c2e6a2f
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467183"
---
# <a name="create-the-app-code-using-express"></a>Express を使用してアプリ コードを作成する

[前の手順:概要と前提条件](tutorial-vscode-azure-cli-node-01.md)

この手順では、[Express Generator](https://expressjs.com/en/starter/generator.html) を使用して [Express](https://www.expressjs.com) で簡単な Nodo.js アプリを作成します。

1. 次のコマンドを使用して Express Generator を実行し、"myExpressApp" という新しい Express アプリをスキャフォールディングします。 (`--view pug --git` パラメーターは、[pug](https://pugjs.org/api/getting-started.html) テンプレート エンジン (旧称 Jade) を使用することと、 *.gitignore* ファイルを作成することをジェネレーターに指示します。)

    ```bash
    npx express-generator myExpressApp --view pug –git
    ```

1. 次のコマンドを実行して、アプリ フォルダーに移動し、アプリの依存関係をインストールします。

    ```bash
    cd myExpressApp
    npm install
    ```

1. 次のコマンドを実行して、アプリ サーバーを開始します。

    ```bash
    npm start
    ```

1. ブラウザーを開いて [http://localhost:3000](http://localhost:3000) に移動し、実行中のアプリを表示します。

    ![Express アプリをローカルで実行](media/azure-cli/local-app.png)

1. アプリのテストが完了したら、`npm start` を実行したターミナルで **Ctrl**+**C** を押してサーバーを停止します。

> [!div class="nextstepaction"]
> [アプリを作成します](tutorial-vscode-azure-cli-node-03.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=express)

---
title: Visual Studio Code から静的 Node.js Web サイトに変更をデプロイする
description: チュートリアル パート 5、変更を加えて再デプロイする
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: buhollan
ms.openlocfilehash: 0db773cdea2e288dc461479c3753b94c9a286e82
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466645"
---
# <a name="make-changes-and-redeploy"></a>変更を加えて再デプロイする

[前の手順:Azure Storage へのデプロイ](tutorial-vscode-static-website-node-04.md)

この手順では、アプリのソース コードに簡単な変更を行ってサイトを再デプロイして、エンドツーエンドのデプロイ ワークフローを体験します。

# <a name="angulartabangular"></a>[Angular](#tab/angular)

1. Visual Studio Code で _src/app/app.component.html_ ファイルを開き、次に一致するように 305 行目を変更します。

    ```html
    <span>Welcome To Azure</span>
    ```

1. ターミナルまたはコマンド プロンプトで `npm run build` を実行します。

1. VS Code で、更新した _dist/my-static-site_ フォルダーを右クリックし、もう一度 **[Deploy to Static Website]\(静的な Web サイトにデプロイする\)** を選択します。 Storage アカウントを選択し、変更をデプロイすることを確認します。 (Azure 拡張機能では、キャッシュの問題を回避するために、変更をデプロイする前に古いファイルが自動的に削除されます。)

1. デプロイが完了したら、ブラウザーでサイトを更新して変更を確認します。

    ![再デプロイ後のアプリの変更](media/static-website/updated-azure-app-angular.png)

# <a name="reacttabreact"></a>[React](#tab/react)

1. Visual Studio Code で _src/app.js_ ファイルを開き、次に一致するように 11 行目を変更します。

    ```html
    <h1 className="App-title">Welcome to Azure!</h1>
    ```

1. ターミナルまたはコマンド プロンプトで `npm run build` を実行します。

1. VS Code で、更新した _build_ フォルダーを右クリックし、もう一度 **[Deploy to Static Website]\(静的な Web サイトにデプロイする\)** を選択します。 Storage アカウントを選択し、変更をデプロイすることを確認します。 (Azure 拡張機能では、キャッシュの問題を回避するために、変更をデプロイする前に古いファイルが自動的に削除されます。)

1. デプロイが完了したら、ブラウザーでサイトを更新して変更を確認します。

    ![再デプロイ後のアプリの変更](media/static-website/updated-azure-app-react.png)

# <a name="vuetabvue"></a>[Vue](#tab/vue)

1. Visual Studio Code で _src/App.vue_ ファイルを開き、次に一致するように 11 行目を変更します。

    ```html
    <HelloWorld msg="Welcome to Azure!" />
    ```

1. ターミナルまたはコマンド プロンプトで `npm run build` を実行します。

1. VS Code で、更新した _dist_ フォルダーを右クリックし、もう一度 **[Deploy to Static Website]\(静的な Web サイトにデプロイする\)** を選択します。 Storage アカウントを選択し、変更をデプロイすることを確認します。 (Azure 拡張機能では、キャッシュの問題を回避するために、変更をデプロイする前に古いファイルが自動的に削除されます。)

1. デプロイが完了したら、ブラウザーでサイトを更新して変更を確認します。

    ![再デプロイ後のアプリの変更](media/static-website/updated-azure-app-vue.png)

---

> [!div class="nextstepaction"]
> [変更をデプロイしました](tutorial-vscode-static-website-node-06.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=code-change)

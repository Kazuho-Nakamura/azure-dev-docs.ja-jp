---
title: チュートリアル:Visual Studio Code から Azure App Service on Linux にデプロイするアプリを準備する
description: チュートリアルの手順 2、アプリケーションの設定
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: e504e78ae660719c60827db46d4801f5f5c4b3ce
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466215"
---
# <a name="tutorial-prepare-your-app-for-deployment-to-azure-app-service"></a>チュートリアル:Azure App Service にデプロイするためにアプリを準備する

[前の手順: 前提条件](tutorial-deploy-app-service-on-linux-01.md)

この記事では、このチュートリアルの Azure App Service にデプロイするアプリを準備します。 既存のアプリを使用することも、アプリを作成またはダウンロードすることもできます。

作業対象のアプリが既にある場合は、フレームワーク (Flask、Django など) を含む依存関係を記述した *requirements.txt* ファイルがあることを確認してください。

まだアプリがない場合は、次のいずれかの方法を使用します。 アプリがローカルで動作することを確認してください。

## <a name="minimal-flask-app"></a>最小限の Flask アプリ

このセクションでは、このチュートリアルで使用する最小限の Flask アプリについて説明します。

1. 新しいフォルダーを作成して VS Code で開き、以下の内容を含んだ *hello.py* という名前のファイルを追加します。 App Service のスタートアップ コマンドで名前がどのように使用されるか (後述) を説明するために、このアプリ オブジェクトには意図的に `myapp` という名前を付けています。

    ```python
    from flask import Flask
    myapp = Flask(__name__)

    @myapp.route("/")
    def hello():
        return "Hello Flask, on Azure App Service for Linux"
    ```

1. *requirements.txt* という名前のファイルを作成し、内容を次のようにします。

    ```text
    Flask==1.1.1
    ```

1. [Flask チュートリアル (Flask 用のプロジェクト環境の作成)](https://code.visualstudio.com/docs/python/tutorial-flask#create-a-project-environment-for-flask) の手順に従って、Flask がインストールされた仮想環境を作成します。この仮想環境内のローカルでアプリを実行することができます。

1. このアプリを実行するには、(オペレーティング システムに応じて) 次のコマンドを使用します。 FLASK_APP 環境変数によって、アプリ オブジェクトの検索先が Flask に伝えられます。

    ```ps
    set FLASK_APP=hello:myapp
    flask run
    ```

    ```bash
    export FLASK_APP=hello:myapp
    flask run
    ```

    その後、`http://127.0.0.1:5000/` という URL を使用してアプリをブラウザーで開くことができます。

## <a name="vs-code-flask-tutorial-sample"></a>VS Code Flask チュートリアル サンプル

[python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial) をダウンロードまたは複製します。これは [Flask チュートリアル](https://code.visualstudio.com/docs/python/tutorial-flask)に従って得られた結果です。

## <a name="vs-code-django-tutorial-sample"></a>VS Code Django チュートリアル サンプル

[Django チュートリアル](https://code.visualstudio.com/docs/python/tutorial-django)で得られた結果である [python-sample-vscode-django-tutorial](https://github.com/Microsoft/python-sample-vscode-django-tutorial) をダウンロードまたは複製します。

このサンプルのようにローカル SQLite データベースが Django アプリに使用されている場合、事前初期化済みかつ事前設定済みの *db.sqlite3* ファイルのコピーをリポジトリに追加する必要があります。 これは、現在 App Service for Linux には、デプロイの過程で Django の `migrate` コマンドを実行する手段が用意されていないため、あらかじめ作成しておいたデータベースを自分でデプロイしなければならないためです。 その場合でも、データベースは事実上読み取り専用であり、また、データベースに書き込みを行うとエラーが発生します。

いずれのケースも最善の方法は、アプリのコードとは無関係にデプロイおよび初期化された別個のデータベースを使用することです。

> [!div class="nextstepaction"]
> [アプリの準備が完了しました](tutorial-deploy-app-service-on-linux-03.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=02-prepare-app)

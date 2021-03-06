---
title: Azure Cosmos DB で Spring Data MongoDB API を使用する方法
description: Azure Cosmos DB で Spring Data MongoDB API を使用する方法を説明します。
services: cosmos-db
documentationcenter: java
ms.date: 12/19/2018
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 0284f89f6a37497709947649fba3b1284416a95c
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74811938"
---
# <a name="how-to-use-spring-data-mongodb-api-with-azure-cosmos-db"></a>Azure Cosmos DB で Spring Data MongoDB API を使用する方法

## <a name="overview"></a>概要

この記事では、[Spring Data] を使用して、[Azure Cosmos DB MongoDB API](/azure/cosmos-db/mongodb-introduction) を使って情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。

## <a name="prerequisites"></a>前提条件

この記事の手順を実行するには、次の前提条件を満たす必要があります。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。
* [Git](https://git-scm.com/downloads) クライアント。

## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB アカウントを作成する

### <a name="create-a-cosmos-db-account-using-the-azure-portal"></a>Azure portal を使用して Cosmos DB アカウントを作成する

> [!NOTE]
> 
> Azure Cosmos DB アカウントの作成に関する詳細については、[Azure Cosmos DB のドキュメント](/azure/cosmos-db/)を参照してください。

1. Azure portal (<https://portal.azure.com/>) を参照し、サインインします。

1. **[+ リソースの作成]** をクリックし、 **[データベース]** 、 **[Azure Cosmos DB]** の順にクリックします。

   ![Azure Cosmos DB アカウントを作成する][COSMOSDB01]

1. 次の情報を指定します。

   - **サブスクリプション**:使用する Azure サブスクリプションを指定します。
   - **[リソース グループ]** :新しいリソース グループを作成するのか、既存のリソース グループを選択するのかを指定します。
   - **アカウント名**:Cosmos DB アカウント用に一意の名前を選択します。この名前は、*wingtiptoysmongodb.documents.azure.com* のような完全修飾ドメイン名の作成に使用されます。
   - **API**:このチュートリアルでは、`Azure Cosmos DB for MongoDB API` を指定します。
   - **[場所]** :データベースに最も近い地理的リージョンを指定します。

   ![Cosmos DB アカウントの設定を指定する][COSMOSDB02]
   
1. 上記の情報をすべて入力したら、 **[レビュー + 作成]** をクリックします。

1. レビュー ページに問題がなければ、 **[作成]** をクリックします。

   ![Cosmos DB アカウントの設定を確認する][COSMOSDB03]

### <a name="retrieve-the-connection-string-for-your-azure-cosmos-db-account"></a>Azure Cosmos DB アカウントの接続文字列を取得する

1. Azure portal (<https://portal.azure.com/>) を参照し、サインインします。

1. **[すべてのリソース]** をクリックしてから、先ほど作成した Azure Cosmos DB アカウントをクリックします。

1. **[接続文字列]** をクリックし、 **[プライマリ接続文字列]** フィールドの値をコピーします。この値は、アプリケーションを構成するために後で使用します。

   ![Cosmos DB の接続文字列を取得する][COSMOSDB06]

## <a name="configure-the-sample-application"></a>サンプル アプリケーションを構成する

1. コマンド シェルを開き、次の例のように git コマンドを使用してサンプル プロジェクトを複製します。

   ```shell
   git clone https://github.com/spring-guides/gs-accessing-data-mongodb.git
   ```

1. サンプル プロジェクトの *&lt;プロジェクト ルート&gt;/complete/src/main* ディレクトリに *resources* ディレクトリを作成し、この *resources* ディレクトリに *application.properties* ファイルを作成します。

1. テキスト エディターで *application.properties* ファイルを開き、このファイルに次の行を追加して、サンプルの値を前半の該当する値に置き換えます。

   ```yaml
   spring.data.mongodb.database=wingtiptoysmongodb
   spring.data.mongodb.uri=mongodb://wingtiptoysmongodb:AbCdEfGhIjKlMnOpQrStUvWxYz==@wingtiptoysmongodb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb
   ```
   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `spring.data.mongodb.database` | この記事の前半の Cosmos DB アカウントの名前を指定します。 |
   | `spring.data.mongodb.uri` | この記事の前半の**プライマリ接続文字列**を指定します。 |

1. *application.properties* ファイルを保存して閉じます。

## <a name="package-and-test-the-sample-application"></a>サンプル アプリケーションをパッケージ化してテストする

アプリケーションをビルドするには、 */gs-accessing-data-mongodb/complete* ディレクトリを参照します。ここには、pom.xml ファイルがあります。

1. Maven でサンプル アプリケーションをビルドし、テストをスキップします。次に例を示します。

   ```shell
   mvn clean package -DskipTests
   ```

1. サンプル アプリケーションを開始します。次に例を示します。

   ```shell
   java -jar target/accessing-data-mongodb-0.0.1-SNAPSHOT.jar
   ```
    
   アプリケーションから次のような値が返されます。

   ```json
   Customers found with findAll():
   -------------------------------
   Customer[id=5c1b4ae4d0b5080ac105cc13, firstName='Alice', lastName='Smith']
   Customer[id=5c1b4ae4d0b5080ac105cc14, firstName='Bob', lastName='Smith']
   
   Customer found with findByFirstName('Alice'):
   --------------------------------
   Customer[id=5c1b4ae4d0b5080ac105cc13, firstName='Alice', lastName='Smith']
   Customers found with findByLastName('Smith'):
   --------------------------------
   Customer[id=5c1b4ae4d0b5080ac105cc13, firstName='Alice', lastName='Smith']
   Customer[id=5c1b4ae4d0b5080ac105cc14, firstName='Bob', lastName='Smith']
   ```

## <a name="summary"></a>まとめ

このチュートリアルでは、Spring Data を使用して、Azure Cosmos DB MongoDB API を使って情報を格納および取得する Java のサンプル アプリケーションを作成しました。

## <a name="next-steps"></a>次の手順

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/azure/java/spring-framework)

### <a name="additional-resources"></a>その他のリソース

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。

<!-- URL List -->

[Java 開発者向けの Azure]: /azure/java/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Azure DevOps と Java の操作]: /azure/devops/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Data]: https://spring.io/projects/spring-data
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[COSMOSDB01]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-01.png
[COSMOSDB02]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-02.png
[COSMOSDB03]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-03.png
[COSMOSDB04]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-04.png
[COSMOSDB06]: media/configure-spring-data-mongodb-with-cosmos-db/create-cosmos-db-06.png

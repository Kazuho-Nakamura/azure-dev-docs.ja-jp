---
title: Azure Database for MySQL で Spring Data JDBC を使用する方法
description: Azure Database for MySQL データベースで Spring Data JDBC を使用する方法を説明します。
documentationcenter: java
ms.date: 01/07/2020
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.topic: conceptual
ms.openlocfilehash: 7a6550be633b29d97d55b8db2f50b2c57d0ba30d
ms.sourcegitcommit: 2ad3f7ce8c87331f8aff759ac2a3dc1b29581866
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "76022075"
---
# <a name="how-to-use-spring-data-jdbc-with-azure-mysql"></a>Azure MySQL で Spring Data JDBC を使用する方法

## <a name="overview"></a>概要

この記事では、[Spring Data] を使用して、[Java Database Connectivity (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) を使って [Azure Database for MySQL](/azure/mysql/) データベース内の情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。

## <a name="prerequisites"></a>前提条件

この記事の手順を実行するには、次の前提条件を満たす必要があります。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。
* [Curl](https://curl.haxx.se/) または機能をテストするための類似の HTTP ユーティリティ。
* [mysql](https://dev.mysql.com/downloads/) コマンドライン ユーティリティ。
* [Git](https://git-scm.com/downloads) クライアント。

## <a name="create-an-azure-database-for-mysql"></a>Azure Database for MySQL の作成 

### <a name="create-a-server-using-the-azure-portal"></a>Azure portal を使用してサーバーを作成する

> [!NOTE]
> 
> MySQL データベースの作成に関する詳細については、「[Azure portal を使用した Azure Database for MySQL サーバーの作成](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal)」を参照してください。

1. Azure portal (<https://portal.azure.com/>) を参照し、サインインします。

1. **[+ リソースの作成]** をクリックし、 **[データベース]** 、 **[Azure Database for MySQL]** の順にクリックします。

   ![MySQL データベースを作成する][MYSQL01]

1. 次の情報を入力します。

   - **サブスクリプション**:使用する Azure サブスクリプションを指定します。
   - **[リソース グループ]** :新しいリソース グループを作成するのか、既存のリソース グループを選択するのかを指定します。
   - **[サーバー名]** : MySQL サーバー用に一意の名前を選択します。この名前は、*wingtiptoysmysql.mysql.database.azure.com* のような完全修飾ドメイン名の作成に使用されます。
   - **データ ソース**:このチュートリアルでは、`Blank` を選択して新しいデータベースを作成します。
   - **管理者ユーザー名**:データベース管理者名を指定します。
   - **[パスワード]** と **[パスワードの確認]** :データベース管理者のパスワードを指定します。
   - **[場所]** :データベースに最も近い地理的リージョンを指定します。
   - **バージョン**:最新のデータベース バージョンを指定します。

   ![MySQL データベースのプロパティを作成する][MYSQL02]

1. 上記の情報をすべて入力したら、 **[レビュー + 作成]** をクリックします。

1. 指定した内容を確認し、 **[作成]** をクリックします。

### <a name="configure-a-firewall-rule-for-your-server-using-the-azure-portal"></a>Azure portal を使用してサーバーのファイアウォール規則を構成する

1. Azure portal (<https://portal.azure.com/>) を参照し、サインインします。

1. **[すべてのリソース]** をクリックし、先ほど作成した Azure Database for MySQL リソースをクリックします。

1. **[接続のセキュリティ]** をクリックし、 **[ファイアウォール規則]** で、規則の一意の名前を指定して新しい規則を作成し、データベースへのアクセス権を必要とする IP アドレスの範囲を入力して、 **[保存]** をクリックします。 (この演習では、クライアントである開発用マシンの IP アドレスです。  これを **[開始 IP アドレス]** と **[終了 IP アドレス]** の両方に使用できます。)

   ![接続のセキュリティを構成する][MYSQL04]

### <a name="retrieve-the-connection-string-for-your-server-using-the-azure-portal"></a>Azure portal を使用してサーバーの接続文字列を取得する

1. Azure portal (<https://portal.azure.com/>) を参照し、サインインします。

1. **[すべてのリソース]** をクリックし、先ほど作成した Azure Database for MySQL を選択します。

1. **[接続文字列]** をクリックし、 **[JDBC]** テキスト フィールド内の値をコピーします。

   ![JDBC 接続文字列を取得する][MYSQL05]

### <a name="create-a-database-using-the-mysql-command-line-utility"></a>`mysql` コマンド ライン ユーティリティを使用してデータベースを作成する

1. コマンド シェルを開き、次の例のように `mysql` コマンドを入力して MySQL サーバーに接続します。

   ```shell
   mysql --host wingtiptoysmysql.mysql.database.azure.com --user wingtiptoysuser@wingtiptoysmysql -p
   ```
   各値の説明:

   | パラメーター | [説明] |
   |---|---|
   | `host` | この記事の前半の完全修飾 MySQL サーバー名を指定します。 |
   | `user` | この記事の前半の MySQL 管理者と短縮サーバー名を指定します。 |
   | `p` | パスワードが要求されるまで待機するように指定します。 |

   MySQL サーバーは、次の例のような表示で応答します。

   ```shell
   Welcome to the MySQL monitor.  Commands end with ; or \g.
   Your MySQL connection id is 64552
   Server version: 5.6.39.0 MySQL Community Server (GPL)
   
   Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.
   
   Oracle is a registered trademark of Oracle Corporation and/or its
   affiliates. Other names may be trademarks of their respective
   owners.
   
   Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
   
   mysql>
   ```
   > 注:サーバーがこの IP アドレスを認識しないというエラーが表示される場合は、クライアントが使用している IP アドレスがエラーに示されます。  前に戻って、前の手順で説明したようにこれを割り当てます。*Azure portal を使用してサーバーのファイアウォール規則を構成する*。

1. 次の例のように `mysql` コマンドを入力して、*mysqldb* という名前のデータベースを作成します。

   ```SQL
   CREATE DATABASE mysqldb;
   ```

   MySQL サーバーは、次の例のような表示で応答します。

   ```shell
   Query OK, 1 row affected (0.30 sec)
   ```

1. 省略可能:次の例のように、`mysql` コマンドを入力することで、データベースが作成されたことを確認できます。

   ```SQL
   SHOW DATABASES;
   ```

   MySQL サーバーは、次の例のような表示で応答します。

   ```shell
   +--------------------+
   | Database           |
   +--------------------+
   | information_schema |
   | mysql              |
   | mysqldb            |
   | performance_schema |
   | sys                |
   +--------------------+
   ```

1. `\q` を入力して `mysql` ユーティリティを終了します。

## <a name="configure-the-sample-application"></a>サンプル アプリケーションを構成する

1. コマンド シェルを開き、次の例のように git コマンドを使用してサンプル プロジェクトを複製します。

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. サンプル プロジェクトの *resources* ディレクトリ内で *application.properties* ファイルを探すか、まだ存在しない場合はファイルを作成します。

1. テキスト エディターで *application.properties* ファイルを開き、このファイルに次の行を追加するか構成して、サンプルの値を前半の該当する値に置き換えます。

   ```yaml
   spring.datasource.url=jdbc:mysql://wingtiptoysmysql.mysql.database.azure.com:3306/mysqldb?useSSL=true&requireSSL=false&serverTimezone=UTC
   spring.datasource.username=wingtiptoysuser@wingtiptoysmysql
   spring.datasource.password=********
    ```
   各値の説明:

   | パラメーター | [説明] |
   |---|---|
   | `spring.datasource.url` | タイム ゾーンを追加して、この記事の前半の MySQL JDBC 文字列を指定します。 |
   | `spring.datasource.username` | この記事の前半の MySQL 管理者名を指定し、その後に短縮サーバー名を追加します。 |
   | `spring.datasource.password` | この記事の前半の MySQL 管理者パスワードを指定します。 |

1. *application.properties* ファイルを保存して閉じます。

## <a name="package-and-test-the-sample-application"></a>サンプル アプリケーションをパッケージ化してテストする 

1. サンプル アプリケーションを Maven でビルドします。次に例を示します。

   ```shell
   mvn clean package -P mysql
   ```

1. サンプル アプリケーションを開始します。次に例を示します。

   ```shell
   java -jar target/spring-data-jdbc-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. 次の例のように、コマンド プロンプトから `curl` を使用して新しいレコードを作成します。

   ```shell
   curl -s -d "{\"name\":\"dog\",\"species\":\"canine\"}" -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d "{\"name\":\"cat\",\"species\":\"feline\"}" -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   アプリケーションから次のような値が返されます。

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. 次の例のように、コマンド プロンプトから `curl` を使用して既存のすべてのレコードを取得します。

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   アプリケーションから次のような値が返されます。

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a>まとめ

このチュートリアルでは、Spring Data を使用して、JDBC を使って Azure Database for MySQL データベース内の情報を格納および取得する Java のサンプル アプリケーションを作成しました。

## <a name="next-steps"></a>次のステップ

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

[MYSQL01]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-01.png
[MYSQL02]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-02.png
[MYSQL04]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-04.png
[MYSQL05]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-05.png

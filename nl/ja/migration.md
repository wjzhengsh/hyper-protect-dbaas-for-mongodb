---

copyright:
  years: 2019
lastupdated: "2019-06-12"

keywords: migrate, beta, restore

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# {{site.data.keyword.mongodb}} データベースの {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} へのマイグレーション
{: #migration_mongodb}

{{site.data.keyword.mongodb}} データベースを使用していて、{{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}} にマイグレーションする場合には、このトピックで詳細を確認してください。 プラットフォームで {{site.data.keyword.mongodb}} データベース・サービスが提供されている限り、{{site.data.keyword.mongodb}} データベースがホストされている場所は問題にはなりません。
{: shortdesc}

## 始めに
{: #migration_mongodb_before_begin}

{{site.data.keyword.mongodb}} コマンドを使用してマイグレーションを実行するには、まずマシンに {{site.data.keyword.mongodb}} をダウンロードしてインストールする必要があります。 詳しくは、[{{site.data.keyword.mongodb}} の Web サイト](https://docs.mongodb.com/manual/installation/){: external}を参照してください。

## 手順 1: 元のデータベースを復元するためのダンプ・ファイルを作成する
{: #step1_create_dump_file}

以下の `mongodump` コマンドを使用して、復元するデータベースが含まれるダンプ・ファイルを作成します。

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_file>
```
{: pre}

以下の表に、このコマンドで使用する変数の説明を記載します。

|変数|説明|例|
|---------|-----------|-------|
|*replica_set_name*|{{site.data.keyword.mongodb}} レプリカ・セット名。 クラスターを {{site.data.keyword.ihsdbaas_full}} ベータ・インスタンスでデプロイした場合は、インスタンスを作成するときに設定したクラスター名です。 クラスターの URI は、クラスター UI の概要ページで確認できます。 `replicaSet=` の後の値が名前です。|MongoDB-001|
|*host_name1、host_name2、host_name3*|レプリカ・セットをホストしているホスト・サーバーの名前。 レプリカは 3 つあります。 クラスターを {{site.data.keyword.ihsdbaas_full}} ベータ・インスタンスでデプロイした場合は、概要ページのクラスター URI でホスト名を確認できます。|dbaas29.hyperprotectdbaas.cloud.ibm.com、dbaas31.hyperprotectdbaas.cloud.ibm.com、dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1、port2、port3*|対応するホストに接続するためのポート。 クラスターを {{site.data.keyword.ihsdbaas_full}} ベータ・インスタンスでデプロイした場合は、概要ページのクラスター URI でポート番号を確認できます。|28205、28016、28175|
|*user_name*|元のデータベースの認証を受けるユーザー名。 マイグレーションするデータベースに対する READ 特権を持つユーザーでなければなりません。|my_user|
|*authentication_database_name*|指定したユーザーの認証データベース。 認証データベースを指定しない場合、`mongodump` では、`--db` オプションで設定するデータベースが想定されます。 `--db` オプションも指定しない場合、`mongodump` では admin データベースが想定されます。|admin|
|*database_name*|マイグレーションするデータベース。 データベースを指定しない場合、`mongodump` は対象ユーザーに属するすべてのデータベースをダンプします。|my_database|
|*cert_file*|認証局の `.pem` ファイル。 クラスターを {{site.data.keyword.ihsdbaas_full}} ベータ・インスタンスでデプロイした場合は、概要ページから証明書ファイルをダウンロードできます。 このファイルは、絶対パスと相対パスのどちらでも指定できます。|./cert.pem|
|*dump_file*|元のデータが格納される `.dump` ファイル。 ファイルの指定には絶対パスと相対パスのどちらでも使用できます。|./mongo.dump|
{: caption="表 1. ダンプ・ファイルを作成するときに必要な変数"}

`mongodump` ユーティリティーの詳細については、[{{site.data.keyword.mongodb}} の資料](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}を参照してください。

ほとんどの情報はクラスター URI から取得できます。 URI の形式は、`mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>` です。 この値を前述の `mongodump` コマンドの対応するフィールドと突き合わせることができます。
{: tip}

## 手順 2: {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} に新しいサービス・インスタンスを作成する
{: #step2_creat_new_service_instance}

データを復元する前に、ターゲット・データベース・クラスターである新しいサービス・インスタンスを {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} に作成する必要があります。 新しいインスタンスは以下のいずれかを使用して作成できます。
- [Web ユーザー・インターフェース](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service#dbaas_webui_create_service)
- [DBaaS Manger API](https://{DomainName}/apidocs/hyperp-dbaas#create-an-ibm-cloud-service-instance-of-hyperprote){: external}
- [{{site.data.keyword.cloud_notm}} CLI の CLI プラグイン](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_create_service)

新しいサービス・インスタンスを作成するときには、クラスター名、管理者名、パスワードを設定する必要があります。元のインスタンスと同じにする必要はありません。 マイグレーションには影響ありません。 マイグレーションの完了後に、データベースは新しい管理者データベース/認証データベースに割り当てられます。

## 手順 3: ダンプ・ファイルのデータを新しいサービス・インスタンスに復元する
{: #step3_restore_data}

`mongorestore` コマンドを使用して、[手順 1](#step1_create_dump_file) で作成したダンプ・ファイルのデータを復元します。

```
mongorestore --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> <dump_file>
```
{: pre}

以下の表に、このコマンドで使用する変数の説明を記載します。

|変数|説明|例|
|---------|-----------|-------|
|*replica_set_name*|ターゲットの {{site.data.keyword.mongodb}} レプリカ・セット名。 インスタンスを作成するときに設定したクラスター名です。 概要ページのクラスター URI で確認できます。 `replicaSet=` の後の値が名前です。|MongoDB-002|
|*host_name1、host_name2、host_name3*|ターゲットのレプリカ・セットをホストしている DBaaS Manager サーバー。 レプリカは 3 つあります。 概要ページのクラスター URI でホスト名を確認できます。|dbaas29.hyperprotectdbaas.cloud.ibm.com、dbaas31.hyperprotectdbaas.cloud.ibm.com、dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1、port2、port3*|対応するホストに接続するためのポート。 概要ページのクラスター URI でポートを確認できます。|28128、28248、28043|
|*user_name*|ターゲット・データベースの認証を受けるユーザー名。 ダンプ・ファイルを作成したユーザーと同じにする必要はありません。|new_user|
|*authentication_database_name*|指定したユーザーの認証データベース。 認証データベースを指定しない場合、`mongodump` では、`--db` オプションで設定するデータベースが想定されます。 `--db` オプションも指定しない場合、`mongodump` では admin データベースが想定されます。|admin|
|*database_name*|データのマイグレーション先にするターゲット・データベース。 データベースが存在しない場合は、`mongorestore` によって自動的に作成されます。 データベースを指定しない場合は、元のデータベースに基づくデータベースが `mongorestore` によって作成されます。|my_database|
|*cert_file*|認証局の `.pem` ファイル。 このファイルはインスタンスの概要ページからダウンロードできます。相対パスと絶対パスのどちらでも指定できます。|./cert.pem|
|*dump_file*|[手順 1](#step1_create_dump_file) で作成した `.dump` ファイル。 ファイルの指定には絶対パスと相対パスのどちらでも使用できます。|./mongo.dump|
{: caption="表 2. ダンプ・ファイルのデータを復元するときに必要な変数"}

[Web ユーザー・インターフェース](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas-webui-database-users#webui-create-database-user)、[DBaaS Manger API](https://{DomainName}/apidocs/hyperp-dbaas#create-a-user-in-a-database-cluster){: external}、[{{site.data.keyword.cloud_notm}} CLI の CLI プラグイン](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_plugin#user_create)、[{{site.data.keyword.mongodb}} コマンド](https://docs.mongodb.com/manual/reference/method/db.createUser/index.html){: external}によって、新しいユーザーを作成できます。
{: tip}

{{site.data.keyword.mongodb}} では、既存のデータがある場合にマイグレーションを実行すると、既存のデータが上書きされるのではなく、元のデータがターゲット・クラスターにマージされます。 {{site.data.keyword.mongodb}} の Web サイトに、この機能の[詳細](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external} が記載されています。 また、`mongorestore` ユーティリティーの詳細についても、[{{site.data.keyword.mongodb}} の資料](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external}を参照してください。

## 次の作業
{: #whats_next_migration_mongodb}

マイグレーションしたら、新しいデータベース・クラスターに接続して、データが正常にマイグレーションされたかどうかを確認できます。

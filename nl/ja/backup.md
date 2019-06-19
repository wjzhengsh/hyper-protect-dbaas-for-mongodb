---

copyright:
  years: 2019
lastupdated: "2019-06-12"

keywords: backup, disaster recovery, restore

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:tip: .tip}


# {{site.data.keyword.cos_full_notm}} を使用したデータベースのバックアップとリストア
{: #backup_mongodb_databases}

{{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}} サービスは、24 時間ごとに 1 回、データベース全体のバックアップを自動的にトリガーします。 これらの暗号化されたバックアップの直近 7 日分が、サポート対象領域のすべてのアベイラビリティー・ゾーンのローカル・ストレージ上に冗長的に保持されます。
{: shortdesc}

災害復旧能力を向上させるためにサポート対象外の地域にデータをバックアップするには、以下の手順を参照することで、[{{site.data.keyword.cos_full_notm}} サービス](https://cloud.ibm.com/catalog/services/cloud-object-storage){: external}を使用して別の地域にデータを保管できます。

## 始めに
{: #backup_mongodb_before_begin}

{{site.data.keyword.mongodb}} コマンドを使用してバックアップを実行するには、まずマシンに {{site.data.keyword.mongodb}} をダウンロードしてインストールする必要があります。 詳しくは、[{{site.data.keyword.mongodb}} の Web サイト](https://docs.mongodb.com/manual/installation/){: external}を参照してください。

## 手順 1: 元のデータベースをバックアップするためのダンプ・ファイルを作成する
{: #step1_create_dump_file_backup_mongodb}

以下の `mongodump` コマンドを使用して、バックアップするデータベースが含まれるダンプ・ファイルを作成します。

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_file>
```
{: pre}

以下の表に、このコマンドで使用する変数の説明を記載します。

|変数|説明|例|
|---------|-----------|-------|
|*replica_set_name*|{{site.data.keyword.mongodb}} レプリカ・セット名。 インスタンスを作成するときに設定したクラスター名です。 クラスターの URI は、クラスター UI の概要ページで確認できます。 `replicaSet=` の後の値が名前です。|MongoDB-001|
|*host_name1、host_name2、host_name3*|レプリカ・セットをホストしている DBaaS Manager サーバー。 レプリカは 3 つあります。 概要ページのクラスター URI でホスト名を確認できます。|dbaas29.hyperprotectdbaas.cloud.ibm.com、dbaas31.hyperprotectdbaas.cloud.ibm.com、dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1、port2、port3*|対応するホストに接続するためのポート。 概要ページのクラスター URI でポート番号を確認できます。|28205、28016、28175|
|*user_name*|元のデータベースの認証を受けるユーザー名。 マイグレーションするデータベースに対する READ 特権を持つユーザーでなければなりません。|my_user|
|*authentication_database_name*|指定したユーザーの認証データベース。 認証データベースを指定しない場合、`mongodump` では、`--db` オプションで設定するデータベースが想定されます。 `--db` オプションも指定しない場合、`mongodump` では admin データベースが想定されます。|admin|
|*database_name*|バックアップするデータベース。 データベースを指定しない場合、`mongodump` は対象ユーザーに属するすべてのデータベースをダンプします。|my_database|
|*cert_file*|認証局の `.pem` ファイル。 証明書ファイルは概要ページからダウンロードできます。相対パスと絶対パスのどちらでも指定できます。|./cert.pem|
|*dump_file*|元のデータが格納される `.dump` ファイル。 ファイルの指定には絶対パスと相対パスのどちらでも使用できます。|./mongo.dump|
{: caption="表 1. ダンプ・ファイルを作成するときに必要な変数"}

`mongodump` ユーティリティーの詳細については、[{{site.data.keyword.mongodb}} の資料](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}を参照してください。

ほとんどの情報はクラスター URI から取得できます。 URI の形式は、`mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>` です。 この値を前述の `mongodump` コマンドの対応するフィールドと突き合わせることができます。
{: tip}

## 手順 2: 新規 {{site.data.keyword.cos_full_notm}} インスタンスを作成する
{: #step2_create_object_storage_backup_mongodb}

{{site.data.keyword.ihsdbaas_mongodb_full}} サービス・インスタンスがホストされている地域とは別の地域でインスタンスを作成する必要があります。 以下の手順を参照してください。

1. [新規 Cloud {{site.data.keyword.cos_short}} インスタンスを作成します](/docs/services/cloud-object-storage?topic=cloud-object-storage-provision)。
2. 他の地域に[バケットを作成](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-region)します。
3. [サービス資格情報を作成](/docs/services/cloud-object-storage?topic=cloud-object-storage-service-credentials)し、後で使用するために `access_key_id` と `secret_access_key` を保存します。
4. S3 クライアントをインストールします。
5. S3 クライアントとアクセス・キーを使用して、前の手順で作成したバケットの Cloud {{site.data.keyword.cos_short}} エンドポイントに接続します。
6. S3 クライアントを使用して、[手順 1](#step1_create_dump_file_backup_mongodb) で作成した [{{site.data.keyword.mongodb}} バックアップ・ファイルをバケットにアップロードします](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload)。

詳しくは、[{{site.data.keyword.cos_full_notm}} の資料](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started) を参照してください。

この手順を完了すると、別の地域の Cloud {{site.data.keyword.cos_short}} インスタンスにデータが正常にバックアップされます。 クラウド {{site.data.keyword.cos_short}} インスタンスから {{site.data.keyword.ihsdbaas_mongodb_full}} インスタンスにデータをリストアする場合は、以下の手順 3 を読んで詳細を確認してください。

## 手順 3: クラウド {{site.data.keyword.cos_short}} インスタンスから {{site.data.keyword.ihsdbaas_mongodb_full}} インスタンスにデータをリストアする
{: #step3_restore_data_from_cos_mongodb}

まずクラウド {{site.data.keyword.cos_short}} バケットからローカル・マシンにバックアップ・ファイルをダウンロードしてから、`mongorestore` コマンドを使用してデータをリストアする必要があります。

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
|*database_name*|データのリストア先にするターゲット・データベース。 データベースが存在しない場合は、`mongorestore` によって自動的に作成されます。 データベースを指定しない場合は、元のデータベースに基づくデータベースが `mongorestore` によって作成されます。|my_database|
|*cert_file*|認証局の `.pem` ファイル。 このファイルはインスタンスの概要ページからダウンロードできます。相対パスと絶対パスのどちらでも指定できます。|./cert.pem|
|*dump_file*|Cloud {{site.data.keyword.cos_short}} バケットからダウンロードした `.dump` ファイル。 ファイルの指定には絶対パスと相対パスのどちらでも使用できます。|./mongo.dump|
{: caption="表 2. ダンプ・ファイルのデータを復元するときに必要な変数"}

{{site.data.keyword.mongodb}} では、既存のデータがある場合にマイグレーションを実行すると、既存のデータが上書きされるのではなく、元のデータがターゲット・クラスターにマージされます。 {{site.data.keyword.mongodb}} の Web サイトに、この機能の[詳細](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external} が記載されています。 また、`mongorestore` ユーティリティーの詳細についても、[{{site.data.keyword.mongodb}} の資料](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external}を参照してください。

## 次の作業
{: #whats_next_backup_mongodb}

[手順 3](#step3_restore_data_from_cos_mongodb) が終わったら、データベース・クラスターに接続して、データが正常にリストアされたかどうかを確認できます。

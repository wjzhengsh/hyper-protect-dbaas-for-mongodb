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


# 使用 {{site.data.keyword.cos_full_notm}} 備份和還原資料庫
{: #backup_mongodb_databases}

{{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}} 服務每 24 小時會自動觸發一次完整資料庫的備份。其中可用的是最近 7 天的加密備份，這些備份以冗餘方式在受支援地區中所有可用性區域的本端儲存空間提供。
{: shortdesc}

如果要提高災難回復功能，並將資料備份到受支援地區外，可以使用 [{{site.data.keyword.cos_full_notm}} 服務](https://cloud.ibm.com/catalog/services/cloud-object-storage){: external}，以參照下列步驟將資料儲存在其他地區。

## 開始之前
{: #backup_mongodb_before_begin}

若要使用 {{site.data.keyword.mongodb}} 指令來完成備份，您需要先在機器上下載並安裝 {{site.data.keyword.mongodb}}。如需相關資訊，請參閱 [{{site.data.keyword.mongodb}} 網站](https://docs.mongodb.com/manual/installation/){: external}。

## 步驟 1：建立用於備份原始資料庫的傾出檔案
{: #step1_create_dump_file_backup_mongodb}

使用下列 `mongodump` 指令來建立包含要備份的資料庫的傾出檔案。

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_file>
```
{: pre}

下表說明指令中使用的變數。

|變數|說明|範例|
|---------|-----------|-------|
|*replica_set_name*|{{site.data.keyword.mongodb}} 抄本集名稱。這是建立實例時設定的叢集名稱。可以在叢集使用者介面的概觀頁面上，找到叢集 URI。名稱是 `replicaSet=` 之後的值。|MongoDB-001|
|*host_name1, host_name2, host_name3*|管理抄本集的 DBaaS 管理程式伺服器。有三個抄本可用。可以在概觀頁面上的叢集 URI 中找到主機名稱。|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|用來連接對應主機的埠。可以在概觀頁面上的叢集 URI 中找到埠號。|28205, 28016, 28175|
|*user_name*|用於向原始資料庫進行鑑別的使用者名稱。使用者需要對您要移轉的資料庫具有 READ 專用權。|my_user|
|*authentication_database_name*|指定使用者的鑑別資料庫。如果您未指定鑑別資料庫，`mongodump` 會假設它是您在 `--db` 選項中設定的資料庫。如果也未指定 `--db` 選項，`mongodump` 會假設它是 admin 資料庫。|admin|
|*database_name*|要備份的資料庫。如果您未指定資料庫，`mongodump` 將傾出屬於該使用者的所有資料庫。|my_database|
|*cert_file*|憑證管理中心的 `.pem` 檔案。可以從概觀頁面下載該證書檔案，並使用相對或絕對路徑來指定該檔案。|./cert.pem|
|*dump_file*|用於儲存原始資料的 `.dump` 檔案。您可以使用相對或絕對路徑來指定檔案。|./mongo.dump|
{: caption="表 1. 建立傾出檔案所需的變數"}

如果需要有關 `mongodump` 公用程式的更多詳細資訊，可以參閱 [{{site.data.keyword.mongodb}} 文件](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}。

您可以從叢集 URI 取得大部分資訊。URI 的格式為 `mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>`。您可以讓該值與前一個 `mongodump` 指令中的對應欄位相符。
{: tip}

## 步驟 2：建立新的 {{site.data.keyword.cos_full_notm}} 實例
{: #step2_create_object_storage_backup_mongodb}

您建立該實例所在的地區需要與現行管理 {{site.data.keyword.ihsdbaas_mongodb_full}} 服務實例的位置不同。可以參閱下列步驟：

1. [建立新的 Cloud {{site.data.keyword.cos_short}} 實例](/docs/services/cloud-object-storage?topic=cloud-object-storage-provision)。
2. 在其他地區中[建立儲存區](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-region)。
3. [建立服務認證](/docs/services/cloud-object-storage?topic=cloud-object-storage-service-credentials)，並儲存 `access_key_id` 和 `secret_access_key` 以供稍後使用。
4. 安裝 S3 用戶端。
5. 使用 S3 用戶端和存取金鑰來連接至先前建立之儲存區的 Cloud {{site.data.keyword.cos_short}} 端點。
6. 使用 S3 用戶端[上傳 {{site.data.keyword.mongodb}} 備份檔](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload)（即在[步驟 1](#step1_create_dump_file_backup_mongodb) 中建立的備份檔）至儲存區。

如需相關資訊，請參閱 [{{site.data.keyword.cos_full_notm}} 文件](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started)。

完成此步驟後，您已順利在不同地區的 Cloud {{site.data.keyword.cos_short}} 實例中備份資料。如果要將資料從 Cloud {{site.data.keyword.cos_short}} 實例還原到 {{site.data.keyword.ihsdbaas_mongodb_full}} 實例，請閱讀下面的步驟 3 以取得詳細資料。

## 步驟 3：將資料從 Cloud {{site.data.keyword.cos_short}} 實例還原到 {{site.data.keyword.ihsdbaas_mongodb_full}} 實例
{: #step3_restore_data_from_cos_mongodb}

您需要先將 Cloud {{site.data.keyword.cos_short}} 儲存區中的備份檔下載到本端機器，然後使用 `mongorestore` 指令來還原資料。

```
mongorestore --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> <dump_file>
```
{: pre}

下表說明指令中使用的變數。

|變數|說明|範例|
|---------|-----------|-------|
|*replica_set_name*|目標 {{site.data.keyword.mongodb}} 抄本集名稱。這是建立實例時設定的叢集名稱。可以在概觀頁面上找到叢集 URI。名稱是 `replicaSet=` 之後的值。|MongoDB-002|
|*host_name1, host_name2, host_name3*|管理目標抄本集的 DBaaS 管理程式伺服器。有三個抄本可用。可以在概觀頁面上的叢集 URI 中找到主機名稱。|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|用來連接對應主機的埠。可以在概觀頁面上的叢集 URI 中找到埠。|28128, 28248, 28043|
|*user_name*|用於向目標資料庫進行鑑別的使用者名稱。使用者不必與建立傾出檔案的使用者相同。|new_user|
|*authentication_database_name*|指定使用者的鑑別資料庫。如果您未指定鑑別資料庫，`mongodump` 會假設它是您在 `--db` 選項中設定的資料庫。如果也未指定 `--db` 選項，`mongodump` 會假設它是 admin 資料庫。|admin|
|*database_name*|要將資料還原到其中的目標資料庫。如果資料庫不存在，`mongorestore` 會自動予以建立。如果您未指定資料庫，`mongorestore` 會根據原始資料庫來建立資料庫。|my_database|
|*cert_file*|憑證管理中心的 `.pem` 檔案。可以從實例概觀頁面下載該檔案，並使用相對或絕對路徑來指定該檔案。|./cert.pem|
|*dump_file*|從 Cloud {{site.data.keyword.cos_short}} 儲存區下載的 `.dump` 檔案。您可以使用相對或絕對路徑來指定檔案。|./mongo.dump|
{: caption="表 2. 從傾出檔案還原資料所需的變數"}

對於 {{site.data.keyword.mongodb}}，移轉會將原始資料合併到目標叢集，而不是改寫現有資料（如果有的話）。您可以在 {{site.data.keyword.mongodb}} 網站中參閱此特性的[詳細說明](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external}。您還可以參閱 [{{site.data.keyword.mongodb}} 文件](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external}，以取得 `mongorestore` 公用程式的相關資訊。

## 下一步為何？
{: #whats_next_backup_mongodb}

執行[步驟 3](#step3_restore_data_from_cos_mongodb) 之後，可以連接至資料庫叢集，以檢查資料還原是否順利。

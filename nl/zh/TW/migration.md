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

# 將 {{site.data.keyword.mongodb}} 資料庫移轉到 {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #migration_mongodb}

如果使用的是 {{site.data.keyword.mongodb}} 資料庫，並且要將其移轉到 {{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}}，請閱讀本主題以取得詳細資料。不管在何處管理 {{site.data.keyword.mongodb}} 資料庫，只要平台提供 {{site.data.keyword.mongodb}} 資料庫服務即可。
{: shortdesc}

## 開始之前
{: #migration_mongodb_before_begin}

若要使用 {{site.data.keyword.mongodb}} 指令來完成移轉，您需要先在機器上下載並安裝 {{site.data.keyword.mongodb}}。如需相關資訊，請參閱 [{{site.data.keyword.mongodb}} 網站](https://docs.mongodb.com/manual/installation/){: external}。

## 步驟 1：建立用於還原原始資料庫的傾出檔案
{: #step1_create_dump_file}

請使用下列 `mongodump` 指令來建立傾出檔案，其中包含您要還原的資料庫。

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_file>
```
{: pre}

下表說明指令中使用的變數。

|變數|說明|範例|
|---------|-----------|-------|
|*replica_set_name*|{{site.data.keyword.mongodb}} 抄本集名稱。如果叢集部署在 {{site.data.keyword.ihsdbaas_full}} Beta 實例中，這是建立實例時設定的叢集名稱。可以在叢集使用者介面的概觀頁面上，找到叢集 URI。名稱是 `replicaSet=` 之後的值。|MongoDB-001|
|*host_name1, host_name2, host_name3*|管理抄本集的主主伺服器名稱。有三個抄本可用。如果叢集部署在 {{site.data.keyword.ihsdbaas_full}} Beta 實例中，可以在概觀頁面上的叢集 URI 中找到主機名稱。|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|用來連接對應主機的埠。如果叢集部署在 {{site.data.keyword.ihsdbaas_full}} Beta 實例中，可以在概觀頁面上的叢集 URI 中找到埠號。|28205, 28016, 28175|
|*user_name*|用於向原始資料庫進行鑑別的使用者名稱。使用者需要對您要移轉的資料庫具有 READ 專用權。|my_user|
|*authentication_database_name*|指定使用者的鑑別資料庫。如果您未指定鑑別資料庫，`mongodump` 會假設它是您在 `--db` 選項中設定的資料庫。如果也未指定 `--db` 選項，`mongodump` 會假設它是 admin 資料庫。|admin|
|*database_name*|要移轉的資料庫。如果您未指定資料庫，`mongodump` 將傾出屬於該使用者的所有資料庫。|my_database|
|*cert_file*|憑證管理中心的 `.pem` 檔案。如果叢集部署在 {{site.data.keyword.ihsdbaas_full}} Beta 實例中，可以從概觀頁面下載憑證檔案。可以使用相對路徑或絕對路徑來指定該檔案。|./cert.pem|
|*dump_file*|用於儲存原始資料的 `.dump` 檔案。您可以使用相對或絕對路徑來指定檔案。|./mongo.dump|
{: caption="表 1. 建立傾出檔案所需的變數"}

如果需要有關 `mongodump` 公用程式的更多詳細資訊，可以參閱 [{{site.data.keyword.mongodb}} 文件](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}。

您可以從叢集 URI 取得大部分資訊。URI 的格式為 `mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>`。您可以讓該值與前一個 `mongodump` 指令中的對應欄位相符。
{: tip}

## 步驟 2：在 {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} 中建立新的服務實例
{: #step2_creat_new_service_instance}

在還原資料之前，需要在 {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} 中建立新的服務實例，以作為目標資料庫叢集。可以使用下列其中一種方式來建立新的實例：
- [Web 使用者介面](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service#dbaas_webui_create_service)
- [DBaaS 管理程式 API](https://{DomainName}/apidocs/hyperp-dbaas#create-an-ibm-cloud-service-instance-of-hyperprote){: external}
- [含 {{site.data.keyword.cloud_notm}} CLI 的 CLI 外掛程式](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_create_service)

建立新服務實例時，需要設定叢集名稱、管理者名稱和密碼。這些不必與原始實例中的相同。這不會影響移轉。移轉完成之後，會將資料庫指派給新的管理者/鑑別資料庫。

## 步驟 3：將資料從傾出檔案還原到新服務實例
{: #step3_restore_data}

使用 `mongorestore` 指令可從[步驟 1](#step1_create_dump_file) 中建立的傾出檔案還原資料。

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
|*database_name*|要將資料移轉到其中的目標資料庫。如果資料庫不存在，`mongorestore` 會自動予以建立。如果您未指定資料庫，`mongorestore` 會根據原始資料庫來建立資料庫。|my_database|
|*cert_file*|憑證管理中心的 `.pem` 檔案。可以從實例概觀頁面下載該檔案，並使用相對或絕對路徑來指定該檔案。|./cert.pem|
|*dump_file*|在[步驟 1](#step1_create_dump_file) 中建立的 `.dump` 檔案。您可以使用相對或絕對路徑來指定檔案。|./mongo.dump|
{: caption="表 2. 從傾出檔案還原資料所需的變數"}

您可以使用下列方式來建立新使用者：[Web 使用者介面](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas-webui-database-users#webui-create-database-user)、[DBaaS 管理程式 API](https://{DomainName}/apidocs/hyperp-dbaas#create-a-user-in-a-database-cluster){: external}、[{{site.data.keyword.cloud_notm}} CLI 的 CLI 外掛程式](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_plugin#user_create)以及 [{{site.data.keyword.mongodb}} 指令](https://docs.mongodb.com/manual/reference/method/db.createUser/index.html){: external}。
{: tip}

對於 {{site.data.keyword.mongodb}}，移轉會將原始資料合併到目標叢集，而不是改寫現有資料（如果有的話）。您可以在 {{site.data.keyword.mongodb}} 網站中參閱此特性的[詳細說明](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external}。您還可以參閱 [{{site.data.keyword.mongodb}} 文件](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external}，以取得 `mongorestore` 公用程式的相關資訊。

## 下一步為何？
{: #whats_next_migration_mongodb}

移轉之後，可以連接至新的資料庫叢集，以檢查資料移轉是否順利。

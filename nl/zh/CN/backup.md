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


# 使用 {{site.data.keyword.cos_full_notm}} 备份和复原数据库
{: #backup_mongodb_databases}

{{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}} 服务每 24 小时自动触发一次完整数据库备份。其中可用的是最近 7 天的加密备份，这些备份以冗余方式在受支持区域中所有可用性专区的本地存储器上提供。
{: shortdesc}

如果要提高灾难恢复能力，并备份受支持区域外部的数据，那么可以使用 [{{site.data.keyword.cos_full_notm}} 服务](https://cloud.ibm.com/catalog/services/cloud-object-storage){: external}通过参考以下步骤将数据存储在其他区域。

## 开始之前
{: #backup_mongodb_before_begin}

要使用 {{site.data.keyword.mongodb}} 命令来完成备份，需要首先在计算机上下载并安装 {{site.data.keyword.mongodb}}。有关更多信息，请参阅 [{{site.data.keyword.mongodb}} Web 站点](https://docs.mongodb.com/manual/installation/){: external}。

## 步骤 1：创建用于备份原始数据库的转储文件
{: #step1_create_dump_file_backup_mongodb}

使用以下 `mongodump` 命令来创建包含要备份的数据库的转储文件。

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_file>
```
{: pre}

下表说明了命令中使用的变量。

|变量|描述|示例|
|---------|-----------|-------|
|*replica_set_name*|{{site.data.keyword.mongodb}} 副本集名称。这是创建实例时设置的集群名称。可以在集群 UI 的概述页面上找到集群 URI。名称是 `replicaSet=` 后面的值。|MongoDB-001|
|*host_name1, host_name2, host_name3*|托管副本集的 DBaaS 管理器服务器。有三个副本可用。可以在概述页面上的集群 URI 中找到主机名。|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|用于连接相应主机的端口。可以在概述页面上的集群 URI 中找到端口号。|28205, 28016, 28175|
|*user_name*|用于向原始数据库认证的用户名。用户需要对要迁移的数据库具有 READ 特权。|my_user|
|*authentication_database_name*|特定用户的认证数据库。如果未指定认证数据库，那么 `mongodump` 会假定它是您在 `--db` 选项中设置的数据库。如果也未指定 `--db` 选项，那么 `mongodump` 会假定认证数据库是管理数据库。|admin|
|*database_name*|要备份的数据库。如果未指定数据库，那么 `mongodump` 将转储属于该用户的所有数据库。|my_database|
|*cert_file*|认证中心的 `.pem` 文件。可以从概述页面下载该证书文件，并使用相对或绝对路径来指定该文件。|./cert.pem|
|*dump_file*|用于存储原始数据的 `.dump` 文件。可以使用相对路径或绝对路径来指定该文件。|./mongo.dump|
{: caption="表 1. 创建转储文件所需的变量"}

如果需要有关 `mongodump` 实用程序的更详细信息，可参阅 [{{site.data.keyword.mongodb}} 文档](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}。

可以从集群 URI 获取大部分信息。URI 的格式为 `mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>`。可以将该值与之前 `mongodump` 命令中的相应字段匹配。
{: tip}

## 步骤 2：创建新的 {{site.data.keyword.cos_full_notm}} 实例
{: #step2_create_object_storage_backup_mongodb}

您创建该实例所在的区域需要与当前托管 {{site.data.keyword.ihsdbaas_mongodb_full}} 服务实例的位置不同。可以参阅以下步骤：

1. [创建新的 Cloud {{site.data.keyword.cos_short}} 实例](/docs/services/cloud-object-storage?topic=cloud-object-storage-provision)。
2. 在其他区域中[创建存储区](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-region)。
3. [创建服务凭证](/docs/services/cloud-object-storage?topic=cloud-object-storage-service-credentials)，并保存 `access_key_id` 和 `secret_access_key` 以供稍后使用。
4. 安装 S3 客户机。
5. 使用 S3 客户机和访问密钥来连接到先前创建的存储区的 Cloud {{site.data.keyword.cos_short}} 端点。
6. 使用 S3 客户机[上传 {{site.data.keyword.mongodb}} 备份文件](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload)（即在[步骤 1](#step1_create_dump_file_backup_mongodb) 中创建的备份文件）至存储区。

有关更多信息，请参阅 [{{site.data.keyword.cos_full_notm}} 文档](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started)。

完成此步骤后，您已成功在不同区域的 Cloud {{site.data.keyword.cos_short}} 实例中备份数据。如果要将数据从 Cloud {{site.data.keyword.cos_short}} 实例复原到 {{site.data.keyword.ihsdbaas_mongodb_full}} 实例，请阅读下面的步骤 3 以获取详细信息。

## 步骤 3：将数据从 Cloud {{site.data.keyword.cos_short}} 实例复原到 {{site.data.keyword.ihsdbaas_mongodb_full}} 实例
{: #step3_restore_data_from_cos_mongodb}

您需要先将 Cloud {{site.data.keyword.cos_short}} 存储区中的备份文件下载到本地计算机，然后使用 `mongorestore` 命令来复原数据。

```
mongorestore --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> <dump_file>
```
{: pre}

下表说明了命令中使用的变量。

|变量|描述|示例|
|---------|-----------|-------|
|*replica_set_name*|目标 {{site.data.keyword.mongodb}} 副本集名称。这是创建实例时设置的集群名称。可以在概述页面上找到集群 URI。名称是 `replicaSet=` 后面的值。|MongoDB-002|
|*host_name1, host_name2, host_name3*|托管目标副本集的 DBaaS 管理器服务器。有三个副本可用。可以在概述页面上的集群 URI 中找到主机名。|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|用于连接相应主机的端口。可以在概述页面上的集群 URI 中找到端口。|28128, 28248, 28043|
|*user_name*|用于向目标数据库认证的用户名。用户不必与创建转储文件的用户相同。|new_user|
|*authentication_database_name*|特定用户的认证数据库。如果未指定认证数据库，那么 `mongodump` 会假定它是您在 `--db` 选项中设置的数据库。如果也未指定 `--db` 选项，那么 `mongodump` 会假定认证数据库是管理数据库。|admin|
|*database_name*|要将数据复原到其中的目标数据库。如果该数据库不存在，`mongorestore` 会自动进行创建。如果未指定该数据库，`mongorestore` 会基于原始数据库创建一个数据库。|my_database|
|*cert_file*|认证中心的 `.pem` 文件。可以从实例概述页面下载该文件，并使用相对或绝对路径来指定该文件。|./cert.pem|
|*dump_file*|从 Cloud {{site.data.keyword.cos_short}} 存储区下载的 `.dump` 文件。可以使用相对路径或绝对路径来指定该文件。|./mongo.dump|
{: caption="表 2. 从转储文件复原数据所需的变量"}

对于 {{site.data.keyword.mongodb}}，迁移会将原始数据合并到目标集群，而不是覆盖现有数据（如果有的话）。可参阅 {{site.data.keyword.mongodb}} Web 站点中此功能的[详细说明](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external}。您还可以参阅 [{{site.data.keyword.mongodb}} 文档](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external}，以获取有关 `mongorestore` 实用程序的更多信息。

## 后续步骤
{: #whats_next_backup_mongodb}

执行[步骤 3](#step3_restore_data_from_cos_mongodb) 之后，可以连接到数据库集群，以检查数据复原是否成功。

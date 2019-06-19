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

# 将 {{site.data.keyword.mongodb}} 数据库迁移到 {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #migration_mongodb}

如果使用的是 {{site.data.keyword.mongodb}} 数据库，并且要将其迁移到 {{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}}，请阅读本主题以获取详细信息。不管在何处托管 {{site.data.keyword.mongodb}} 数据库，只要平台提供 {{site.data.keyword.mongodb}} 数据库服务即可。
{: shortdesc}

## 开始之前
{: #migration_mongodb_before_begin}

要使用 {{site.data.keyword.mongodb}} 命令来完成迁移，需要首先在计算机上下载并安装 {{site.data.keyword.mongodb}}。有关更多信息，请参阅 [{{site.data.keyword.mongodb}} Web 站点](https://docs.mongodb.com/manual/installation/){: external}。

## 步骤 1：创建用于复原原始数据库的转储文件
{: #step1_create_dump_file}

使用以下 `mongodump` 命令来创建包含要复原的数据库的转储文件。

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_file>
```
{: pre}

下表说明了命令中使用的变量。

|变量|描述|示例|
|---------|-----------|-------|
|*replica_set_name*|{{site.data.keyword.mongodb}} 副本集名称。如果集群部署在 {{site.data.keyword.ihsdbaas_full}} Beta 实例中，那么这是创建实例时设置的集群名称。可以在集群 UI 的概述页面上找到集群 URI。名称是 `replicaSet=` 后面的值。|MongoDB-001|
|*host_name1, host_name2, host_name3*|托管副本集的主机服务器的名称。有三个副本可用。如果集群部署在 {{site.data.keyword.ihsdbaas_full}} Beta 实例中，那么可以在概述页面上的集群 URI 中找到主机名。|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|用于连接相应主机的端口。如果集群部署在 {{site.data.keyword.ihsdbaas_full}} Beta 实例中，那么可以在概述页面上的集群 URI 中找到端口号。|28205, 28016, 28175|
|*user_name*|用于向原始数据库认证的用户名。用户需要对要迁移的数据库具有 READ 特权。|my_user|
|*authentication_database_name*|特定用户的认证数据库。如果未指定认证数据库，那么 `mongodump` 会假定它是您在 `--db` 选项中设置的数据库。如果也未指定 `--db` 选项，那么 `mongodump` 会假定认证数据库是管理数据库。|admin|
|*database_name*|要迁移的数据库。如果未指定数据库，那么 `mongodump` 将转储属于该用户的所有数据库。|my_database|
|*cert_file*|认证中心的 `.pem` 文件。如果集群部署在 {{site.data.keyword.ihsdbaas_full}} Beta 实例中，那么可以从概述页面下载证书文件。可以使用相对路径或绝对路径来指定该文件。|./cert.pem|
|*dump_file*|用于存储原始数据的 `.dump` 文件。可以使用相对路径或绝对路径来指定该文件。|./mongo.dump|
{: caption="表 1. 创建转储文件所需的变量"}

如果需要有关 `mongodump` 实用程序的更详细信息，可参阅 [{{site.data.keyword.mongodb}} 文档](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}。

可以从集群 URI 获取大部分信息。URI 的格式为 `mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>`。可以将该值与之前 `mongodump` 命令中的相应字段匹配。
{: tip}

## 步骤 2：在 {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} 中创建新的服务实例
{: #step2_creat_new_service_instance}

在复原数据之前，需要在 {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} 中创建新的服务实例，以作为目标数据库集群。可以使用下列其中一种方式来创建新实例：
- [Web 用户界面](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service#dbaas_webui_create_service)
- [DBaaS 管理器 API](https://{DomainName}/apidocs/hyperp-dbaas#create-an-ibm-cloud-service-instance-of-hyperprote){: external}
- [含 {{site.data.keyword.cloud_notm}} CLI 的 CLI 插件](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_create_service)

创建新服务实例时，需要设置集群名称、管理员名称和密码。这些信息不必与原始实例中的相同。这不会影响迁移。迁移完成后，会将数据库分配给新的管理员/认证数据库。

## 步骤 3：将数据从转储文件复原到新服务实例
{: #step3_restore_data}

使用 `mongorestore` 命令可从[步骤 1](#step1_create_dump_file) 中创建的转储文件复原数据。

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
|*database_name*|要将数据迁移到其中的目标数据库。如果该数据库不存在，`mongorestore` 会自动进行创建。如果未指定该数据库，`mongorestore` 会基于原始数据库创建一个数据库。|my_database|
|*cert_file*|认证中心的 `.pem` 文件。可以从实例概述页面下载该文件，并使用相对或绝对路径来指定该文件。|./cert.pem|
|*dump_file*|在[步骤 1](#step1_create_dump_file) 中创建的 `.dump` 文件。可以使用相对路径或绝对路径来指定该文件。|./mongo.dump|
{: caption="表 2. 从转储文件复原数据所需的变量"}

可以使用下列方式来创建新用户：[Web 用户界面](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas-webui-database-users#webui-create-database-user)、[DBaaS 管理器 API](https://{DomainName}/apidocs/hyperp-dbaas#create-a-user-in-a-database-cluster){: external}、
[含 {{site.data.keyword.cloud_notm}} CLI 的 CLI 插件](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_plugin#user_create)以及 [{{site.data.keyword.mongodb}} 命令](https://docs.mongodb.com/manual/reference/method/db.createUser/index.html){: external}。
{: tip}

对于 {{site.data.keyword.mongodb}}，迁移会将原始数据合并到目标集群，而不是覆盖现有数据（如果有的话）。可参阅 {{site.data.keyword.mongodb}} Web 站点中此功能的[详细说明](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external}。您还可以参阅 [{{site.data.keyword.mongodb}} 文档](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external}，以获取有关 `mongorestore` 实用程序的更多信息。

## 后续步骤
{: #whats_next_migration_mongodb}

迁移之后，可以连接到新的数据库集群，以检查数据迁移是否成功。

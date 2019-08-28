---

copyright:
  years: 2019
lastupdated: "2019-08-28"

keywords: migrate, restore

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Migrating {{site.data.keyword.mongodb}} databases to {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #migration_mongodb}

If you are using {{site.data.keyword.mongodb}} databases and want to migrate to {{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}}, read this topic for details. It does not matter where your {{site.data.keyword.mongodb}} databases are hosted, as long as the platform provides {{site.data.keyword.mongodb}} database services.
{: shortdesc}

## Before you begin
{: #migration_mongodb_before_begin}

To use the {{site.data.keyword.mongodb}} commands to complete the migration, you need to download and install {{site.data.keyword.mongodb}} on your machine first. For more information, see [{{site.data.keyword.mongodb}} website](https://docs.mongodb.com/manual/installation/){: external}.

## Step1: Create a dump file for restoring the original databases
{: #step1_create_dump_file}

Use the following `mongodump` command to create a dump file that contains the databases you want to restore.

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_path>
```
{: pre}

The following table illustrates the variables used in the command.

|Variables|Description|Example|
|---------|-----------|-------|
|*replica_set_name*|The {{site.data.keyword.mongodb}} replica set name. If the cluster is deployed in a {{site.data.keyword.ihsdbaas_full}} Beta instance, this is the cluster name you set when you create the instance. You can find the cluster URI on the overview page of the cluster UI. The name is the value after `replicaSet=`.|MongoDB-001|
|*host_name1, host_name2, host_name3*|The name of the host server that hosts the replica set. Three replicas are available. If the cluster is deployed in a {{site.data.keyword.ihsdbaas_full}} Beta instance, you can find the hostnames in the cluster URI on the overview page.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|The ports to connect the corresponding host. If the cluster is deployed in a {{site.data.keyword.ihsdbaas_full}} Beta instance, you can find the port numbers in the cluster URI on the overview page.|28205, 28016, 28175|
|*user_name*|The username to authenticate to the original databases. The user needs to have READ privilege on the database you want to migrate.|my_user|
|*authentication_database_name*|The authentication database for the specified user. If you do not specify an authentication database, `mongodump` assumes it to be the database that you set in `--db` option. If the `--db` option is also not specified, `mongodump` assumes it to be the admin database.|admin|
|*database_name*|The database you want to migrate. If you do not specify the database, `mongodump` dumps all the databases that belong to the user.|my_database|
|*cert_file*|The certificate authority (CA) file in the `.pem` format that contains SSL CA certificates. You can specify the file using relative or absolute paths. If the cluster is deployed in a Hyper Protect DBaaS Beta instance, you can select **Manage** on the left navigation bar of the instance dashboard, and download the CA file in the **Connect to Database** pane on the right side of the **Overview** tab page.|./cert.pem|
|*dump_path*|The path specifies the directory where `mongodump` will write a [BSON](https://docs.mongodb.com/manual/reference/program/mongodump/index.html#cmdoption-mongodump-out){: external} file for the dumped database. You will find the BSON file in a subdirectory with the name of *database_name*. You can use relative or absolute paths.|my_dump_path|
{: caption="Table 1. The variables that are needed to create a dump file"}

If you need more detailed information about `mongodump` utility, you can refer to [{{site.data.keyword.mongodb}} documentation](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}.

You can get most of the information from the cluster URI. The format of the URI is `mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>`. You can match the value to the corresponding field in the previous `mongodump` command.
{: tip}

## Step2: Create a new service instance in {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #step2_creat_new_service_instance}

Before you restore the data, you need to create a new service instance in {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} as the target database cluster. You can use one of the following ways to create a new instance:
- [The web user interface](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service#dbaas_webui_create_service)
- [The {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} RESTful APIs](https://{DomainName}/apidocs/hyperp-dbaas#create-an-ibm-cloud-service-instance-of-hyperprote){: external}
- [The CLI plug-in with the {{site.data.keyword.cloud_notm}} CLI](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_create_service)

When you create the new service instance, you need to set the cluster name, the administrator name and password. They are not necessarily to be the same as the ones in the original instance. It doesn't affect the migration. After the migration is completed, the databases are assigned to the new administrator/authentication database.

## Step3: Restore the data from the dump file to the new service instance
{: #step3_restore_data}

Use the `mongorestore` command to restore the data from the dump file that is created in [Step1](#step1_create_dump_file).

```
mongorestore --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> <dump_file_path>
```
{: pre}

The following table illustrates the variables used in the command.

|Variables|Description|Example|
|---------|-----------|-------|
|*replica_set_name*|The target {{site.data.keyword.mongodb}} replica set name. This is the cluster name you set when you create the instance. You can find the cluster URI on the overview page. The name is the value after `replicaSet=`.|MongoDB-002|
|*host_name1, host_name2, host_name3*|The DBaaS manager server that hosts the target replica set. Three replicas are available. You can find the hostnames in the cluster URI on the overview page.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|The ports to connect the corresponding host. You can find the ports in the cluster URI on the overview page.|28128, 28248, 28043|
|*user_name*|The username to authenticate to the target database. The user does not have to be the same with the user that creates the dump file.|new_user|
|*authentication_database_name*|The authentication database for the specified user. If you do not specify an authentication database, `mongodump` assumes it to be the database that you set in `--db` option. If the `--db` option is also not specified, `mongodump` assumes it to be the admin database.|admin|
|*database_name*|The target database that you want to migrate the data into. If the database does not exist, `mongorestore` creates it automatically. If you do not specify the database, `mongorestore` creates a database based on the original database.|my_database|
|*cert_file*|The CA file in the `.pem` format that contains SSL CA certificates. You can specify the file using relative or absolute paths. If the cluster is deployed in a Hyper Protect DBaaS Beta instance, you can select **Manage** on the left navigation bar of the instance dashboard, and download the CA file in the **Connect to Database** pane on the right side of the **Overview** tab page.|./cert.pem|
|*dump_file_path*|The path of the folder containing dump files you created in [Step1](#step1_create_dump_file), that is, *dump_path/database_name*. You can use relative or absolute paths.|my_dump_path/my_database|
{: caption="Table 2. The variables that are needed to restore the data from a dump file"}

You can use the following ways to create new users: [The web user interface](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas-webui-database-users#webui-create-database-user), [The {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} RESTful APIs](https://{DomainName}/apidocs/hyperp-dbaas#create-a-user-in-a-database-cluster){: external}, [The CLI plug-in with the {{site.data.keyword.cloud_notm}} CLI](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_plugin#user_create), and [{{site.data.keyword.mongodb}} command](https://docs.mongodb.com/manual/reference/method/db.createUser/index.html){: external}.
{: tip}

For {{site.data.keyword.mongodb}}, the migration merges the original data into the target cluster rather than overwriting the existing data if any. You can refer to the [detailed explanation](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external} of this feature in the {{site.data.keyword.mongodb}} website. You can also see [{{site.data.keyword.mongodb}} documentation](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external} for more information about the `mongorestore` utility.

## What's next
{: #whats_next_migration_mongodb}

After the migration, you can connect to the new database cluster to check whether the data is migrated successfully.

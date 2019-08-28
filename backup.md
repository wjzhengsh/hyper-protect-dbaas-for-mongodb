---

copyright:
  years: 2019
lastupdated: "2019-08-28"

keywords: backup, disaster recovery, restore

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:tip: .tip}


# Backing up and restoring your databases using {{site.data.keyword.cos_full_notm}}
{: #backup_mongodb_databases}

The {{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}} service automatically triggers a backup of your complete database once every 24 hours. These encrypted backups are available for the last 7 days and redundantly available on local storage in all the Availability Zones of the supported region.
{: shortdesc}

If you want to increase your disaster recovery capabilities and backup the data outside of the supported region, you can use [{{site.data.keyword.cos_full_notm}} service](https://cloud.ibm.com/catalog/services/cloud-object-storage){: external} to store your data in a different region by referring to the following steps.

## Before you begin
{: #backup_mongodb_before_begin}

To use the {{site.data.keyword.mongodb}} commands to complete the backup, you need to download and install {{site.data.keyword.mongodb}} on your machine first. For more information, see [{{site.data.keyword.mongodb}} website](https://docs.mongodb.com/manual/installation/){: external}.

## Step1: Create a dump file for backing up the original databases
{: #step1_create_dump_file_backup_mongodb}

Use the following `mongodump` command to create a dump file that contains the databases you want to back up.

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_file>
```
{: pre}

The following table illustrates the variables used in the command.

|Variables|Description|Example|
|---------|-----------|-------|
|*replica_set_name*|The {{site.data.keyword.mongodb}} replica set name. This is the cluster name that you set when you create the instance. You can find the cluster URI on the overview page of the cluster UI. The name is the value after `replicaSet=`.|MongoDB-001|
|*host_name1, host_name2, host_name3*|The DBaaS manager server that hosts the replica set. Three replicas are available. You can find the hostnames in the cluster URI on the overview page.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|The ports to connect the corresponding host. You can find the port numbers in the cluster URI on the overview page.|28205, 28016, 28175|
|*user_name*|The username to authenticate to the original databases. The user needs to have READ privilege on the database you want to migrate.|my_user|
|*authentication_database_name*|The authentication database for the specified user. If you do not specify an authentication database, `mongodump` assumes it to be the database that you set in `--db` option. If the `--db` option is also not specified, `mongodump` assumes it to be the admin database.|admin|
|*database_name*|The database you want to back up. If you do not specify the database, `mongodump` dumps all the databases that belong to the user.|my_database|
|*cert_file*|The `.pem` file for Certificate Authority. You can download the cert file from the overview page and specify the file using relative or absolute paths.|./cert.pem|
|*dump_file*|The `.dump` file to store the original data. You can use relative or absolute paths to specify the file.|./mongo.dump|
{: caption="Table 1. The variables that are needed to create a dump file"}

If you need more detailed information about `mongodump` utility, you can refer to [{{site.data.keyword.mongodb}} documentation](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}.

You can get most of the information from the cluster URI. The format of the URI is `mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>`. You can match the value to the corresponding field in the previous `mongodump` command.
{: tip}

## Step2: Create a new {{site.data.keyword.cos_full_notm}} instance
{: #step2_create_object_storage_backup_mongodb}

You need to create the instance in a region that is different from where the {{site.data.keyword.ihsdbaas_mongodb_full}} service instance is currently hosted. You can refer to the following steps:

1. [Create a new Cloud {{site.data.keyword.cos_short}} instance](/docs/services/cloud-object-storage?topic=cloud-object-storage-provision).
2. [Create a bucket](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-region) in a different region.
3. [Create a service credential](/docs/services/cloud-object-storage?topic=cloud-object-storage-service-credentials) and save the `access_key_id` and `secret_access_key` for later use.
4. Install a S3 client.
5. Use the S3 client and the access keys to connect to the Cloud {{site.data.keyword.cos_short}} endpoint of the bucket that you create earlier.
6. Use the S3 client to [upload the {{site.data.keyword.mongodb}} backup file](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) that you create in [Step1](#step1_create_dump_file_backup_mongodb) to the bucket.

You can refer to [{{site.data.keyword.cos_full_notm}} documentation](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started) for more information.

After completing this step, you successfully back up your data in a Cloud {{site.data.keyword.cos_short}} instance in a different region. If you want to restore the data from the Cloud {{site.data.keyword.cos_short}} instance to a {{site.data.keyword.ihsdbaas_mongodb_full}} instance, read the following Step3 for details.

## Step3: Restore the data from the Cloud {{site.data.keyword.cos_short}} instance to a {{site.data.keyword.ihsdbaas_mongodb_full}} instance
{: #step3_restore_data_from_cos_mongodb}

You need to download the backup file from the Cloud {{site.data.keyword.cos_short}} bucket to your local machine first, then use the `mongorestore` command to restore the data.

```
mongorestore --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> <dump_file>
```
{: pre}

The following table illustrates the variables used in the command.

|Variables|Description|Example|
|---------|-----------|-------|
|*replica_set_name*|The target {{site.data.keyword.mongodb}} replica set name. This is the cluster name you set when you create the instance. You can find the cluster URI on the overview page. The name is the value after `replicaSet=`.|MongoDB-002|
|*host_name1, host_name2, host_name3*|The DBaaS manager server that hosts the target replica set. Three replicas are available. You can find the hostnames in the cluster URI on the overview page.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|The ports to connect the corresponding host. You can find the ports in the cluster URI on the overview page.|28128, 28248, 28043|
|*user_name*|The username to authenticate to the target database. The user does not have to be the same as the user that creates the dump file.|new_user|
|*authentication_database_name*|The authentication database for the specified user. If you do not specify an authentication database, `mongodump` assumes it to be the database that you set in `--db` option. If the `--db` option is also not specified, `mongodump` assumes it to be the admin database.|admin|
|*database_name*|The target database that you want to restore the data into. If the database does not exist, `mongorestore` creates it automatically. If you do not specify the database, `mongorestore` creates a database based on the original database.|my_database|
|*cert_file*|The `.pem` file for Certificate Authority. You can download the file from the instance overview page and specify it using relative or absolute paths.|./cert.pem|
|*dump_file*|The `.dump` file that you download from your Cloud {{site.data.keyword.cos_short}} bucket. You can use relative or absolute paths to specify the file.|./mongo.dump|
{: caption="Table 2. The variables that are needed to restore the data from a dump file"}

For {{site.data.keyword.mongodb}}, the migration merges the original data into the target cluster rather than overwriting the existing data if any. You can refer to the [detailed explanation](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external} of this feature in the {{site.data.keyword.mongodb}} website. You can also see [{{site.data.keyword.mongodb}} documentation](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external} for more information about the `mongorestore` utility.

## What's next
{: #whats_next_backup_mongodb}

After [Step3](#step3_restore_data_from_cos_mongodb), you can connect to the database cluster to check if the data is restored successfully.

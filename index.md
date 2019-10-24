---

copyright:
  years: 2019
lastupdated: "2019-10-23"

keywords: Hyper Protect DBaaS, database, data security

subcollection: hyper-protect-dbaas-for-mongodb

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:codeblock: .codeblock}
{:tip: .tip}
{:pre: .pre}
{:note: .note}
{:external: target="_blank" .external}

# Getting started with {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #gettingstarted}

{{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}} is an {{site.data.keyword.cloud_notm}} service that provides {{site.data.keyword.mongodb}} databases on demand. It offers a flexible and scalable platform that allows you to quickly and easily provision and manage your database of choice.
{: shortdesc}

This {{site.data.keyword.cloud_notm}} offering provides {{site.data.keyword.mongodb}} database clusters. Each database cluster comprises one primary database instance and two secondary database instances (replicas that back up the primary).

With {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}, you can create database clusters in the {{site.data.keyword.cloud_notm}}, manage database instances, create and delete databases, manage user access, monitor databases, and view service logs.

## Supported version
{: #mongodb_supported_version}

{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} currently supports {{site.data.keyword.mongodb}} EE 3.6.4. It provides a secure, up-to-date version of the {{site.data.keyword.mongodb}} Enterprise database. We upgrade database maintenance versions `major.minor.maintenance` when appropriate.

For more information about versions, see [{{site.data.keyword.mongodb}} support policy](https://www.mongodb.com/support-policy){: external}.

## Prerequisite
{: #prerequisite}

Before you start, make sure you are using the [required browser software](/docs/overview?topic=overview-prereqs-platform) for {{site.data.keyword.cloud_notm}}.

For Safari, ensure that the **Prevent cross-site tracking** and **Block all cookies** options under **Safari > Preferences > Privacy** are not selected.

If you encounter problems using one of the required browsers, disable your browser plug-ins.

## Step 1: Creating a service instance
{: #creating-a-database-cluster-introduction}

When you create a service instance, you create a database cluster with one primary and two secondary database instances as replicas. 

If you haven't created a service instance yet, you can create one through [the web user interface](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service), [the CLI plug-in with the {{site.data.keyword.cloud_notm}} CLI tool](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-install-dbaas-cli-plugin), or [the {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} RESTful APIs](/apidocs/hyperp-dbaas){: external}.

Free plans are available. They are designed for evaluation purposes and are not suitable for production usage. If you create free-plan instances, note that they will be automatically deleted 30 days after creation.

##  Step 2: Managing the database cluster
{: #managing-database-cluster-introduction}

Each {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} cluster contains a DBaaS Manager, which manages and
intelligently schedules your requests based on the available resources.

In a database cluster, you can:
- Manage database instances
- Create and delete databases
- Manage database users
- Monitor databases
- View service logs

You can send the requests to the DBaaS Manager through one of the following interfaces:

- The [Web User Interface](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service)
- The [CLI plug-in with the {{site.data.keyword.cloud_notm}} CLI tool](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-install-dbaas-cli-plugin)
- The [{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} RESTful APIs](/apidocs/hyperp-dbaas){: external}

## Step 3: Accessing the databases
{: #accessing-database-introduction}

After creating a {{site.data.keyword.mongodb}} database, you can use the mongo shell, your favorite {{site.data.keyword.mongodb}} driver, or tools like {{site.data.keyword.mongodb}} Compass to access the database.

### Before you begin
{: #accessing-database-introduction-byb}

To ensure secure data transfer, obtain a certificate authority (CA) file from the dashboard, and copy it to the appropriate directory.

### Connecting to the databases
{: #accessing-database-introduction-connect}

The {{site.data.keyword.ihsdbaas_mongodb_full}} dashboard provides the necessary information to connect to the databases.

#### Using mongo shell
{: #accessing-database-introduction-connect-mongoshell}

The [mongo shell](https://docs.mongodb.com/manual/reference/program/mongo/index.html){: external} version needs to be compatible with MongoDB EE 3.6.4 that is supported by {{site.data.keyword.ihsdbaas_mongodb_full}}.

Run the mongo shell command that is provided in the {{site.data.keyword.ihsdbaas_mongodb_full}} dashboard. 

1. Click the icon next to the command to copy it to your clipboard.

2. If the secure data connection fails with an SSL error, specify the obtained CA file to validate the server certificate. Add the `--sslCAFile` parameter to indicate the CA file.

See the following example:

```
# mongo 'mongodb://<Hostname_1>:<PortNumber_1>,\
<Hostname_2>:<PortNumber_2>,\
<Hostname_3>:<PortNumber_3>/admin?replicaSet=<replicaSetName>' \
--ssl --username <Username> --password <Password> --sslCAFile <CAFile>
```
{: codeblock}

**Command options**

- *Hostname_i*

  The hostname of the database replica.

- *PortNumber_i*

  The port number of the database replica.

- *replicaSetName*

  The name of your returned replica set as specified in the {{site.data.keyword.ihsdbaas_mongodb_full}} dashboard.

- *Username*

  The user name for the Database admin as specified in the service creating screen.

- *Password*

  The password for the Database admin as specified in the service creating screen.

- *CAFile*

  The `cert.pem` file.

#### Using other tools
{: #accessing-database-introduction-connect-other}

For other tools, such as {{site.data.keyword.mongodb}} Compass, {{site.data.keyword.ihsdbaas_mongodb_full}} supports *SSL server certificate validation* to connect to the host. If needed, use the CA file from the service dashboard.

## Step 4 (Conditional): Migrating from {{site.data.keyword.mongodb}} databases
{: #migrating-from-mongodb}

To migrate from {{site.data.keyword.mongodb}} databases to {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}, follow the [migration instructions](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-migration_mongodb).

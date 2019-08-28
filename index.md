---

copyright:
  years: 2019
lastupdated: "2019-08-28"

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

This {{site.data.keyword.cloud_notm}} offering provides {{site.data.keyword.mongodb}} database clusters. Each database cluster comprises one primary database instance and two database instance replicas that back up the primary one (for the convenience of reference, the three instances can all be called replicas).

With {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}, you can create database clusters in the {{site.data.keyword.cloud_notm}}, manage database instances, administer database users, create, and monitor databases.

## Supported version
{: #mongodb_supported_version}

{{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}} currently supports {{site.data.keyword.mongodb}} 3.6. It provides a secure, up-to-date version of the {{site.data.keyword.mongodb}} Enterprise database. We upgrade database maintenance versions `major.minor.maintenance` when appropriate.

For more information about versions, see [{{site.data.keyword.mongodb}} support policy](https://www.mongodb.com/support-policy){: external}.

## Prerequisite
{: #prerequisite}

Before you start, make sure you are using the [required browser software](/docs/overview?topic=overview-prereqs-platform) for {{site.data.keyword.cloud_notm}}.

For Safari, ensure that the **Prevent cross-site tracking** and **Block all cookies** options under **Safari > Preferences > Privacy** are not selected.

If you encounter problems using one of the required browsers, disable your browser plug-ins.

## Data security and privacy
{: #data-security-and-privacy}

{{site.data.keyword.IBM_notm}} hosts your databases in a highly available and secure environment:
<ul>
<li>The underlying technologies prevent {{site.data.keyword.IBM_notm}} or a third party from being able to access your data.
<p>The {{site.data.keyword.IBM_notm}} Secure Service Container technology protects the system via a tamper-proof environment. Access to the system is restricted and is only enabled through well-defined RESTful APIs.</p></li>
<li>Data is encrypted at rest and in flight.</li>
<li>The system hardware, the system configuration, and the database setup ensure high availability.</li>
</ul>

<!--
For more information, watch:

- [Data security and privacy using {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} - English version](https://www.youtube.com/watch?v=__IBP727IL8){: external}
- [Data security and privacy using {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} - Chinese version](https://v.youku.com/v_show/id_XMzc3ODQzMzYwMA==.html){: external}
-->

## Creating a database cluster
{: #creating-a-database-cluster-introduction}

To create a database cluster, you simply enter the required values in the {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} service configuration screen and click **Create**.

After you have created the database cluster, {{site.data.keyword.IBM_notm}} provides you one or more hostnames and port
numbers of the created database instances. You can now use this information and the user credentials you specified in the catalog to create and access your databases.

## Managing the database cluster
{: #managing-database-cluster-introduction}

In a database cluster, you can create databases, manage the database instances, and create or delete users.

{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} contains a DBaaS Manager, which manages and
intelligently schedules your requests based on the available resources.

You can address requests to the DBaaS Manager through one of these interfaces:

- The [Web User Interface](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service)
- The [{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} RESTful APIs (for service instance management)](/apidocs/hyperp-dbaas){: external}
- The [CLI plug-in with the {{site.data.keyword.cloud_notm}} CLI tool](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-install-dbaas-cli-plugin)

## Accessing the database
{: #accessing-database-introduction}

After creating a {{site.data.keyword.mongodb}} database, you can use the mongo shell, your favorite {{site.data.keyword.mongodb}} driver, or tools like {{site.data.keyword.mongodb}} Compass to manage the database.

### Before you begin
{: #accessing-database-introduction-byb}

To ensure secure data transfer, obtain a certificate authority (CA) file from the dashboard, and copy it to the appropriate directory.

### Connecting to a database
{: #accessing-database-introduction-connect}

The {{site.data.keyword.ihsdbaas_mongodb_full}} dashboard provides the necessary information to connect to a database.

#### mongo shell
{: #accessing-database-introduction-connect-mongoshell}

You can run the mongo shell command that is provided at the {{site.data.keyword.ihsdbaas_mongodb_full}} dashboard:

1. Click the icon next to the command to copy it to your clipboard.
2. If the secure data connection fails with an SSL error, specify the obtained CA file to validate the server certificate. Add the `--sslCAFile` parameter to indicate the CA file.

See the following example:

<pre><code class="hljs"># mongo 'mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,\
&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,\
&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin?replicaSet=&lt;<em>replicaSetName</em>&gt;' \
--ssl --username &lt;<em>Username</em>&gt; --password &lt;<em>Password</em>&gt; --sslCAFile &lt;<em>CAFile</em>&gt;</code></pre>

Where:
<dl>
  <dt> &lt;<em>Hostname_i</em>&gt; </dt>
    <dd> Is the hostname of the database replica (<em>i=1,2,3</em>) </dd>
  <dt> &lt;<em>PortNumber_i</em>&gt; </dt>
    <dd> Is the port number of the database replica (<em>i=1,2,3</em>) </dd>
  <dt> &lt;<em>replicaSetName</em>&gt; </dt>
    <dd> Is the name of your returned replica set as specified in the {{site.data.keyword.ihsdbaas_mongodb_full}} dashboard </dd>
  <dt> &lt;<em>Username</em>&gt; </dt>
    <dd> Is the user name for the Database admin as specified in the service creating screen </dd>
  <dt> &lt;<em>Password</em>&gt; </dt>
    <dd> Is the password for the Database admin as specified in the service creating screen </dd>
  <dt> &lt;<em>CAFile</em>&gt; </dt>
    <dd> Is the CA file cert.pem </dd>
</dl>


### Other tools
{: #accessing-database-introduction-connect-other}

For other tools, such as {{site.data.keyword.mongodb}} Compass, {{site.data.keyword.ihsdbaas_mongodb_full}} supports *SSL server certificate validation* to connect to the host. If needed, use the provided CA file.

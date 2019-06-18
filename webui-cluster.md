---

copyright:
  years: 2019
lastupdated: "2019-06-12"

keywords: database cluster, service instance, pricing plans, License Agreement

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}


# Service instances
{: #dbaas_webui_service}

## Creating a service instance
{: #dbaas_webui_create_service}

<ol>
<li>Click the {{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}} catalog entry to open the service creating screen.</li>
<li>Enter the required values.
  <dl>
    <dt>Service name:</dt>
    <dd>A name for the database service.</dd>

    <dt>Choose a region/location to deploy in:</dt>
    <dd>Select the data center where the database will be located.</dd>

    <dt>Select a resource group:</dt>
    <dd>If no resource group is selectable, you can create one on the {{site.data.keyword.cloud_notm}} dashboard.</dd>

    <dt>Tags:</dt>
    <dd>Tags are optional. See [Tagging resources](/docs/resources?topic=resources-tag#tag) for more information about tagging.</dd>

    <dt>Cluster/Replicaset Name:</dt>
    <dd>A name for the service instance.</dd>

    <dt>Database Admin Name:</dt>
    <dd>A user ID for the database administrator (DBA).</dd>

    <dt>Database Admin Password:</dt>
    <dd>
    <p>A password for the DBA user ID. You need to create a strong password with the following attributes:
      <ul>
        <li>minimum of **15 characters** in length</li>
        <li>at least **one uppercase character**</li>
        <li>at least **one lowercase character**</li>
        <li>at least **one number**</li>
        <li>does not contain user name</li>
      </ul>
    </p>
    </dd>

    <dt>Confirm Password:</dt>
    <dd>Confirm the password for the DBA user ID.</dd>

    <dt>License Agreement:</dt>
    <dd>After reading the license agreement, check the box to confirm your agreement.</dd>

    <dt>Pricing Plans:</dt>
    <dd>The pricing plans offer different pricing categories for databases of type MongoDB. Choose the one that best meets your needs.</dd>
  </dl>
</li>

<li>Click **Create**.
<p>After a dialog window that tells you that it takes time to deploy the service, the {{site.data.keyword.cloud_notm}} Resource List dashboard is displayed. You can see the service instance that you create under **Services** section in the list. When the status of the service is **Provisioned**, the instance is ready to use.</p>
</li>

<li>Select the service to display the {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} dashboard.</li>
</ol>

To even further strengthen security, it is suggested that you update the **database admin password** immediately after the service instance is provisioned. You need to follow the same rules that are previously mentioned to set the new password.
{: note}

## Listing all service instances
{: #dbaas_webui_list_service}

Open the {{site.data.keyword.cloud_notm}} dashboard to display a list of created service instances:

<ol>
	<li>Click the three bars button in the upper left of the console.</li>
	<li>Select Dashboard.</li>
	<li>Select Services.</li>
</ol>

## Showing detail information of a service instance
{: #dbaas_webui_show_detail_service}

1. Click the target instance name to display the service dashboard.
2. Select **Manage** on the left navigation bar and you can see the overall information on the **Overview** tab page.
3. Select the **Instances** tab to display the detailed information.

## Deleting a service instance
{: #dbaas_webui_delete_service}

1. Display the {{site.data.keyword.cloud_notm}} resource list by clicking **Navigation Menu > Resource List**.
2. Open the **Service** twister to display your service instances.
3. Select the service instance you want to delete.
4. Click the three dots on the right.
5. Click **Delete**.

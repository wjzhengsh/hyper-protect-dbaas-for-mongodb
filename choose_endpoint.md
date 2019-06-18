---

copyright:
  years: 2019
lastupdated: "2019-06-10"

keywords: service endpoint, ibmcloud regions

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}


# Choosing a service endpoint to log in to
{: #choose-endpoint}

{{site.data.keyword.cloud}} provides many service endpoints in various geographical regions that you can log in to.
To find a service endpoint near your geographical location, use the `ibmcloud regions` command as shown in this example:

<pre><code class="hljs">~$ ibmcloud regions
Listing regions...

Name       Geolocation      Customer   Deployment   Type
au-syd     Sydney           IBM        Production   public
jp-tok     AP North
eu-de      Germany          IBM        Production   public
eu-gb      United Kingdom   IBM        Production   public
us-east    US East          IBM        Production   public
us-south   US South         IBM        Production   public

</code></pre>

Currently, {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} is supported only on the **us-south** region.
{: note}

To log in to an {{site.data.keyword.cloud_notm}} service endpoint, follow these steps:

1. Enter the `ibmcloud login` command, indicating that you are using Single Sign-On (SSO) and specifying the URL of the endpoint you want to log in to, as shown in this example:

   ```javascript
   ibmcloud login --sso -a https://api.cloud.ibm.com
   ```
   {:codeblock}

   The system displays a URL for a web page that provides you with a one-time passcode.

2. Copy the URL from your system console and paste it into your web browser.

   The system displays the **{{site.data.keyword.cloud_notm}} One time passcode** page.

3. Copy the passcode from the web page and paste it into your system console command line.

   The passcode is not displayed in the command line. When the password is authenticated, you receive an **OK** message indicating that you are logged in.

For more information about the `ibmcloud login` command parameters, you can see [{{site.data.keyword.cloud_notm}} CLI reference](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_login).

When you use {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}, you might need to create spaces in more than one region and switch between regions by using the `ibmcloud target` command. For more information, see [Adding orgs and spaces](/docs/account?topic=account-orgsspacesusers#orgsspacesusers)
and [General CLI (ibmcloud) commands](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#bluemix_target).
{: note}

---

copyright:
  years: 2019
lastupdated: "2019-06-05"

keywords: ibmcloud resource, service instance, CLI tool

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}


# Deleting a service instance
{: #dbaas_cli_delete_service}

To review which services you have created, use this command:

<pre><code class="hljs">~$ ibmcloud resource service-instances
</code></pre>

If you want to delete one of them, use the `ibmcloud resource service-instance-delete` command, as shown in this example:

<pre><code class="hljs">~$ ibmcloud resource service-instance-delete MyDBaaSIns03
</code></pre>

Confirm the deletion by typing **y**.

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


# Serviceinstanz löschen
{: #dbaas_cli_delete_service}

Verwenden Sie den folgenden Befehl, um zu überprüfen, welche Services Sie erstellt haben:

<pre><code class="hljs">~$ ibmcloud resource service-instances
</code></pre>

Wenn Sie eine der Instanzen löschen möchten, verwenden Sie den Befehl `ibmcloud resource service-instance-delete`, wie in diesem Beispiel dargestellt:

<pre><code class="hljs">~$ ibmcloud resource service-instance-delete MyDBaaSIns03
</code></pre>

Bestätigen Sie den Löschvorgang, indem Sie **y** eingeben.

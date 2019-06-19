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


# Serviceendpunkt für die Anmeldung auswählen
{: #choose-endpoint}

{{site.data.keyword.cloud}} stellt viele Serviceendpunkte in verschiedenen geografischen Regionen bereit, an denen Sie sich anmelden können.
Um einen Serviceendpunkt in der Nähe Ihres Standorts zu finden, verwenden Sie den Befehl `ibmcloud regions`, wie in diesem Beispiel dargestellt:

<pre><code class="hljs">~$ ibmcloud regions
Regionen werden aufgelistet ...

Name    Geoortung      Kunde     Bereitstellung   Typ
au-syd     Sydney           IBM        Produktion   öffentlich
jp-tok     AP (Norden)
eu-de      Deutschland      IBM        Produktion   öffentlich
eu-gb      Großbritannien   IBM        Produktion   öffentlich
us-east    USA (Osten)      IBM        Produktion   öffentlich
us-south   USA (Süden)      IBM        Produktion   öffentlich

</code></pre>

Derzeit wird {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} nur in der Region **us-south** unterstützt.
{: note}

Führen Sie die folgenden Schritte aus, um sich an einem {{site.data.keyword.cloud_notm}}-Serviceendpunkt anzumelden:

1. Geben Sie den Befehl `ibmcloud login` ein, der angibt, dass Sie Single Sign-On (SSO) verwenden, und gleichzeitig die URL des Endpunkts anzeigt, an dem Sie sich anmelden möchten, wie in diesem Beispiel gezeigt:

   ```javascript
   ibmcloud login --sso -a https://api.cloud.ibm.com
   ```
   {:codeblock}

   Das System zeigt eine URL für eine Webseite an, die Ihnen einen einmaligen Kenncode zur Verfügung stellt.

2. Kopieren Sie die URL von Ihrer Systemkonsole und fügen Sie sie in Ihren Web-Browser ein.

   Das System zeigt die Seite für den **einmaligen {{site.data.keyword.cloud_notm}}-Kenncode** an.

3. Kopieren Sie den Kenncode von der Webseite und fügen Sie ihn in die Befehlszeile Ihrer Systemkonsole ein.

   Der Kenncode wird in der Befehlszeile nicht angezeigt. Wenn das Kennwort authentifiziert wird, empfangen Sie eine Nachricht zur **Bestätigung**, dass Sie angemeldet sind.

Weitere Informationen zu den Befehlsparametern von `ibmcloud login` finden Sie in den [Referenzinformationen zur {{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_login).

Bei der Verwendung von {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} müssen Sie möglicherweise Bereiche in mehr als einer Region erstellen und mithilfe des Befehls `ibmcloud target` zwischen Regionen wechseln. Weitere Informationen finden Sie in den Abschnitten zum [Hinzufügen von Organisationen und Bereichen](/docs/account?topic=account-orgsspacesusers#orgsspacesusers)
und zu [allgemeinen CLI-Befehlen (ibmcloud)](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#bluemix_target).
{: note}

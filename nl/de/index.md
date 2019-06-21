---

copyright:
  years: 2019
lastupdated: "2019-06-10"

keywords: database cluster, Data security, database instance

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}

# Einführung in {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #gettingstarted}

{{site.data.keyword.cloud}}{{site.data.keyword.ihsdbaas_mongodb_full}} ist ein {{site.data.keyword.cloud_notm}}-Service, der bei Bedarf MongoDB-Datenbanken bereitstellt. Der Service bietet eine flexible und skalierbare Plattform, die es Ihnen ermöglicht, Ihre Datenbank
schnell und einfach bereitzustellen und zu verwalten.
{: shortdesc}

Dieses {{site.data.keyword.cloud_notm}}-Angebot stellt MongoDB-Datenbankcluster bereit. Jeder Datenbankcluster
umfasst eine primäre Datenbankinstanz und zwei Datenbankinstanzreplikate,
die die primäre Datenbank sichern.

Mit {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} können Sie Datenbankcluster in {{site.data.keyword.cloud_notm}} erstellen,
Datenbankinstanzen und Datenbankbenutzer verwalten und Datenbanken erstellen und
überwachen.

## Datensicherheit und Datenschutz
{: #data-security-and-privacy}

{{site.data.keyword.IBM_notm}} dient als Host für Ihre Datenbanken in einer hoch verfügbaren und sicheren Umgebung:
<ul>
<li>Die zugrunde liegenden Technologien verhindern, dass {{site.data.keyword.IBM_notm}} oder ein anderer Anbieter auf Ihre
Daten zugreifen kann.
<p>Die {{site.data.keyword.IBM_notm}} Secure Service Container-Technologie schützt das System über eine
manipulationssichere Umgebung. Der Zugriff auf das System ist eingeschränkt und wird nur
über klar definierte RESTful-APIs aktiviert.</p></li>
<li>Es werden ruhende und momentan ausgeführte Daten verschlüsselt.</li>
<li>Die Systemhardware, die Systemkonfiguration und die Datenbankkonfiguration stellen
eine hohe Verfügbarkeit sicher.</li>
</ul>

<!--
For more information, watch:

- [Data security and privacy using {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} - English version](https://www.youtube.com/watch?v=__IBP727IL8){: external}
- [Data security and privacy using {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} - Chinese version](https://v.youku.com/v_show/id_XMzc3ODQzMzYwMA==.html){: external}
-->

## Datenbankcluster erstellen
{: #creating-a-database-cluster-introduction}

Wenn Sie einen Datenbankcluster erstellen möchten, geben Sie einfach die erforderlichen Werte in die Anzeige für die
{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}-Servicekonfiguration ein und klicken Sie auf **Erstellen**.

Nachdem Sie den Datenbankcluster erstellt haben, stellt {{site.data.keyword.IBM_notm}} Ihnen einen oder mehrere Hostnamen und Portnummern
der erstellten Datenbankinstanzen zur Verfügung. Sie können nun diese Informationen
und die Benutzerberechtigungen verwenden, die Sie im Katalog angegeben haben, um Ihre
Datenbanken zu erstellen und auf diese zuzugreifen.

## Datenbankcluster verwalten
{: #managing-database-cluster-introduction}

In einem Datenbankcluster können Sie Datenbanken erstellen, die Datenbankinstanzen verwalten und
Benutzer erstellen oder löschen.

{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} enthält einen DBaaS-Manager, der Ihre Anforderungen
basierend auf den verfügbaren Ressourcen verwaltet und intelligent plant.

Sie können Anforderungen an den DBaaS-Manager über eine der folgenden Schnittstellen verarbeiten:

- Die [Webbenutzerschnittstelle](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service)
- Die [DBaaS-Manager-APIs (für das Datenbankcluster-Management)](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-gen_inst_mgr_apis)
- Die [{{site.data.keyword.cloud_notm}}-APIs (für die Verwaltung von Serviceinstanzen)](https://{DomainName}/apidocs/hyperp-dbaas){: external}
- Das [CLI-Plug-in mit dem {{site.data.keyword.cloud_notm}}-CLI-Tool](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-install-ibm-cli)

## Zugriff auf die Datenbank
{: #accessing-database-introduction}

Nachdem Sie eine MongoDB-Datenbank erstellt haben, können Sie die Mongo-Shell, Ihren bevorzugten MongoDB-Treiber oder Tools wie MongoDB Compass verwenden, um die Datenbank zu verwalten.

### Vorbereitende Schritte
{: #accessing-database-introduction-byb}

Um die sichere Datenübertragung sicherzustellen, müssen Sie eine CA-Datei (CA = Certificate Authority, Zertifizierungsstelle)
aus dem Dashboard abrufen und in das entsprechende Verzeichnis kopieren.

### Verbindung zu einer Datenbank herstellen
{: #accessing-database-introduction-connect}

Das {{site.data.keyword.ihsdbaas_mongodb_full}}-Dashboard stellt die erforderlichen Informationen zum Herstellen einer Verbindung zu einer Datenbank bereit.

#### Mongo-Shell
{: #accessing-database-introduction-connect-mongoshell}

Sie können den Mongo-Shellbefehl ausführen, der im {{site.data.keyword.ihsdbaas_mongodb_full}}-Dashboard bereitgestellt wird:

1. Klicken Sie auf das Symbol neben dem Befehl, um ihn in die Zwischenablage zu kopieren.
2. Wenn die sichere Datenverbindung mit einem SSL-Fehler fehlschlägt, geben Sie die erhaltene CA-Datei an, um das Serverzertifikat zu überprüfen. Fügen Sie den Parameter `--sslCAFile` hinzu, um die CA-Datei anzugeben.

Beispiel:

<pre><code class="hljs"># mongo 'mongodb:/&sol;&lt;<em>hostname_1</em>&gt;&colon;&lt;<em>portnummer_1</em>&gt;,\
&lt;<em>hostname_2</em>&gt;&colon;&lt;<em>portnummer_2</em>&gt;,\
&lt;<em>hostname_3</em>&gt;&colon;&lt;<em>portnummer_3</em>&gt;/admin?replicaSet=&lt;<em>name_der_replikatgruppe</em>&gt;' \
--ssl --username &lt;<em>benutzername</em>&gt; --password &lt;<em>kennwort</em>&gt; --sslCAFile &lt;<em>ca-datei</em>&gt;</code></pre>

Dabei gilt Folgendes:
<dl>
  <dt> &lt;<em>hostname_i</em>&gt; </dt>
    <dd> Ist der Hostname des Datenbankreplikats (<em>i=1,2,3</em>) </dd>
  <dt> &lt;<em>portnummer_i</em>&gt; </dt>
    <dd> Gibt die Portnummer des Datenbankreplikats an (<em>i=1,2,3</em>) </dd>
  <dt> &lt;<em>name_der_replikatgruppe</em>&gt; </dt>
    <dd> Ist der Name der zurückgegebenen Replikatgruppe, wie im {{site.data.keyword.ihsdbaas_mongodb_full}}-Dashboard angegeben </dd>
  <dt> &lt;<em>benutzername</em>&gt; </dt>
    <dd> Der Benutzername für den Datenbankadministrator, der in der Serviceerstellungsanzeige angegeben wurde. </dd>
  <dt> &lt;<em>kennwort</em>&gt; </dt>
    <dd> Das Kennwort für den Datenbankadministrator, das in der Serviceerstellungsanzeige angegeben wurde. </dd>
  <dt> &lt;<em>ca-datei</em>&gt; </dt>
    <dd> Ist die CA-Datei 'cert.pem' </dd>
</dl>


### Andere Tools
{: #accessing-database-introduction-connect-other}

Für andere Tools, wie z. B. MongoDB Compass, unterstützt {{site.data.keyword.ihsdbaas_mongodb_full}} *die SSL-Serverzertifikatsprüfung*, um eine Verbindung zum Host herzustellen. Falls erforderlich, verwenden Sie die bereitgestellte CA-Datei.

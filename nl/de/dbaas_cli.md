---

copyright:
  years: 2019
lastupdated: "2019-06-10"

keywords: instance commands, cluster resource, CLI plugin

subcollection: hyper-protect-dbaas-for-mongodb

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}


# {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}-CLI-Plug-in
{: #dbaas_cli_plugin}

- Installieren Sie die [{{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle](/docs/cli?topic=cloud-cli-getting-started). Die {{site.data.keyword.cloud_notm}}-CLI erfordert Java SDK 1.7.0. Das Präfix zum Ausführen von Befehlen über die {{site.data.keyword.cloud_notm}}-CLI ist `ibmcloud`. Im Terminal werden Sie benachrichtigt, wenn Aktualisierungen an der `ibmcloud`-CLI und den Plug-ins verfügbar sind. Stellen Sie sicher, dass Ihre CLI immer auf dem neusten Stand ist, damit Sie alle verfügbaren Befehle und Flags verwenden können.

- Installieren Sie das {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}-Befehlszeilenschnittstellen-Plug-in. [DBaaS-Befehlszeilenschnittstellen-Plug-in installieren](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-install-dbaas-cli-plugin) enthält Referenzinformationen hierzu. Wenn Sie die aktuelle Version des {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}-CLI-Plug-ins
anzeigen möchten, führen Sie den Befehl `ibmcloud plugin show dbaas-cli` aus.

## Befehl zur Nutzung des CLI-Plug-ins
{: #plugin_use}

### `ibmcloud dbaas help`
{: #display_list}

Mit diesem Befehl wird eine Liste der DBaaS-Befehle angezeigt.

```
ibmcloud help dbaas
```
{: pre}

## Clusterbefehl
{: #cluster_cmds}

### `ibmcloud dbaas cluster-show`
{: #cluster_show}

Mit diesem Befehl werden die Details des angegebenen Clusters angezeigt, einschließlich Informationen zu jedem Replikat-Member.  

```
ibmcloud dbaas cluster-show <ressourcenname>
```
{: pre}

**Befehlsoptionen**

<dl>
<dt>*Ressourcenname*</dt>
<dd>Der Name der Clusterressource. Um den Ressourcennamen zu ermitteln, verwenden Sie den {{site.data.keyword.cloud_notm}}-Befehl `ibmcloud resource service-instances`.</dd>
</dl>

## Datenbankbefehle
{: #db_cmds}

### `ibmcloud dbaas database-create`
{: #db_create}

Mit diesem Befehl werden eine Datenbank und eine Datensammlung erstellt. Für {{site.data.keyword.mongodb}} muss eine Datensammlung zusammen mit der Datenbank erstellt werden.

```
ibmcloud dbaas database-create <ressourcenname> <datenbankname> <datensammlung>
```
{: pre}

**Befehlsoptionen**

<dl>
<dt>*Ressourcenname*</dt>
<dd>Der Name der Clusterressource.</dd>
<dt>*Datenbankname*</dt>
<dd>Der Name der zu erstellenden Datenbank.</dd>
<dt>*Datensammlung*</dt>
<dd>Der Name der zu erstellenden Datensammlung.</dd>
</dl>


### `ibmcloud dbaas database-delete`
{: #db_delete}

Mit diesem Befehl wird eine Datenbank gelöscht. Löschen Sie eine Datenbank nicht, wenn diese Datenbank anhand eines Berechtigungsnachweises erstellt wird und dieser Berechtigungsnachweis noch im Gebrauch ist.

```
ibmcloud dbaas database-delete <ressourcenname> <datenbankname>
```
{: pre}

**Befehlsoptionen**

<dl>
<dt>*Ressourcenname*</dt>
<dd>Der Name der Clusterressource.</dd>
<dt>*Datenbankname*</dt>
<dd>Der Name der Datenbank, die gelöscht werden soll.</dd>
</dl>


### `ibmcloud dbaas databases-list`
{: #db_list}

Mit diesem Befehl werden alle Datenbanken auf dem angegebenen Cluster aufgelistet.

```
ibmcloud dbaas databases-list <ressourcenname>
```
{: pre}

**Befehlsoptionen**

<dl>
<dt>*Ressourcenname*</dt>
<dd>Der Name der Clusterressource.</dd>
</dl>


## Datenbankbenutzerbefehle
{: #user_cmds}

### `ibmcloud dbaas user-create`
{: #user_create}

Mit diesem Befehl wird ein Datenbankbenutzer erstellt.

```
ibmcloud dbaas user-create <ressourcenname> <auth.datenbankbenutzername> <kennwort> [<datenbankname> [<datenbankname> [...]]]
```
{: pre}

**Befehlsoptionen**

<dl>
<dt>*Ressourcenname*</dt>
<dd>Der Name der Clusterressource.</dd>
<dt>*auth.datenbankbenutzername*</dt>
<dd>Der Name der Authentifizierungsdatenbank und der Benutzername (durch einen Punkt voneinander getrennt), die dem zu erstellenden Datenbankbenutzer zugeordnet werden.</dd>
<dt>*Datenbankname*</dt>
<dd>(Optional) Gibt eine Datenbank an, für die der Benutzer über Lese- und Schreibzugriff verfügt.</dd>
</dl>


### `ibmcloud dbaas user-delete`
{: #user_delete}

Mit diesem Befehl wird ein Datenbankbenutzer gelöscht. Löschen Sie einen Datenbankbenutzer nicht, wenn dieser Datenbankbenutzer anhand eines Berechtigungsnachweises erstellt wird und dieser Berechtigungsnachweis noch im Gebrauch ist.

```
ibmcloud dbaas user-delete <ressourcenname> <auth.datenbankbenutzername>
```
{: pre}

**Befehlsoptionen**

<dl>
<dt>*Ressourcenname*</dt>
<dd>Der Name der Clusterressource.</dd>
<dt>*auth.datenbankbenutzername*</dt>
<dd>Der Name der Authentifizierungsdatenbank und der Benutzername (durch einen Punkt voneinander getrennt) des Datenbankbenutzers, der gelöscht wird.</dd>
</dl>

### `ibmcloud dbaas users-list`
{: #user_list}

Mit diesem Befehl werden alle Datenbankbenutzer aufgelistet.

```
ibmcloud dbaas users-list <ressourcenname>
```
{: pre}

**Befehlsoptionen**

<dl>
<dt>*Ressourcenname*</dt>
<dd>Der Name der Clusterressource.</dd>
</dl>


### `ibmcloud dbaas user-show`
{: #user_show}

Mit diesem Befehl werden Details zu einem Datenbankbenutzer angezeigt.

```
ibmcloud dbaas user-show <ressourcenname> <auth.datenbankbenutzername>
```
{: pre}

**Befehlsoptionen**

<dl>
<dt>*Ressourcenname*</dt>
<dd>Der Name der Clusterressource.</dd>
<dt>*auth.datenbankbenutzername*</dt>
<dd>Der Name der Authentifizierungsdatenbank und der Benutzername (durch einen Punkt voneinander getrennt) des Datenbankbenutzers, der angezeigt wird.</dd>
</dl>


## Instanzbefehle
{: #instance_cmds}

Wenn Sie einen der hier aufgeführten Instanzbefehle verwenden möchten, müssen Sie die Instanz-ID (`instance_id`) kennen. Zum Abrufen des Werts für `instance_id` verwenden Sie zuerst den `ibmcloud`-Befehl zum Auflisten der Namen der Ressourcengruppen für die Serviceinstanzen unter Ihrem Konto und anschließend den Befehl [`cluster_show`](#cluster_show), um den Wert für `instance_id` abzurufen.

```
ibmcloud resource service-instances
```
{: pre}


### `ibmcloud dbaas instance-restart`
{: #instance_restart}

Mit diesem Befehl wird eine aktive Instanz erneut gestartet.

```
ibmcloud dbaas instance-restart <ressourcenname> <instanz-id>
```
{: pre}

**Befehlsoptionen**

<dl>
<dt>*Ressourcenname*</dt>
<dd>Der Name der Clusterressource.</dd>
<dt>*Instanz-ID*</dt>
<dd>Die ID der Instanz.</dd>
</dl>


### ibmcloud dbaas instance-start
{: #instance_start}

Mit diesem Befehl wird eine gestoppte Instanz gestartet.

```
ibmcloud dbaas instance-start <ressourcenname> <instanz-id>
```
{: pre}

**Befehlsoptionen**

<dl>
<dt>*Ressourcenname*</dt>
<dd>Der Name der Clusterressource.</dd>
<dt>*Instanz-ID*</dt>
<dd>Die ID der Instanz.</dd>
</dl>


### `ibmcloud dbaas instance-stop`
{: #instance_stop}

Mit diesem Befehl wird eine Instanz gestoppt (ein Replikationsmember des Clusters).

```
ibmcloud dbaas instance-stop <ressourcenname> <instanz-id>
```
{: pre}

**Befehlsoptionen**

<dl>
<dt>*Ressourcenname*</dt>
<dd>Der Name der Clusterressource.</dd>
<dt>*Instanz-ID*</dt>
<dd>Die ID der Instanz.</dd>
</dl>


## Protokollbefehle
{: log_cmds}

### `ibmcloud dbaas log-get`
{: #log_get}

Mit diesem Befehl wird eine Protokolldatei von einer Instanz heruntergeladen.

```
ibmcloud dbaas log-get <ressourcenname> <instanz-id> <dateiname>
```
{: pre}

**Befehlsoptionen**

<dl>
<dt>*Ressourcenname*</dt>
<dd>Der Name der Clusterressource.</dd>
<dt>*Instanz-ID*</dt>
<dd>Die ID der Instanz.</dd>
<dt>*Dateiname*</dt>
<dd>Der Name der Protokolldatei, die heruntergeladen werden soll. Um den Dateinamen der Protokolldatei zu ermitteln, die Sie herunterladen möchten, verwenden Sie den Befehl [ibmcloud dbaas logs-list](#log_list).</dd>
</dl>

### `ibmcloud dbaas logs-list`
{: #log_list}

Mit diesem Befehl werden alle Protokolldateien in einer Instanz aufgelistet. Sie können jeden der aufgelisteten Dateinamen als Eingabe für den Befehl [ibmcloud dbaas log-get](#log_get) verwenden.

```
ibmcloud dbaas logs-list <ressourcenname> <instanz-id>
```
{: pre}

**Befehlsoptionen**

<dl>
<dt>*Ressourcenname*</dt>
<dd>Der Name der Clusterressource.</dd>
<dt>*Instanz-ID*</dt>
<dd>Die ID der Instanz.</dd>
</dl>

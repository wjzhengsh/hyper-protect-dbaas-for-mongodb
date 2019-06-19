---

copyright:
  years: 2019
lastupdated: "2019-06-12"

keywords: migrate, beta, restore

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Migration von {{site.data.keyword.mongodb}}-Datenbanken auf {{site.data.keyword.cloud_notm}}{{site.data.keyword.ihsdbaas_mongodb_full}}
{: #migration_mongodb}

Wenn Sie {{site.data.keyword.mongodb}}-Datenbanken verwenden und eine Migration auf {{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}} durchführen möchten, finden Sie in diesem Thema die entsprechenden Details. Dabei spielt es keine Rolle, wo Ihre {{site.data.keyword.mongodb}}-Datenbanken gehostet werden, solange die Plattform {{site.data.keyword.mongodb}}-Datenbankservices bereitstellt.
{: shortdesc}

## Vorbereitende Schritte
{: #migration_mongodb_before_begin}

Um die {{site.data.keyword.mongodb}}-Befehle zum Durchführen der Migration verwenden zu können, müssen Sie zuerst {{site.data.keyword.mongodb}} auf Ihr System herunterladen und installieren. Weitere Informationen hierzu finden Sie auf der [{{site.data.keyword.mongodb}}-Website](https://docs.mongodb.com/manual/installation/){: external}.

## Schritt 1: Erstellen einer Speicherauszugsdatei zum Wiederherstellen der ursprünglichen Datenbanken
{: #step1_create_dump_file}

Verwenden Sie den folgenden Befehl `mongodump`, um eine Speicherauszugsdatei zu erstellen, die die Datenbanken enthält, die wiederhergestellt werden sollen.

```
mongodump --host "<name_der_replikatgruppe>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <benutzername> --authenticationDatabase <name_der_authentifizierungsdatenbank> --db <datenbankname> --sslCAFile <zertifikatsdatei> --out <speicherauszugsdatei>
```
{: pre}

In der folgenden Tabelle sind die Variablen dargestellt, die im Befehl verwendet werden.

|Variablen|Beschreibung|Beispiel|
|---------|-----------|-------|
|*name_der_replikatgruppe*|Der Name der {{site.data.keyword.mongodb}}-Replikatgruppe. Wenn der Cluster in einer {{site.data.keyword.ihsdbaas_full}} Beta-Instanz implementiert ist, ist dies der Clustername, den Sie bei der Erstellung der Instanz festgelegt haben. Sie finden den Cluster-URI auf der Übersichtsseite der Benutzerschnittstelle des Clusters. Der Name ist der Wert nach `replicaSet=`.|MongoDB-001|
|*hostname1, hostname2, hostname3*|Der Name des Host-Servers, auf dem sich die Replikatgruppe befindet. Es sind drei Replikate verfügbar. Wenn der Cluster in einer {{site.data.keyword.ihsdbaas_full}} Beta-Instanz implementiert ist, finden Sie die Hostnamen im Cluster-URI auf der Übersichtsseite.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|Die Ports für die Verbindung des entsprechenden Hosts. Wenn der Cluster in einer {{site.data.keyword.ihsdbaas_full}} Beta-Instanz implementiert ist, finden Sie die Portnummern im Cluster-URI auf der Übersichtsseite.|28205, 28016, 28175|
|*benutzername*|Der Benutzername für die Authentifizierung bei den ursprünglichen Datenbanken. Der Benutzer muss über Leseberechtigung für die Datenbank verfügen, die migriert werden soll.|mein_benutzer|
|*name_der_authentifizierungsdatenbank*|Die Authentifizierungsdatenbank für den angegebenen Benutzer. Wenn Sie keine Authentifizierungsdatenbank angeben, geht `mongodump` davon aus, dass es sich um die Datenbank handelt, die mit der Option `--db` festgelegt wurde. Wenn auch die Option `--db` nicht angegeben wurde, geht `mongodump` davon aus, dass es sich um die Verwaltungsdatenbank handelt.|admin|
|*datenbankname*|Die Datenbank, die migriert werden soll. Wenn Sie die Datenbank nicht angeben, erstellt `mongodump` Speicherauszüge für alle Datenbanken, die zum Benutzer gehören.|meine_datenbank|
|*zertifikatsdatei*|Die `.pem`-Datei für die Zertifizierungsstelle. Wenn der Cluster in einer {{site.data.keyword.ihsdbaas_full}} Beta-Instanz implementiert ist, können Sie die Zertifikatsdatei von der Übersichtsseite herunterladen. Sie können die Datei mit relativen oder absoluten Pfaden angeben.|./cert.pem|
|*speicherauszugsdatei*|Die `.dump`-Datei, in der die ursprünglichen Daten gespeichert werden. Sie können relative oder absolute Pfade verwenden, um die Datei anzugeben.|./mongo.dump|
{: caption="Tabelle 1. Die Variablen, die zum Erstellen einer Speicherauszugsdatei erforderlich sind."}

Ausführlichere Informationen zum Dienstprogramm `mongodump` finden Sie in der [Dokumentation zu {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}.

Sie können die meisten Informationen aus dem Cluster-URI abrufen. Das Format des URI lautet `mongodb: //<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin? replicaSet=<replica_set_name>`. Sie können den Wert mit dem entsprechenden Feld im vorherigen Befehl `mongodump` abgleichen.
{: tip}

## Schritt 2: Erstellen einer neuen Serviceinstanz in {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #step2_creat_new_service_instance}

Bevor Sie die Daten wiederherstellen, müssen Sie eine neue Serviceinstanz in {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} als Zieldatenbankcluster erstellen. Sie können eine neue Instanz auf eine der folgenden Arten erstellen:
- Über die [Webbenutzerschnittstelle](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service#dbaas_webui_create_service)
- Über die [DBaaS-Manger-APIs](https://{DomainName}/apidocs/hyperp-dbaas#create-an-ibm-cloud-service-instance-of-hyperprote){: external}
- Über das [CLI-Plug-in mit der {{site.data.keyword.cloud_notm}}-CLI](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_create_service)

Beim Erstellen der neuen Serviceinstanz müssen Sie den Clusternamen, den Administratornamen und das Kennwort festlegen. Diese müssen nicht unbedingt mit den Angaben in der ursprünglichen Instanz übereinstimmen. Es hat keine Auswirkungen auf die Migration. Nach Abschluss der Migration werden die Datenbanken der neuen Administrator-/Authentifizierungsdatenbank zugeordnet.

## Schritt 3: Wiederherstellen der Daten aus der Speicherauszugsdatei in die neue Serviceinstanz
{: #step3_restore_data}

Verwenden Sie den Befehl `mongorestore`, um die Daten aus der Speicherauszugsdatei wiederherzustellen, die in [Schritt 1](#step1_create_dump_file) erstellt wurde.

```
mongorestore --host "<name_der_replikatgruppe>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <benutzername> --authenticationDatabase <name_der_authentifizierungsdatenbank> --db <datenbankname> --sslCAFile <zertifikatsdatei> <speicherauszugsdatei>
```
{: pre}

In der folgenden Tabelle sind die Variablen dargestellt, die im Befehl verwendet werden.

|Variablen|Beschreibung|Beispiel|
|---------|-----------|-------|
|*name_der_replikatgruppe*|Der Name der {{site.data.keyword.mongodb}}-Zielreplikatgruppe. Dies ist der Clustername, den Sie beim Erstellen der Instanz festlegen. Sie finden den Cluster-URI auf der Übersichtsseite. Der Name ist der Wert nach `replicaSet=`.|MongoDB-002|
|*hostname1, hostname2, hostname3*|Der DBaaS-Manager-Server, auf dem sich die Zielreplikatgruppe befindet. Es sind drei Replikate verfügbar. Sie finden die Hostnamen im Cluster-URI auf der Übersichtsseiten.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|Die Ports für die Verbindung des entsprechenden Hosts. Sie finden die Ports im Cluster-URI auf der Übersichtsseite.|28128, 28248, 28043|
|*benutzername*|Der Benutzername für die Authentifizierung bei der Zieldatenbank. Der Benutzer muss nicht mit dem Benutzer identisch sein, der die Speicherauszugsdatei erstellt.|neuer_benutzer|
|*name_der_authentifizierungsdatenbank*|Die Authentifizierungsdatenbank für den angegebenen Benutzer. Wenn Sie keine Authentifizierungsdatenbank angeben, geht `mongodump` davon aus, dass es sich um die Datenbank handelt, die mit der Option `--db` festgelegt wurde. Wenn auch die Option `--db` nicht angegeben wurde, geht `mongodump` davon aus, dass es sich um die Verwaltungsdatenbank handelt.|admin|
|*datenbankname*|Die Zieldatenbank, in die die Daten migriert werden sollen. Wenn die Datenbank nicht vorhanden ist, wird sie von `mongorestore` automatisch erstellt. Wenn Sie die Datenbank nicht angeben, erstellt `mongorestore` eine Datenbank auf der Basis der ursprünglichen Datenbank.|meine_datenbank|
|*zertifikatsdatei*|Die `.pem`-Datei für die Zertifizierungsstelle. Sie können die Datei von der Übersichtsseite der Instanz herunterladen und sie mit relativen oder absoluten Pfaden angeben.|./cert.pem|
|*speicherauszugsdatei*|Die `.dump`-Datei, die Sie in [Schritt 1](#step1_create_dump_file) erstellen. Sie können relative oder absolute Pfade verwenden, um die Datei anzugeben.|./mongo.dump|
{: caption="Tabelle 2. Die Variablen, die zum Wiederherstellen der Daten aus einer Speicherauszugsdatei erforderlich sind."}

Für die Erstellung neuer Benutzer stehen die folgenden Möglichkeiten zur Verfügung: die [Webbenutzerschnittstelle](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas-webui-database-users#webui-create-database-user), die [DBaaS-Manager-APIs](https://{DomainName}/apidocs/hyperp-dbaas#create-a-user-in-a-database-cluster){: external}, das [Befehlszeilenschnittstellen-Plug-in mit der {{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_plugin#user_create) und der [{{site.data.keyword.mongodb}}-Befehl](https://docs.mongodb.com/manual/reference/method/db.createUser/index.html){: external}.
{: tip}

Bei {{site.data.keyword.mongodb}} werden bei der Migration die ursprünglichen Daten im Zielcluster mit den vorhandenen Daten zusammengeführt, anstatt diese zu überschreiben. Die [detaillierte Erläuterung](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external} zu diesem Feature auf der {{site.data.keyword.mongodb}}-Website enthält weitere Informationen hierzu. In der [Dokumentation zu {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external} finden Sie ebenfalls Informationen zum Dienstprogramm `mongorestore`.

## Nächste Schritte
{: #whats_next_migration_mongodb}

Nach der Migration können Sie eine Verbindung zum neuen Datenbankcluster herstellen, um zu überprüfen, ob die Daten erfolgreich migriert wurden.

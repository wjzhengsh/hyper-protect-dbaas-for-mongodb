---

copyright:
  years: 2019
lastupdated: "2019-06-12"

keywords: backup, disaster recovery, restore

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:tip: .tip}


# Datenbanken mithilfe von {{site.data.keyword.cos_full_notm}} sichern und wiederherstellen
{: #backup_mongodb_databases}

Der {{site.data.keyword.cloud}}{{site.data.keyword.ihsdbaas_mongodb_full}}-Service löst einmal alle 24 Stunden automatisch eine Sicherung Ihrer gesamten Datenbank aus. Diese verschlüsselten Sicherungen stehen für die letzten 7 Tage zur Verfügung und sind in allen Verfügbarkeitszonen der unterstützten Region redundant verfügbar.
{: shortdesc}

Wenn Sie die Disaster-Recovery-Funktionen erweitern und die Daten außerhalb der unterstützten Region sichern möchten, können Sie den [{{site.data.keyword.cos_full_notm}}-Service](https://cloud.ibm.com/catalog/services/cloud-object-storage){: external} verwenden, um Daten in einer anderen Region zu speichern, indem Sie die nachfolgend beschriebenen Schritte ausführen. 

## Vorbereitende Schritte
{: #backup_mongodb_before_begin}

Um die {{site.data.keyword.mongodb}}-Befehle zum Durchführen der Sicherung zu verwenden, müssen Sie zuerst {{site.data.keyword.mongodb}} auf Ihr System herunterladen und installieren. Weitere Informationen hierzu finden Sie auf der [{{site.data.keyword.mongodb}}-Website](https://docs.mongodb.com/manual/installation/){: external}.

## Schritt 1: Erstellen einer Speicherauszugsdatei zum Sichern der ursprünglichen Datenbanken
{: #step1_create_dump_file_backup_mongodb}

Verwenden Sie den folgenden `mongodump`-Befehl, um eine Speicherauszugsdatei zu erstellen, die die Datenbanken enthält, die gesichert werden sollen.

```
mongodump --host "<name_der_replikatgruppe>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <benutzername> --authenticationDatabase <name_der_authentifizierungsdatenbank> --db <datenbankname> --sslCAFile <zertifikatsdatei> --out <speicherauszugsdatei>
```
{: pre}

In der folgenden Tabelle sind die Variablen dargestellt, die im Befehl verwendet werden.

|Variablen|Beschreibung|Beispiel|
|---------|-----------|-------|
|*name_der_replikatgruppe*|Der Name der {{site.data.keyword.mongodb}}-Replikatgruppe. Dies ist der Clustername, den Sie beim Erstellen der Instanz festlegen. Sie finden den Cluster-URI auf der Übersichtsseite der Benutzerschnittstelle des Clusters. Der Name ist der Wert nach `replicaSet=`.|MongoDB-001|
|*hostname1, hostname2, hostname3*|Der DBaaS-Manager-Server, auf dem sich die Replikatgruppe befindet. Es sind drei Replikate verfügbar. Sie finden die Hostnamen im Cluster-URI auf der Übersichtsseiten.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|Die Ports für die Verbindung des entsprechenden Hosts. Sie finden die Portnummern im Cluster-URI auf der Übersichtsseite.|28205, 28016, 28175|
|*benutzername*|Der Benutzername für die Authentifizierung bei den ursprünglichen Datenbanken. Der Benutzer muss über Leseberechtigung für die Datenbank verfügen, die migriert werden soll.|mein_benutzer|
|*name_der_authentifizierungsdatenbank*|Die Authentifizierungsdatenbank für den angegebenen Benutzer. Wenn Sie keine Authentifizierungsdatenbank angeben, geht `mongodump` davon aus, dass es sich um die Datenbank handelt, die mit der Option `--db` festgelegt wurde. Wenn auch die Option `--db` nicht angegeben wurde, geht `mongodump` davon aus, dass es sich um die Verwaltungsdatenbank handelt.|admin|
|*datenbankname*|Die Datenbank, die gesichert werden soll. Wenn Sie die Datenbank nicht angeben, erstellt `mongodump` Speicherauszüge für alle Datenbanken, die zum Benutzer gehören.|meine_datenbank|
|*zertifikatsdatei*|Die `.pem`-Datei für die Zertifizierungsstelle. Sie können die Zertifikatsdatei von der Übersichtsseite herunterladen und sie mit relativen oder absoluten Pfaden angeben.|./cert.pem|
|*speicherauszugsdatei*|Die `.dump`-Datei, in der die ursprünglichen Daten gespeichert werden. Sie können relative oder absolute Pfade verwenden, um die Datei anzugeben.|./mongo.dump|
{: caption="Tabelle 1. Die Variablen, die zum Erstellen einer Speicherauszugsdatei erforderlich sind."}

Ausführlichere Informationen zum Dienstprogramm `mongodump` finden Sie in der [Dokumentation zu {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}.

Sie können die meisten Informationen aus dem Cluster-URI abrufen. Das Format des URI lautet `mongodb: //<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin? replicaSet=<replica_set_name>`. Sie können den Wert mit dem entsprechenden Feld im vorherigen Befehl `mongodump` abgleichen.
{: tip}

## Schritt 2: Erstellen einer neuen {{site.data.keyword.cos_full_notm}}-Instanz
{: #step2_create_object_storage_backup_mongodb}

Sie müssen die Instanz in einer Region erstellen, bei der es sich nicht um die Region handelt, in der die {{site.data.keyword.ihsdbaas_mongodb_full}}-Serviceinstanz aktuell bereitgestellt wird. Sie können die folgenden Schritte ausführen:

1. [Erstellen Sie eine neue {{site.data.keyword.cos_short}}-Instanz in der Cloud](/docs/services/cloud-object-storage?topic=cloud-object-storage-provision).
2. [Erstellen Sie ein Bucket](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-region) in einer anderen Region.
3. [Erstellen Sie einen Serviceberechtigungsnachweis](/docs/services/cloud-object-storage?topic=cloud-object-storage-service-credentials) und speichern Sie die Werte für `access_key_id` und `secret_access_key` zur späteren Verwendung.
4. Installieren Sie einen S3-Client.
5. Verwenden Sie den S3-Client und die Zugriffsschlüssel, um eine Verbindung zum {{site.data.keyword.cos_short}}-Endpunkt in der Cloud für das Bucket herzustellen, das Sie zuvor erstellt haben. 
6. Verwenden Sie den S3-Client, um die [{{site.data.keyword.mongodb}}-Sicherungsdatei](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload), die Sie in [Schritt 1](#step1_create_dump_file_backup_mongodb) erstellt haben, in das Bucket hochzuladen.

In der [Dokumentation zu {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started) finden Sie weitere Informationen.

Durch die Ausführung dieses Schritts sichern Sie die Daten erfolgreich in einer {{site.data.keyword.cos_short}}-Instanz in der Cloud in einer anderen Region. Wenn Sie die Daten aus der {{site.data.keyword.cos_short}}-Instanz in der Cloud in einer {{site.data.keyword.ihsdbaas_mongodb_full}}-Instanz wiederherstellen möchten, lesen Sie die Details im nachfolgend beschriebenen Schritt 3.

## Schritt 3: Daten aus der {{site.data.keyword.cos_short}}-Instanz in der Cloud in einer {{site.data.keyword.ihsdbaas_mongodb_full}}-Instanz wiederherstellen
{: #step3_restore_data_from_cos_mongodb}

Sie müssen zuerst die Sicherungsdatei aus dem {{site.data.keyword.cos_short}}-Bucket in der Cloud auf Ihre lokale Maschine herunterladen. Anschließend verwenden Sie den Befehl `mongorestore`, um die Daten wiederherzustellen.

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
|*datenbankname*|Die Zieldatenbank, in der die Daten wiederhergestellt werden sollen. Wenn die Datenbank nicht vorhanden ist, wird sie von `mongorestore` automatisch erstellt. Wenn Sie die Datenbank nicht angeben, erstellt `mongorestore` eine Datenbank auf der Basis der ursprünglichen Datenbank.|meine_datenbank|
|*zertifikatsdatei*|Die `.pem`-Datei für die Zertifizierungsstelle. Sie können die Datei von der Übersichtsseite der Instanz herunterladen und sie mit relativen oder absoluten Pfaden angeben.|./cert.pem|
|*speicherauszugsdatei*|Die `.dump`-Datei, die Sie aus dem {{site.data.keyword.cos_short}}-Bucket in der Cloud heruntergeladen haben. Sie können relative oder absolute Pfade verwenden, um die Datei anzugeben.|./mongo.dump|
{: caption="Tabelle 2. Die Variablen, die zum Wiederherstellen der Daten aus einer Speicherauszugsdatei erforderlich sind."}

Bei {{site.data.keyword.mongodb}} werden bei der Migration die ursprünglichen Daten im Zielcluster mit den vorhandenen Daten zusammengeführt, anstatt diese zu überschreiben. Die [detaillierte Erläuterung](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external} zu diesem Feature auf der {{site.data.keyword.mongodb}}-Website enthält weitere Informationen hierzu. In der [Dokumentation zu {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external} finden Sie ebenfalls Informationen zum Dienstprogramm `mongorestore`.

## Nächste Schritte
{: #whats_next_backup_mongodb}

Nach dem Abschluss von [Schritt 3](#step3_restore_data_from_cos_mongodb) können Sie eine Verbindung zum Datenbankcluster herstellen, um zu überprüfen, ob die Daten erfolgreich wiederhergestellt wurden.

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


# Backup e ripristino dei tuoi database tramite {{site.data.keyword.cos_full_notm}}
{: #backup_mongodb_databases}

Il servizio {{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}} attiva automaticamente un backup del tuo intero database ogni 24 ore. Questi backup crittografati sono disponibili per gli ultimi 7 giorni e in modo ridondante sull'archiviazione locale in tutte le zone di disponibilità della regione supportata.
{: shortdesc}

Se vuoi aumentare le tue funzionalità di ripristino di emergenza e backup dei dati al di fuori della regione supportata, puoi utilizzare il servizio [{{site.data.keyword.cos_full_notm}}](https://cloud.ibm.com/catalog/services/cloud-object-storage){: external} per archiviare i tuoi dati in una regione diversa facendo riferimento alla seguente procedura.

## Prima di cominciare
{: #backup_mongodb_before_begin}

Per utilizzare i comandi {{site.data.keyword.mongodb}} in modo da completare il backup, devi prima scaricare e installare {{site.data.keyword.mongodb}} sulla tua macchina. Per ulteriori informazioni, vedi il [sito web {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/installation/){: external}.

## Passo 1: crea un file di dump per eseguire il backup dei database originali
{: #step1_create_dump_file_backup_mongodb}

Utilizza il seguente comando `mongodump` per creare un file di dump che contiene i database di cui vuoi eseguire il backup.

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_file>
```
{: pre}

La seguente tabella illustra le variabili utilizzate nel comando.

|Variabili|Descrizione|Esempio|
|---------|-----------|-------|
|*replica_set_name*|Il nome della serie di repliche {{site.data.keyword.mongodb}}. Questo è il nome del cluster che hai impostato durante la creazione dell'istanza. Puoi trovare l'URI del cluster sulla pagina della panoramica dell'IU del cluster. Il nome è il valore dopo `replicaSet=`.|MongoDB-001|
|*host_name1, host_name2, host_name3*|Il server DBaaS manager che ospita la serie di repliche. Sono disponibili tre repliche. Puoi trovare i nomi host nell'URI del cluster sulla pagina della panoramica.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|Le porte per la connessione all'host corrispondente. Puoi trovare i numeri di porta nell'URI del cluster sulla pagina della panoramica.|28205, 28016, 28175|
|*user_name*|Il nome utente per l'autenticazione ai database originali. L'utente deve disporre del privilegio READ sul database che vuoi migrare.|my_user|
|*authentication_database_name*|Il database di autenticazione per l'utente specificato. Se non specifichi un database di autenticazione, `mongodump` assume che sia il database che hai impostato nell'opzione `--db`. Se anche l'opzione `--db` non viene specificata, `mongodump` assume che sia il database dell'amministratore.|admin|
|*database_name*|Il database di cui vuoi eseguire il backup. Se non specifichi il database, `mongodump` esegue il dump di tutti i database che appartengono all'utente.|my_database|
|*cert_file*|Il file `.pem` dell'autorità di certificazione. Puoi scaricare il file del certificato dalla pagina della panoramica e specificarlo utilizzando dei percorsi relativi o assoluti.|./cert.pem|
|*dump_file*|Il file `.dump` per archiviare i dati originali. Puoi utilizzare dei percorsi relativi o assoluti per specificare il file.|./mongo.dump|
{: caption="Tabella 1. Le variabili necessarie per creare un file di dump"}

Se hai bisogno di informazioni più dettagliate sul programma di utilità `mongodump`, puoi fare riferimento alla [documentazione {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}.

È possibile ottenere la maggior parte delle informazioni dall'URI del cluster. Il formato dell'URI è `mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>`. Puoi far corrispondere il valore al campo corrispondente nel precedente comando `mongodump`.
{: tip}

## Passo 2: crea una nuova istanza di {{site.data.keyword.cos_full_notm}}
{: #step2_create_object_storage_backup_mongodb}

Devi creare l'istanza in una regione diversa rispetto a quella in cui al momento è ospitata l'istanza del servizio {{site.data.keyword.ihsdbaas_mongodb_full}}. Puoi fare riferimento alla seguente procedura:

1. [Crea una nuova istanza {{site.data.keyword.cos_short}} cloud](/docs/services/cloud-object-storage?topic=cloud-object-storage-provision).
2. [Crea un bucket](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-region) in una regione diversa.
3. [Crea le credenziali del servizio](/docs/services/cloud-object-storage?topic=cloud-object-storage-service-credentials) e salva `access_key_id` e `secret_access_key` per un utilizzo successivo.
4. Installa un client S3.
5. Utilizza il client S3 e le chiavi di accesso per la connessione all'endpoint {{site.data.keyword.cos_short}} cloud del bucket che hai creato in precedenza.
6. Utilizza il client S3 per [caricare il file di backup di {{site.data.keyword.mongodb}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) che hai creato nel [Passo 1](#step1_create_dump_file_backup_mongodb) nel bucket.

Per ulteriori informazioni, puoi fare riferimento alla [documentazione {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started).

Dopo aver completato questo passo, avrai correttamente eseguito il backup dei tuoi dati in un'istanza {{site.data.keyword.cos_short}} cloud in una regione diversa. Se vuoi ripristinare i dati dall'istanza {{site.data.keyword.cos_short}} cloud in un'istanza {{site.data.keyword.ihsdbaas_mongodb_full}}, leggi il seguente passo 3 per i dettagli.

## Passo 3: ripristina i dati dall'istanza {{site.data.keyword.cos_short}} cloud a un'istanza {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #step3_restore_data_from_cos_mongodb}

Devi prima caricare il file di backup dal bucket {{site.data.keyword.cos_short}} cloud nella tua macchina locale e poi utilizzare il comando `mongorestore` per ripristinare i dati.

```
mongorestore --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> <dump_file>
```
{: pre}

La seguente tabella illustra le variabili utilizzate nel comando.

|Variabili|Descrizione|Esempio|
|---------|-----------|-------|
|*replica_set_name*|Il nome della serie di repliche {{site.data.keyword.mongodb}} di destinazione. Questo è il nome del cluster che hai impostato durante la creazione dell'istanza. Puoi trovare l'URI del cluster sulla pagina della panoramica. Il nome è il valore dopo `replicaSet=`.|MongoDB-002|
|*host_name1, host_name2, host_name3*|Il server DBaaS manager che ospita la serie di repliche di destinazione. Sono disponibili tre repliche. Puoi trovare i nomi host nell'URI del cluster sulla pagina della panoramica.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|Le porte per la connessione all'host corrispondente. Puoi trovare le porte nell'URI del cluster sulla pagina della panoramica.|28128, 28248, 28043|
|*user_name*|Il nome utente per l'autenticazione al database di destinazione. L'utente non deve essere lo stesso di quello che crea il file di dump.|new_user|
|*authentication_database_name*|Il database di autenticazione per l'utente specificato. Se non specifichi un database di autenticazione, `mongodump` assume che sia il database che hai impostato nell'opzione `--db`. Se anche l'opzione `--db` non viene specificata, `mongodump` assume che sia il database dell'amministratore.|admin|
|*database_name*|Il database di destinazione in cui vuoi ripristinare i dati. Se il database non esiste, `mongorestore` lo crea automaticamente. Se non specifichi il database, `mongorestore` crea un database basato sul database originale.|my_database|
|*cert_file*|Il file `.pem` dell'autorità di certificazione. Puoi scaricare il file dalla pagina della panoramica dell'istanza e specificarlo utilizzando dei percorsi relativi o assoluti.|./cert.pem|
|*dump_file*|Il file `.dump` che hai scaricato dal tuo bucket {{site.data.keyword.cos_short}} cloud. Puoi utilizzare dei percorsi relativi o assoluti per specificare il file.|./mongo.dump|
{: caption="Tabella 2. Le variabili necessarie per ripristinare i dati da un file di dump"}

Per {{site.data.keyword.mongodb}}, la migrazione unisce i dati originali nel cluster di destinazione invece di sovrascrivere i dati esistenti se presenti. Puoi fare riferimento alla [spiegazione dettagliata](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external} di questa funzione nel sito web {{site.data.keyword.mongodb}}. Puoi anche vedere la [documentazione {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external} per ulteriori informazioni sul programma di utilità `mongorestore`.

## Operazioni successive
{: #whats_next_backup_mongodb}

Dopo aver completato il [Passo 3](#step3_restore_data_from_cos_mongodb), puoi eseguire la connessione al cluster di database per controllare se i dati sono stati ripristinati correttamente.

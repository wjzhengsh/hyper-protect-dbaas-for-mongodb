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

# Migrazione dei database {{site.data.keyword.mongodb}} a {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #migration_mongodb}

Se stai utilizzando i database {{site.data.keyword.mongodb}} e vuoi eseguire la migrazione a {{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}}, leggi questo argomento per i dettagli. Non importa dove sono ubicati i tuoi database {{site.data.keyword.mongodb}}, finché la piattaforma fornisce dei servizi database {{site.data.keyword.mongodb}}.
{: shortdesc}

## Prima di cominciare
{: #migration_mongodb_before_begin}

Per utilizzare i comandi {{site.data.keyword.mongodb}} in modo da completare la migrazione, devi prima scaricare e installare {{site.data.keyword.mongodb}} sulla tua macchina. Per ulteriori informazioni, vedi il [sito web {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/installation/){: external}.

## Passo 1: crea un file di dump per ripristinare i database originali
{: #step1_create_dump_file}

Utilizza il seguente comando `mongodump` per creare un file di dump che contiene i database che vuoi ripristinare.

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_file>
```
{: pre}

La seguente tabella illustra le variabili utilizzate nel comando.

|Variabili|Descrizione|Esempio|
|---------|-----------|-------|
|*replica_set_name*|Il nome della serie di repliche {{site.data.keyword.mongodb}}. Se il cluster viene distribuito in un'istanza {{site.data.keyword.ihsdbaas_full}} Beta, questo è il nome del cluster impostato quando hai creato l'istanza. Puoi trovare l'URI del cluster sulla pagina della panoramica dell'IU del cluster. Il nome è il valore dopo `replicaSet=`.|MongoDB-001|
|*host_name1, host_name2, host_name3*|Il nome del server host che ospita la serie di repliche. Sono disponibili tre repliche. Se il cluster viene distribuito in un'istanza {{site.data.keyword.ihsdbaas_full}} Beta, puoi trovare i nomi host nell'URI del cluster sulla pagina della panoramica.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|Le porte per la connessione all'host corrispondente. Se il cluster viene distribuito in un'istanza {{site.data.keyword.ihsdbaas_full}} Beta, puoi trovare i numeri di porta nell'URI del cluster sulla pagina della panoramica.|28205, 28016, 28175|
|*user_name*|Il nome utente per l'autenticazione ai database originali. L'utente deve disporre del privilegio READ sul database che vuoi migrare.|my_user|
|*authentication_database_name*|Il database di autenticazione per l'utente specificato. Se non specifichi un database di autenticazione, `mongodump` assume che sia il database che hai impostato nell'opzione `--db`. Se anche l'opzione `--db` non viene specificata, `mongodump` assume che sia il database dell'amministratore.|admin|
|*database_name*|Il database che vuoi migrare. Se non specifichi il database, `mongodump` esegue il dump di tutti i database che appartengono all'utente.|my_database|
|*cert_file*|Il file `.pem` dell'autorità di certificazione. Se il cluster viene distribuito in un'istanza {{site.data.keyword.ihsdbaas_full}} Beta, puoi scaricare il file del certificato dalla pagina della panoramica. Puoi specificare il file utilizzando dei percorsi relativi o assoluti.|./cert.pem|
|*dump_file*|Il file `.dump` per archiviare i dati originali. Puoi utilizzare dei percorsi relativi o assoluti per specificare il file.|./mongo.dump|
{: caption="Tabella 1. Le variabili necessarie per creare un file di dump"}

Se hai bisogno di informazioni più dettagliate sul programma di utilità `mongodump`, puoi fare riferimento alla [documentazione {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}.

È possibile ottenere la maggior parte delle informazioni dall'URI del cluster. Il formato dell'URI è `mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>`. Puoi far corrispondere il valore al campo corrispondente nel precedente comando `mongodump`.
{: tip}

## Passo 2: crea una nuova istanza del servizio in {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #step2_creat_new_service_instance}

Prima di ripristinare i dati, devi creare una nuova istanza del servizio in {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} come il cluster di database di destinazione. Puoi utilizzare uno dei seguenti modi per creare una nuova istanza:
- [L'interfaccia utente web](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service#dbaas_webui_create_service)
- [Le API DBaaS Manger](https://{DomainName}/apidocs/hyperp-dbaas#create-an-ibm-cloud-service-instance-of-hyperprote){: external}
- [Il plugin CLI con la CLI {{site.data.keyword.cloud_notm}}](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_create_service)

Quando crei la nuova istanza del servizio, devi impostare il nome del cluster, il nome e la password dell'amministratore. Non devono necessariamente essere gli stessi di quelli nell'istanza originale. Non influisce sulla migrazione. Dopo aver completato la migrazione, i database vengono assegnati al nuovo database di autenticazione/amministratore.

## Passo 3: ripristina i dati dal file di dump alla nuova istanza del servizio
{: #step3_restore_data}

Utilizza il comando `mongorestore` per ripristinare i dati dal file di dump creato nel [Passo 1](#step1_create_dump_file).

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
|*database_name*|Il database di destinazione in cui vuoi migrare i dati. Se il database non esiste, `mongorestore` lo crea automaticamente. Se non specifichi il database, `mongorestore` crea un database basato sul database originale.|my_database|
|*cert_file*|Il file `.pem` dell'autorità di certificazione. Puoi scaricare il file dalla pagina della panoramica dell'istanza e specificarlo utilizzando dei percorsi relativi o assoluti.|./cert.pem|
|*dump_file*|Il file `.dump` che hai creato nel [Passo 1](#step1_create_dump_file). Puoi utilizzare dei percorsi relativi o assoluti per specificare il file.|./mongo.dump|
{: caption="Tabella 2. Le variabili necessarie per ripristinare i dati da un file di dump"}

Puoi utilizzare i seguenti modi per creare dei nuovi utenti: [L'interfaccia utente web](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas-webui-database-users#webui-create-database-user), [Le API DBaaS Manger](https://{DomainName}/apidocs/hyperp-dbaas#create-a-user-in-a-database-cluster){: external},[Il plugin CLI con la CLI {{site.data.keyword.cloud_notm}}](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_plugin#user_create) e [Il comando {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/method/db.createUser/index.html){: external}.
{: tip}

Per {{site.data.keyword.mongodb}}, la migrazione unisce i dati originali nel cluster di destinazione invece di sovrascrivere i dati esistenti se presenti. Puoi fare riferimento alla [spiegazione dettagliata](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external} di questa funzione nel sito web {{site.data.keyword.mongodb}}. Puoi anche vedere la [documentazione {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external} per ulteriori informazioni sul programma di utilità `mongorestore`.

## Operazioni successive
{: #whats_next_migration_mongodb}

Dopo la migrazione, puoi eseguire la connessione al nuovo cluster di database per controllare se i dati sono stati migrati correttamente.

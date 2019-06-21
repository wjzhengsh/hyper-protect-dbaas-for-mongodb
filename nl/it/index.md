---

copyright:
  years: 2019
lastupdated: "2019-06-10"

keywords: database cluster, Data security, database instance

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}

# Introduzione a {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #gettingstarted}

{{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}} è un servizio {{site.data.keyword.cloud_notm}} che fornisce dei database MongoDB su richiesta. Offre una piattaforma flessibile e scalabile che
ti permette di eseguire velocemente e facilmente il provisioning e di gestire il database di tua scelta.
{: shortdesc}

Questa offerta {{site.data.keyword.cloud_notm}} fornisce dei cluster di database MongoDB. Ogni cluster di database
è formato da un'istanza del database primaria e da due repliche dell'istanza del database che eseguono
il backup della primaria.

Con {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}},
puoi creare dei cluster di database in {{site.data.keyword.cloud_notm}}, gestire le istanze del
database, gestire gli utenti del database, creare e monitorare i database.

## Sicurezza e riservatezza dei dati
{: #data-security-and-privacy}

{{site.data.keyword.IBM_notm}} ospita i database in un ambiente altamente disponibile e sicuro:
<ul>
<li>Le tecnologie sottostanti impediscono {{site.data.keyword.IBM_notm}} o a una terza parte di poter
accedere ai tuoi dati.
<p>La tecnologia {{site.data.keyword.IBM_notm}} SSC (Secure Service Container)
protegge il sistema tramite un ambiente a prova di manomissione. L'accesso al sistema è limitato
e viene abilitato solo tramite delle API RESTful ben definite.</p></li>
<li>I dati sono crittografati sia quando sono inattivi che quando sono in transito.</li>
<li>L'hardware del sistema e la configurazione del sistema e del database garantiscono un'elevata disponibilità.</li>
</ul>

<!--
For more information, watch:

- [Data security and privacy using {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} - English version](https://www.youtube.com/watch?v=__IBP727IL8){: external}
- [Data security and privacy using {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} - Chinese version](https://v.youku.com/v_show/id_XMzc3ODQzMzYwMA==.html){: external}
-->

## Creazione di un cluster di database
{: #creating-a-database-cluster-introduction}

Per creare un cluster di database, immetti semplicemente i valori richiesti nella schermata di configurazione del servizio
{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} e fai clic su **Create**.

Dopo aver creato il cluster di database, {{site.data.keyword.IBM_notm}} fornisci
uno o più nomi host e numeri di porta delle istanze del database create. Puoi ora utilizzare queste informazioni
e le credenziali utente che hai specificato nel catalogo per creare e accedere ai
tuoi database.

## Gestione del cluster di database
{: #managing-database-cluster-introduction}

In un cluster di database, puoi creare i database, gestire le istanze del database e creare o eliminare gli utenti.

{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} contiene un DBaaS Manager, che gestisce e pianifica
in modo intelligente le tue richieste in base alle risorse disponibili.

Puoi indirizzare le richieste al DBaaS Manager tramite una di queste interfacce:

- L'[Interfaccia utente web](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service)
- Le [API DBaaS Manager (per la gestione del cluster di database)](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-gen_inst_mgr_apis)
- Le [API {{site.data.keyword.cloud_notm}} (per la gestione dell'istanza del servizio)](https://{DomainName}/apidocs/hyperp-dbaas){: external}
- Il plugin CLI [ con lo strumento CLI {{site.data.keyword.cloud_notm}}](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-install-ibm-cli)

## Accesso al database
{: #accessing-database-introduction}

Dopo aver creato un database MongoDB, puoi utilizzare la shell mongo, il tuo driver MongoDB preferito o degli strumenti come MongoDB Compass per gestire il database.

### Prima di cominciare
{: #accessing-database-introduction-byb}

Per garantire un trasferimento dei dati sicuro, ottieni un file dell'autorità di certificazione (o CA, certificate authority) dal dashboard e copialo nella directory appropriata.

### Connessione a un database
{: #accessing-database-introduction-connect}

Il dashboard {{site.data.keyword.ihsdbaas_mongodb_full}} fornisce le informazioni necessarie per la connessione a un database.

#### mongo shell
{: #accessing-database-introduction-connect-mongoshell}

Puoi eseguire il comando mongo shell fornito nel dashboard {{site.data.keyword.ihsdbaas_mongodb_full}}:

1. Fai clic sull'icona accanto al comando per copiarlo negli appunti.
2. Se la connessione dati sicura ha esito negativo con un errore SSL, specifica il file CA ottenuto per convalidare il certificato del server. Aggiungi il parametro `--sslCAFile` per indicare il file CA.

Vedi il seguente esempio:

<pre><code class="hljs"># mongo 'mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,\
&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,\
&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin?replicaSet=&lt;<em>replicaSetName</em>&gt;' \
--ssl --username &lt;<em>Username</em>&gt; --password &lt;<em>Password</em>&gt; --sslCAFile &lt;<em>CAFile</em>&gt;</code></pre>

Dove:
<dl>
  <dt> &lt;<em>Hostname_i</em>&gt; </dt>
    <dd> È il nome host della replica del database (<em>i=1,2,3</em>) </dd>
  <dt> &lt;<em>PortNumber_i</em>&gt; </dt>
    <dd> È il numero di porta della replica del database (<em>i=1,2,3</em>) </dd>
  <dt> &lt;<em>replicaSetName</em>&gt; </dt>
    <dd> È il nome della tua serie di repliche restituita come specificato nel dashboard {{site.data.keyword.ihsdbaas_mongodb_full}} </dd>
  <dt> &lt;<em>Username</em>&gt; </dt>
    <dd> È il nome utente per l'amministratore del database come specificato nella schermata di creazione del servizio </dd>
  <dt> &lt;<em>Password</em>&gt; </dt>
    <dd> È la password per l'amministratore del database come specificato nella schermata di creazione del servizio </dd>
  <dt> &lt;<em>CAFile</em>&gt; </dt>
    <dd> È il file CA cert.pem </dd>
</dl>


### Altri strumenti
{: #accessing-database-introduction-connect-other}

Per altri strumenti, come ad esempio MongoDB Compass, {{site.data.keyword.ihsdbaas_mongodb_full}} supporta la *convalida del certificato server SSL* per la connessione all'host. Se necessario, utilizza il file CA fornito.

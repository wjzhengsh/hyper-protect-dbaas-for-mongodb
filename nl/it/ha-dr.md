---

copyright:
  years: 2019
lastupdated: "2019-06-12"

keywords: high availability disaster recovery

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

# Considerazioni generali sull'alta disponibilità e il ripristino d'emergenza 
{: #ha-dr}

Tutte le offerte {{site.data.keyword.cloud_notm}} generalmente disponibili (GA) hanno uno SLA (Service Level Agreement) del 99.95% di disponibilità. {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} è un servizio GA offerto in Dallas. Ogni ubicazione ha tre diversi data center per la ridondanza.
{: shortdesc}

{{site.data.keyword.IBM_notm}} ospita i database in un ambiente altamente disponibile e sicuro. La tecnologia {{site.data.keyword.IBM_notm}} SSC (Secure Service Container)
protegge il sistema tramite un ambiente a prova di manomissione. {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} fornisce la crittografia dei dati sia quando sono inattivi che quando sono in transito. Poiché né {{site.data.keyword.IBM_notm}} né un servizio di terze parti può accedere ai tuoi dati, è essenziale una politica di backup e ripristino per i tuoi database. 

Vedi [](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-backup_mongodb_databases)Backup e ripristino dei tuoi database utilizzando IBM Cloud Object Storage
e [Ripristino dei tuoi database da parte del supporto IBM](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-restore_mongodb_databases)
per ulteriori informazioni su come eseguire il backup e il ripristino dei tuoi database.

Vedi [Come garantisco nessun tempo di inattività?](/docs/overview?topic=overview-zero-downtime#zero-downtime) per ulteriori informazioni sugli standard di alta disponibilità e ripristino di emergenza in {{site.data.keyword.cloud_notm}}. Puoi anche trovare informazioni sugli [SLA (Service Level Agreement)](/docs/overview?topic=overview-zero-downtime#SLAs). 

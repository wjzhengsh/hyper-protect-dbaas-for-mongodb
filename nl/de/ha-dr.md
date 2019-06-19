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

# Hinweise zur allgemeinen Hochverfügbarkeit und Disaster-Recovery
{: #ha-dr}

Alle GA-Angebote (GA = General Availability, allgemeine Verfügbarkeit) von {{site.data.keyword.cloud_notm}} weisen eine im Rahmen des Service Level Agreement festgelegte Verfügbarkeit von 99,95 % auf. {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} ist ein GA-Service, der in Dallas angeboten wird. Jeder Standort verfügt aus Redundanzgründen über drei verschiedene Rechenzentren.
{: shortdesc}

{{site.data.keyword.IBM_notm}} stellt Datenbanken in einer hoch verfügbaren und sicheren Umgebung per Hosting bereit. Die {{site.data.keyword.IBM_notm}} Secure Service Container-Technologie schützt das System über eine manipulationssichere Umgebung. {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} bietet Funktionen zur Verschlüsselung von ruhenden und übertragenen Daten. Da weder {{site.data.keyword.IBM_notm}} noch Dritte auf Ihre Daten zugreifen können, ist eine Strategie zur Sicherung und Wiederherstellung Ihrer Datenbanken von zentraler Bedeutung. 

[Datenbanken mithilfe von IBM Cloud Object Storage sichern und wiederherstellen](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-backup_mongodb_databases) und
[Datenbanken mithilfe des IBM Support wiederherstellen](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-restore_mongodb_databases) enthalten
Informationen zur Sicherung und Wiederherstellung Ihrer Datenbanken. 

In [Wie kann sichergestellt werden, dass keine Ausfallzeiten auftreten?](/docs/overview?topic=overview-zero-downtime#zero-downtime) finden Sie Informationen zu den Hochverfügbarkeits- und Disaster-Recovery-Standards in {{site.data.keyword.cloud_notm}}. Dort finden Sie auch Informationen zu [Service Level Agreements](/docs/overview?topic=overview-zero-downtime#SLAs).

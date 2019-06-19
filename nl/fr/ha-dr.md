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

# Considérations relatives à la disponibilité générale et la reprise après incident
{: #ha-dr}

Toutes les offres {{site.data.keyword.cloud_notm}} de disponibilité générale ont un contrat de niveau de service d'une disponibilité de 99,95%. {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} est un service de disponibilité générale proposé à Dallas. Chaque emplacement comporte 3 centres de données différents pour la redondance.
{: shortdesc}

{{site.data.keyword.IBM_notm}} héberge vos bases de données dans un environnement hautement disponible et sécurisé. La technologie {{site.data.keyword.IBM_notm}} Secure Service Container protège le système via un environnement infalsifiable. {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} fournit un chiffrement de données au repos et en cours d'exploitation. Etant donné que ni {{site.data.keyword.IBM_notm}} ni un tiers ne peut accéder à vos données, une règle de sauvegarde et de restauration de vos bases de données est essentielle.

Voir [Sauvegarde et restauration de vos bases de données à l'aide d'IBM Cloud Object Storage](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-backup_mongodb_databases)
et [Restauration de vos bases de données par le support IBM](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-restore_mongodb_databases) pour apprendre à sauvegarder et restaurer vos bases de données.

Voir [Comment garantir une disponibilité permanente ?](/docs/overview?topic=overview-zero-downtime#zero-downtime) pour en savoir plus sur les normes relatives à la haute disponibilité et à la reprise après incident dans {{site.data.keyword.cloud_notm}}. Vous pouvez également consultez les informations sur les [Accords sur les niveaux de service (SLA)](/docs/overview?topic=overview-zero-downtime#SLAs).

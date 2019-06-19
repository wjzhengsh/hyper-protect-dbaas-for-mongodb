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

# Consideraciones sobre la recuperación tras desastre y alta disponibilidad
{: #ha-dr}

Todas las ofertas de disponibilidad general (GA) de {{site.data.keyword.cloud_notm}} tienen un Acuerdo de nivel de servicio (SLA) de disponibilidad del 99,95%. {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} es un servicio de disponibilidad general que se ofrece en Dallas. Cada ubicación tiene tres centros de datos distintos para redundancia.
{: shortdesc}

{{site.data.keyword.IBM_notm}} aloja las bases de datos en un entorno de alta disponibilidad y seguridad. La tecnología Secure Service Container de {{site.data.keyword.IBM_notm}} protege el sistema a través de un entorno a prueba de manipulaciones. {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} proporciona cifrado de datos en reposo y en curso. Ya que ni {{site.data.keyword.IBM_notm}} ni un tercero pueden acceder a sus datos, es esencial tener una política de copia de seguridad y restauración de sus bases de datos.

Consulte [Copia de seguridad y restauración de sus bases de datos utilizando IBM Cloud Object Storage](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-backup_mongodb_databases) y [Restauración de sus bases de datos por parte del equipo de soporte de IBM](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-restore_mongodb_databases) para aprender cómo hacer una copia de seguridad de sus bases de datos y restaurarlas.

Consulte [¿Cómo puedo asegurar un tiempo de inactividad cero?](/docs/overview?topic=overview-zero-downtime#zero-downtime) para obtener más información sobre los estándares de alta disponibilidad y recuperación tras desastre en {{site.data.keyword.cloud_notm}}. También puede encontrar información sobre [Acuerdos de nivel de servicio (SLA)](/docs/overview?topic=overview-zero-downtime#SLAs).

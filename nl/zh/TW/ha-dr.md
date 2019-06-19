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

# 一般高可用性及災難回復考量
{: #ha-dr}

所有 {{site.data.keyword.cloud_notm}} 通用版 (GA) 供應項目都有「服務水準合約」的 99.95% 可用性。{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} 是達拉斯所提供的 GA 服務。每個位置都有三個不同的資料中心以供備援。
{: shortdesc}

{{site.data.keyword.IBM_notm}} 在高可用性的安全環境中管理您的資料庫。{{site.data.keyword.IBM_notm}} Secure Service Container 技術透過防篡改環境來保護系統。{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} 提供靜態及進行中資料加密。因為 {{site.data.keyword.IBM_notm}} 及協力廠商都無法存取您的資料，所以資料庫的備份及還原原則十分重要。

請參閱[使用 IBM Cloud Object Storage 備份及還原資料庫](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-backup_mongodb_databases)及[由 IBM 支援中心還原資料庫](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-restore_mongodb_databases)，以瞭解如何備份及還原資料庫。

請參閱[如何確保運作零中斷？](/docs/overview?topic=overview-zero-downtime#zero-downtime)，進一步瞭解 {{site.data.keyword.cloud_notm}} 中的高可用性及災難回復標準。您也可以找到[服務水準合約](/docs/overview?topic=overview-zero-downtime#SLAs)的相關資訊。
